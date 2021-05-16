# Azure Fundamentals
## Core Azure services
### Storage Services
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
