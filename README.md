# inlets-traefik-docker
Inlets + Traefik 2 + Docker + Docker-Compose

Traefik handles SSL for all subdomains of `*.exit.domain.ext` automatically with wildcard certificate using LetsEncrypt

### DNS Records for exit node / server
```dns
;; A Records
domain.ext.         1	IN	A	1.2.3.4
exit.domain.ext.	  1	IN	A	1.2.3.4

;; CNAME Records
*.exit.domain.ext.	1	IN	CNAME	exit.domain.ext.
```

### Inlets Client Connection
#### Linux Inlets Client
1. Install inlets on client workstation or laptop using https://github.com/inlets/inlets#install-the-cli
2. run following command
```bash
inlets client --upstream "mysubdomain.exit.domain.ext=http://127.0.0.1:LOCALPORT" --remote "wss://exit.domain.ext" --token "REPLACE_WITH_YOUR_ACTUAL_INLET_SERVER_TOKEN"
```

#### Windows Inlets Client
1. First Download NSSM (it helps creates windows service of inlet client): https://nssm.cc/download
2. For simplycity, I copied nssm.exe & inlets.exe to C:\Windows\
3. created inlet-service-install.cmd file with following
```bat
nssm install my.service.name C:\Windows\inlets.exe
nssm set my.service.name AppParameters ^"client --upstream=^\^"mysubdomain.exit.domain.ext=http://127.0.0.1:LOCALPORT^\^" --remote=wss://exit.domain.ext --token=REPLACE_WITH_YOUR_ACTUAL_INLET_SERVER_TOKEN^"
nssm set my.service.name AppDirectory C:\Windows
nssm set my.service.name AppExit Default Restart
nssm set my.service.name AppStdout C:\inlets.log
nssm set my.service.name AppStdoutCreationDisposition 2
nssm set my.service.name AppStderr C:\inlets.error.log
nssm set my.service.name AppStderrCreationDisposition 2
nssm set my.service.name AppRotateFiles 1
nssm set my.service.name AppRotateOnline 1
nssm set my.service.name AppRotateSeconds 108000
nssm set my.service.name AppRotateBytes 30000000
nssm set my.service.name AppTimestampLog 1
nssm set my.service.name Description "This service maintains connection to API on localhost:port"
nssm set my.service.name DisplayName "Inlets Test"
nssm set my.service.name ObjectName LocalSystem
nssm set my.service.name Start SERVICE_AUTO_START
nssm set my.service.name Type SERVICE_WIN32_OWN_PROCESS
```

In case of `mysubdomain.exit.domain.ext` mentioned above `mysubdomain` part can be changed to anything you may like.
http://127.0.0.1:LOCALPORT is having IIS/Apache running locally on my machine serving demo site.
Feel free to modify and use as per your usecase.
