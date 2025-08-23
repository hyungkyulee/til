# Process issues on Macos

### Port 5000 is in-use always
#### Background 
I was trying to launch my webapp backen server on port 5000, though. it's on error because the port 5000 is in-use. 
Even though I kill the process, it's coming again like zombie. 

#### What I've learned... and solution
It seams that airplay is on a listening mode on a latest os upgrade. 
after turning of the airplay, it's working now.. yeh~~~!!!

```
 ~  lsof -n -i TCP:5000                                            ✔  16:17:10
COMMAND   PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
ControlCe 466  kyu   33u  IPv4 0x304e6a560b860de1      0t0  TCP *:commplex-main (LISTEN)
ControlCe 466  kyu   34u  IPv6 0x304e6a560a0e8809      0t0  TCP *:commplex-main (LISTEN)
 ~  lsof -n -i TCP:5000                                                                    ✔  16:18:05
 ~  lsof -n -i TCP:5000                                                                  1 х  16:21:03
COMMAND  PID USER   FD   TYPE             DEVICE SIZE/OFF NODE NAME
dotnet  1906  kyu  227u  IPv6 0x304e6a560a0e4a29      0t0  TCP *:commplex-main (LISTEN)
 ~
```
<img width="382" alt="image" src="https://user-images.githubusercontent.com/59367560/161093593-dc8ea60e-594c-476a-8846-291d5175ccd2.png">
