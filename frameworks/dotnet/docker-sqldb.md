# .NET project with SQL Database on Docker

## Docker setup on MAC

https://www.docker.com/get-started
Download Docker for Mac(Intel Chip) : https://desktop.docker.com/mac/stable/arm64/Docker.dmg?utm_source=docker&utm_medium=webreferral&utm_campaign=dd-smartbutton&utm_location=module

Install and Run the Docker

Docker demory configuration (increase the memory to 4GB)
> docker > preference > resources > Memory > 4GB > Apply & Restart

Install SQL Server
```
docker run -d --name sql_server -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=someThingComplicated1234' -p 1433:1433 mcr.microsoft.com/mssql/server:2019-latest
```
> -d : launch the container in 'detached' mode
> --name : assing a name to the container
> -e : allow you to set environment variables
>   - 'ACCEPT_EULA=Y' : User Agreement to accept
>   - 'SA_PASSWORD=someThingComplicated1234' : admin password (* password should over 9 digits including number, caracter, -, capital, not other special characters)
> -p : map the local port 1433 to port 1433 on the container (1433 = default TCP port of SQL Server)
> mcr.microsoft.com/mssql/server:2019-latest : image to run

```
❯ docker run -d --name sql_server -e 'ACCEPT_EULA=Y' -e 'SA_PASSWORD=**********' -p 1433:1433 mcr.microsoft.com/mssql/server:2019-latest
Unable to find image 'mcr.microsoft.com/mssql/server:2019-latest' locally
2019-latest: Pulling from mssql/server
345e3491a907: Pull complete
57671312ef6f: Pull complete
5e9250ddb7d0: Pull complete
1f9b20e23ebb: Pull complete
e23afff1f9a0: Pull complete
83c8e0c0584e: Pull complete
17d57cdb8829: Pull complete
Digest: sha256:51965e4e4c17e6fef087550190c2920c7ef91bd449d0eec06a5484b92c437767
Status: Downloaded newer image for mcr.microsoft.com/mssql/server:2019-latest
cfb3580b7fc217dbc7d43210b106b18b29b0d8ba7d5546cb71519ffa84e0968f
```

Install SQL CLI Tool
```
npm install -g sql-cli
```

Connect Database
```
mssql -u sa -p someThingComplicated1234
```
> e.g.
```
❯ mssql -u sa -p ******
Connecting to localhost...done

sql-cli version 0.6.2
Enter ".help" for usage hints.
mssql>
```

Create a Database
```
mssql> CREATE DATABASE "xxx.xxx.Db"
```

[Appendix]
docker commands : https://docs.docker.com/engine/reference/commandline/container/

check the container list
```
❯ docker container ls
CONTAINER ID   IMAGE                                        COMMAND                  CREATED          STATUS          PORTS                                       NAMES
cfb3580b7fc2   mcr.microsoft.com/mssql/server:2019-latest   "/opt/mssql/bin/perm…"   10 minutes ago   Up 10 minutes   0.0.0.0:1433->1433/tcp, :::1433->1433/tcp   sql_server
```

stop and remove the container
```
docker container rm [container]
```

docker container process status
```
docker ps -a
```
> odcker ps : show the current docker on up status and running.

docker start or restart
```
docker restart [container]
```
> if docker start is not working, use restart


## Data Source connection via rider
New Database 
> Database  > New > Data Source > Microsoft SQL Server
![image](https://user-images.githubusercontent.com/59367560/124370835-51673480-dc73-11eb-9265-a3444aaaed77.png)

Database Connection and Test
> Database > Data source property
![image](https://user-images.githubusercontent.com/59367560/124370855-98edc080-dc73-11eb-8bb6-b3bfd8a5ea79.png)
> if there are missing driver files, install them
> set the Database Name with the pre-created database

Do test connection with user:password (sa:[your password])
