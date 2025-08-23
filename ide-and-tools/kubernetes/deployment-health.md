
## readinessProbe and livenessProbe

### Overview
The readinessProbe is used to determine if your application is ready to receive traffic. It typically checks if the application is running and able to serve requests (e.g., HTTP 200 on a health endpoint).
Including a DB check can cause issues if the database is temporarily unavailable, as Kubernetes will mark your pod as "not ready" and remove it from service, even if your app itself is healthy.
It's best practice to keep the readinessProbe lightweight and only check the application's own health, not dependencies like databases.

- livenessProbe
  - Question: "Is the application process alive and functioning?"
  - Purpose: Determines if the container is alive and functioning
  - Action when it fails: Kubernetes restarts the container
  - Your example: Checks /api/status endpoint to see if the application is responding
  - Use case:
    - Application is in an infinite loop
    - Process is deadlocked
    - Application crashed but container is still running
    - Memory leak causing unresponsiveness

- readinessProbe
  - Question: "Is the application ready to handle incoming requests?"
  - Purpose: Determines if the container is ready to receive traffic
  - Action when it fails: Kubernetes removes the pod from service (stops sending traffic to it, but doesn't restart it)
  - Your example: Checks Azure Managed Identity token retrieval for database access
  - Use case:
    - Application is starting up and loading configuration
    - Warming up caches
    - Waiting for dependencies to be available
    - Temporarily overloaded but not crashed

* comparison

Aspect	| livenessProbe	| readinessProbe
When it runs	| Throughout pod lifecycle	| Throughout pod lifecycle
Failure Action	| Kill/Restart container	| Remove from service (Stop sending traffic)
Recovery | Fresh start (restart)	Automatic when probe passes
Traffic	| No direct impact	| Controls load balancer traffic
Startup	| Runs after container starts	| Runs during and after startup
Purpose	| Health check	| Traffic eligibility

> How Readiness Probe Works
> - Kubernetes uses internal networking to call your pod directly
> - Your application decides if it's ready for external traffic
> - Kubernetes controls external traffic routing based on the probe result

* reference : Traffic Flow
- External Traffic Flow:
Internet → LoadBalancer → Service → Pod

- Probe Traffic Flow:
Kubernetes Control Plane → Pod (direct internal connection)


### Best Practice Solution
Let your application handle the database readiness check rather than direct "exec" commmand :

Benefits:
- Your app controls retry logic and timeouts
- Can implement circuit breakers for resilience
- Handles Azure AD token refresh internally
- No external command dependencies
- Better error handling and logging

```
readinessProbe:
  httpGet:
    path: /health/ready  # Check if ready for traffic
    port: 5001
  initialDelaySeconds: 20
  periodSeconds: 10
  timeoutSeconds: 5
livenessProbe:
  httpGet:
    path: /health/live   # Check if process is alive
    port: 5001
  initialDelaySeconds: 30
  periodSeconds: 30
  timeoutSeconds: 10
```
> Probe timeline for new pod
> - Time 0s:     Pod starts, no probes running
> - Time 20s:    First readinessProbe + livenessProbe checks
> - Time 80s:    Second checks (20s + 60s)
> - Time 140s:   Third checks, and so on...

### How Kubenetes Load Balancing Works? 
The load balancer (actually the Kubernetes Service) will automatically route traffic to healthy replicas when some pods have failing readiness probes but passing liveness probes.
Automatic Traffic Management: Load balancer routes traffic only to healthy pods
No Manual Intervention: Kubernetes handles endpoint updates automatically
Graceful Degradation: Service capacity reduces but remains available
Fast Recovery: When database issues resolve, pods automatically become ready
Better User Experience: Users get responses from healthy pods instead of database errors

- Service Endpoint Management : (e.g. 3 replicas)
  - Pod 1: livenessProbe=OK, readinessProbe=FAIL → Removed from Service endpoints
  - Pod 2: livenessProbe=OK, readinessProbe=OK   → Added to Service endpoints  
  - Pod 3: livenessProbe=OK, readinessProbe=OK   → Added to Service endpoints

- Traffic Routing:
  - Load balancer only sends traffic to pods in Service endpoints
  - Kubernetes Service automatically updates endpoints based on readiness probe results
  - No manual intervention required

- Error behaviour scenarios (e.g. db error) :
  - Pod 1: db exception → readinessProbe=FAIL (checks DB) → Removed from service → No traffic
  - Pod 2: Healthy → readinessProbe=PASS → Receives traffic → Users get responses
  - Pod 3: Healthy → readinessProbe=PASS → Receives traffic → Users get responses

> Load Balancer Traffic Switching: the load balancer (Kubernetes Service) can switch traffic to other replicas, but the behavior depends on which probe fails:
> - Scenario 1: Readiness Probe Fails First
>   ```
>   # If you optimize readiness probe to check more frequently:
>   readinessProbe:
>     periodSeconds: 15        # Check every 15 seconds
>     failureThreshold: 2      # Only 2 failures needed
>   # Fails after 30 seconds (15s + 15s)
>   livenessProbe:
>     periodSeconds: 60        # Check every 60 seconds
>     failureThreshold: 3      # 3 failures needed
>   # Fails after 180 seconds (60s + 60s + 60s)
>   ```
>   - 30s: Pod removed from service (readiness fails)
>   - 180s: Pod gets restarted (liveness fails)
>   - Traffic switches to other replicas immediately at 30s
>
> - Scenario 2: Liveness Probe Fails First
>   ```
>   # If you optimize readiness probe to check more frequently:
>   readinessProbe:
>     periodSeconds: 30        # Check every 30 seconds
>     failureThreshold: 2      # Only 2 failures needed
>   # Fails after 30 seconds (15s + 15s)
>   livenessProbe:
>     periodSeconds: 60        # Check every 60 seconds
>     failureThreshold: 3      # 3 failures needed
>   # Fails after 180 seconds (60s + 60s + 60s)
>   ```
>   - 60s: Pod gets restarted (liveness fails)
>   - New pod starts: Fresh readiness/liveness probe cycle begins
>   - Traffic switches during restart
>     
> Best Practice Timeline:
> - Optimize readiness probe to fail fast (15s intervals, 2 failures = 30s total)
> - Keep liveness probe more tolerant (30s intervals, 3 failures = 90s total)
> - Result: Traffic switches in 30s, pod restarts only if truly unhealthy
>   
> Key Benefits:
> - Fast traffic switching: Unhealthy pods removed from service quickly
> - Intelligent restarts: Only restart pods that are truly broken
> - High availability: Load balancer ensures traffic goes to healthy replicas
> - Azure-optimized: Proper handling of database connection pool issues



