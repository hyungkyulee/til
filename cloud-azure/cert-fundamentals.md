# Azure Fundamentals
## Core Azure services

### Active Directory
Account management, main subscription of apps which are registered. It includes :
- users : human
- service : app registration (principle)
> if you can select an app on azure portal > left menu > azure active directory > app registrations > [one of app(principle) you created], you can see the below :
  - client id : application id
  - client credentials : secret key/value pair for this application service
    > If you do not have a client secret, create one.
      - Under Client secrets, click New client secret.
      - Enter a name, an expiration date and click Add.
      - Copy and paste the value of the Client secret into the open file and label the value.
  - tenant id : directory id (active directory account)

### Subscription
a project based group which have role-based access control (RBAC) permissions to manage Azure resources
> combining the AD resources (e.g. users, app registrations, etc)
> billing access is required

### Group and IAM Role
#### Group creation
https://docs.microsoft.com/en-us/azure/active-directory/roles/groups-create-eligible
Groups > All groups > New group
Turn on Azure AD roles - 'assigned'
Add members and owner

#### Assign a Role to subscription with Group
Subscription > select one of your subscription > IAM > Add > Add a role assignment > Assign the group as a contributor
> Add a role assignment is only available by admin(owner)

### Storage Services
Storage Account : storage for a project
Containers : kind of partition
> If containers are created by terraform, it should be deleted by terrafom.
Folders : root folders cannot be removed by Azure portal


#### Files Fundamental
- fully managed file shares via industry **standards server message block** and **NFS**(Network File System)
- file shares mounted **concurrently** on Cloud, Windows, Linux, and MacOS.
- VMs and Roles mounted and accessible to the file storage share **simultaneously**
- files to have each end-point URL to public access or SAS(Shared Access Signature) tokens to access a private asset (for a specific amount of time)
- data encryption **in transit** with SMB protocol, and **at rest** between two geographical locations
- use-cases
  - makes file shares easier to migrate those applications' data to Azure
  - a file share and access from multiple VMs, Tools and Utilities used by multiple developers in a group
  - a process or analysis later with the stored data

#### Blob access tiers
- organise data based on attributes 1) frequency of access (active level), and 2) planned retention period (lifetime)
- Access tiers
  - Hot access tier : frequently (e.g. images of a website)
  - Cool access tier : infrequently and stored for at least 30days (e.g. invoices)
  - Archive access tier : rarely accessed and stored for at least 180days (e.g. backups)
- Hot/Cool access tier can be only set at the account level. (*Archive tier cannot set at the account level.)
- Archive tier has low storage cost, but high access cost - best option with Azure Blob Storage

### Networking Services
#### Virtual Network Fundamentals
- 


# MS React - API Management
## 
