## ##
### ###

## the shared responsibility model ##
The responsibilities vary depending on where the workload is hosted:
- Software as a Service (SaaS)
- Platform as a Service (PaaS)
- Infrastructure as a Service (IaaS)
- On-premises datacenter

## defense in depth ##
Example layers of security
- Physical security (eg. firewall level) - limiting access to a datacenter to only authorized personnel.
- Identity and access security controls - multifactor authentication or condition-based access, to control access to infrastructure and change control.
- Perimeter security - your corporate network includes distributed denial of service (DDoS) protection to filter large-scale attacks before they can cause a denial of service for users.
- Network security (eg. VM level) - network segmentation and network access controls, to limit communication between resources. (retro movement, vm-vm protection)
- Compute layer security - securing access to virtual machines either on-premises or in the cloud by closing certain ports.
- Application layer security - to ensure applications are secure and free of security vulnerabilities.
- Data layer security - controls to manage access to business and customer data and encryption to protect data.

## Zero Trust model ##
Principles
- verify explicitly
- least privileged access
- assume breach

Six foundational pillars
- identities - users, services, or devices
- devices - create a large attack surface as data flows (monitoring the devices by health-check and complicance)
- applications - the way that data is consumed (managing permissions and access)
- data - be classified, labeled, and encrypted on its attributes
  - Exemple of Security Policy at Azure
    - mandatory encryption to store data
    - bitlocker at disk
  - How to work
    - encryption key is owned by microsoft
    - encryption key working with user's key
- infrastructure - access the version, configuration, and JIT access, and use telemetry to detect attacks
- networks - should be segmented (employing real-time threat protection, end-to-end encryption, monitoring, and analytics)

## encryption and hashing ##
### Encryption ###
- Symmetric encryption : he same key to encrypt and decrypt the data
- Asymmetric encryption : a public key and private key pair
> both, the key used to encrypt can’t be used to decrypt encrypted data

Encryption for data at rest
Data at rest is the data that's stored on a physical device, such as a server. It may be stored in a database or a storage account but, regardless of where it's stored, encryption of data at rest ensures the data is unreadable without the keys and secrets needed to decrypt it.

If an attacker obtained a hard drive with encrypted data and didn't have access to the encryption keys, they would be unable to read the data.

Encryption for data in transit
Data in transit is the data moving from one location to another, such as across the internet or through a private network. Secure transfer can be handled by several different layers. It could be done by encrypting the data at the application layer before sending it over a network. HTTPS is an example of encryption in transit.

Encrypting data in transit protects it from outside observers and provides a mechanism to transmit data while limiting the risk of exposure.

Encryption for data in use
A common use case for encryption of data in use involves securing data in nonpersistent storage, such as RAM or CPU caches. This can be achieved through technologies that create an enclave (think of this as a secured lockbox) that protects the data and keeps data encrypted while the CPU processes the data.

### Hashing ###
Hashing uses an algorithm to convert text to a unique fixed-length value called a hash. Each time the same text is hashed using the same algorithm, the same hash value is produced. That hash can then be used as a unique identifier of its associated data.

Hashing is different to encryption in that it doesn't use keys, and the hashed value isn't subsequently decrypted back to the original.

Hashing is often used to store passwords. When a user enters their password, the same algorithm that created the stored hash creates a hash of the entered password. This is compared to the stored hashed version of the password. If they match, the user has entered their password correctly. This is more secure than storing plain text passwords, but hashing algorithms are also known to hackers. Because hash functions are deterministic (the same input produces the same output), hackers can use brute-force dictionary attacks by hashing the passwords. For every matched hash, they know the actual password. To mitigate this risk, passwords are often “salted”. This refers to adding a fixed-length random value to the input of hash functions to create unique hashes for same input.

# Describe the function and identity types of Microsoft Entra ID #
## Microsoft Entra ID ##
AAD -> Entra Id (new term)

Basic Terms
Tenant - A Microsoft Entra tenant is an instance of Microsoft Entra ID in which information about a single organization resides including organizational objects such as users, groups, devices, and application registrations. A tenant also contains access and compliance policies for resources, such as applications registered in the directory. Each Microsoft Entra tenant has a unique ID (tenant ID) and a domain name (for example, contoso.onmicrosoft.com) and serves as a security and administrative boundary, allowing the organization to manage and control access to resources, applications, devices, and services.

Directory - The terms Microsoft Entra directory and Microsoft Entra tenant are often used interchangeably. The directory is a logical container within a Microsoft Entra tenant that holds and organizes the various resources and objects related to identity and access management including users, groups, applications, devices, and other directory objects. Basically, the directory is like a database or catalog of identities and resources associated with an organization's tenant. A Microsoft Entra tenant consists of only one directory.

Multi-tenant - A multi-tenant organization is an organization that has more than one instance of Microsoft Entra ID. Reasons why an organization might have multiple tenants include organizations with multiple subsidiaries or business units that operate independently, organizations that merge or acquire companies, multiple geographical boundaries with various residency regulations, and more.

## types of identities ##
- Human : internal/external users
  [eg. external user workflow]
  - create an external user with email 
  - send an invitation from my tenant with redirection url (auto-generated) 
  - accept a consent via source tenant (or external idp)
  - MFA at source tenant (1st authentication)
  - MFA at my tenant (2nd authentication)

- workload identities : an identity you assign to a software workload to authenticate and access other services and resources
  > nature of workload identities
  > - deal with multiple credentials to access different resources and need to be stored securely
  > - deal with multiple credentials to access different resources and need to be stored securelyhard to track when a workload identity is created or when it should be revoked
  > - risk their applications or services being exploited or breached because of difficulties in securing workload identities
  > -> Entra Workload Id will resolve these issues when securing workload identities

  - service principal
    - an identity for an application,
    - the application must first be registered with Microsoft Entra ID
    - a service principal is created in each Microsoft Entra tenant where the application is used
    - The service principal enables core features such as authentication and authorization of the application
  - managed identities : a type of service principal that are automatically managed in Microsoft Entra ID and eliminate the need for developers to manage credentials.
    - System-assigned
    - User-assigned
    ![image](https://github.com/user-attachments/assets/1fbfce35-2e58-4456-893c-ce9d25a0043d)

- devices :
  - Microsoft Entra registered devices - The goal of Microsoft Entra registered devices is to provide users with support for bring your own device (BYOD) or mobile device scenarios. In these scenarios, a user can access your organization’s resources using a personal device. Microsoft Entra registered devices register to Microsoft Entra ID without requiring an organizational account to sign in to the device.
  - Microsoft Entra joined - A Microsoft Entra joined device is a device joined to Microsoft Entra ID through an organizational account, which is then used to sign in to the device. Microsoft Entra joined devices are generally owned by the organization.
  - Microsoft Entra hybrid joined devices - Organizations with existing on-premises Active Directory implementations can benefit from the functionality provided by Microsoft Entra ID by implementing Microsoft Entra hybrid joined devices. These devices are joined to your on-premises Active Directory and Microsoft Entra ID requiring organizational account to sign in to the device.



## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##

## ##
