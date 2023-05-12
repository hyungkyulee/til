
## Setup Docker
- Install the latest Docker for Apple Silicon : https://desktop.docker.com/mac/main/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=dd-smartbutton&utm_location=module
- On Docker Desktop, go to Settings and find the “Features in development” option
- Select the “Use Rosetta for x86/amd64 emulation on Apple Silicon” checkbox, and restart
  > if there is no option, update docker desktop
  > <img width="659" alt="image" src="https://user-images.githubusercontent.com/59367560/235894248-01f976e4-6181-4b4b-9353-63eea51d9534.png">

## SQL image setup on docker
- Login to docker
- Install Azure SQL Edge image
  > ref: https://learn.microsoft.com/en-us/azure/azure-sql-edge/disconnected-deployment
  - by Docker explorer
  - by CLI
    - Install the sql docker image
    ```
     ~  docker pull mcr.microsoft.com/azure-sql-edge                ✔ │ 18:30:18
    Using default tag: latest
    latest: Pulling from azure-sql-edge
    c58359f0ed07: Pull complete
    f9c126982b5c: Pull complete
    589ba23f4d73: Pull complete
    0c037bc6ac64: Pull complete
    ce1f004ff642: Pull complete
    4e0b1d630a9d: Pull complete
    cf712679c0f8: Pull complete
    7f5ed2ab3c5b: Pull complete
    56e4c7793de3: Pull complete
    89f8b7dcee44: Pull complete
    82fa393cf611: Pull complete
    Digest: sha256:1dcc88d2d9e555d0addb0636648d0da033206978d7c5c4da044c904a0f06f58b
    Status: Downloaded newer image for mcr.microsoft.com/azure-sql-edge:latest
    mcr.microsoft.com/azure-sql-edge:latest
    ```
    > after installation by CLI, the image is available on docker images menu
    <img width="782" alt="image" src="https://user-images.githubusercontent.com/59367560/236893739-44a1f085-8dab-4b86-b5c4-5712b76ec9ca.png">

    - run the image
    ```
     ~ docker run -d --name AzureSQLServer -e "ACCEPT_EULA=Y" -e "SA_PASSWORD=p***w*r*" -p 1433:1433 mcr.microsoft.com/azure-sql-edge


    c11ff9b031438de5b67426988870468bc01ed8f8fccad49874c49fe40791f657
    ```
    > sa password should be more than 10digits + including CAP letter + number
    > after run the image, it shows in a Docker container on a live localhost port.
    <img width="862" alt="image" src="https://user-images.githubusercontent.com/59367560/236895447-54ae7f7e-4061-4aee-a3f3-ddaf3202bf31.png">


## Setup DB Client (e.g. Azure Data Studio)
- Isntall and setup 
  > https://learn.microsoft.com/en-us/sql/azure-data-studio/download-azure-data-studio?view=sql-server-ver16&tabs=redhat-install%2Credhat-uninstall

- connect the server via Azure Data Studio
  > df



