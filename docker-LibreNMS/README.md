
## LibreNMS for Docker: 

To run LibreNMS inside Docker, you can use the official LibreNMS Docker image. Below is a step-by-step guide on how to set it up.



### Download and unzip composer files:


_Create a new directory for LibreNMS:_

```
mkdir librenms
cd librenms
```


```
wget https://github.com/librenms/docker/archive/refs/heads/master.zip

unzip master.zip
```






#### Run the docker containers:

This setup uses multiple services to run LibreNMS with several dependencies. Here's a brief explanation of each:

- MariaDB (db): The database used by LibreNMS to store information.
- Redis: Caching service to speed up LibreNMS.
- msmtpd: For sending email alerts.
- LibreNMS (librenms): The main service for LibreNMS.
- Dispatcher: Handles the polling of devices and other tasks.
- syslogng: For collecting syslog messages.
- snmptrapd: Handles SNMP traps.



> [!NOTE]  
> Set a new mysql password in `.env` and inspect `compose.yml`



```
cd docker-master/examples/compose
```



```
docker compose -f compose.yml up -d
```



```
docker ps -a
CONTAINER ID   IMAGE                      COMMAND                  CREATED          STATUS                    PORTS                                                                                                      NAMES
365ec866383a   librenms/librenms:latest   "/init"                  19 minutes ago   Up 19 minutes             514/tcp, 0.0.0.0:162->162/tcp, 0.0.0.0:162->162/udp, :::162->162/tcp, :::162->162/udp, 8000/tcp, 514/udp   librenms_snmptrapd
741070d64719   librenms/librenms:latest   "/init"                  19 minutes ago   Up 19 minutes             162/tcp, 8000/tcp, 162/udp, 0.0.0.0:514->514/tcp, 0.0.0.0:514->514/udp, :::514->514/tcp, :::514->514/udp   librenms_syslogng
1035ea3f4e90   librenms/librenms:latest   "/init"                  19 minutes ago   Up 19 minutes             162/tcp, 162/udp, 514/tcp, 8000/tcp, 514/udp                                                               librenms_dispatcher
e63774f632fe   librenms/librenms:latest   "/init"                  19 minutes ago   Up 19 minutes             162/tcp, 162/udp, 514/tcp, 514/udp, 0.0.0.0:8000->8000/tcp, :::8000->8000/tcp                              librenms
f59ba5c786fc   mariadb:10                 "docker-entrypoint.s…"   19 minutes ago   Up 19 minutes             3306/tcp                                                                                                   librenms_db
dfbd4a86e16f   redis:7.2-alpine           "docker-entrypoint.s…"   19 minutes ago   Up 19 minutes             6379/tcp                                                                                                   librenms_redis
9cef22f94ad5   crazymax/msmtpd:latest     "/init"                  19 minutes ago   Up 19 minutes (healthy)   2500/tcp                                                                                                   librenms_msmtpd
```



### Web Console:

Once the containers are up, open your web browser and go to: `http://localhost:8000/`




### Validate:

If you want to validate your installation from the CLI, type the following command:

```
docker compose exec --user librenms librenms php validate.php
```


```
### Output:

===========================================
Component | Version
--------- | -------
LibreNMS  | 25.1.0 (2025-01-21T00:15:30+01:00)
DB Schema | 2024_11_22_135845_alert_log_refactor_indexes (310)
PHP       | 8.3.16
Python    | 3.12.8
Database  | MariaDB 10.11.11-MariaDB-ubu2204
RRDTool   | 1.9.0
SNMP      | 5.9.4
===========================================

[OK]    Installed from the official Docker image; no Composer required
[OK]    Database connection successful
[OK]    Database connection successful
[OK]    Database Schema is current
[OK]    SQL Server meets minimum requirements
[OK]    lower_case_table_names is enabled
[OK]    MySQL engine is optimal
[OK]    Database and column collations are correct
[OK]    Database schema correct
[OK]    MySQL and PHP time match
[OK]    Active pollers found
[OK]    Dispatcher Service is enabled
[OK]    Locks are functional
[OK]    No python wrapper pollers found
[OK]    Redis is functional
[OK]    rrd_dir is writable
[OK]    rrdtool version ok
[WARN]  Updates are managed through the official Docker image
```



### Links:
- [Installation for Docker](https://docs.librenms.org/Installation/Docker/)
- [Librenms | hub.docker.com](https://hub.docker.com/r/librenms/librenms/)
- [Librenms | github.com](https://github.com/librenms/docker)


