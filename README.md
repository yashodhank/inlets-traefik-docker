# inlets-traefik-docker
Inlets + Traefik 2 + Docker + Docker-Compose

Traefik handles SSL for all subdomains of `*.exit.domain.ext` automatically with wildcard certificate using LetsEncrypt

### DNS Records for exit node / server
```dns
;; A Records
domain.ext.	1	IN	A	1.2.3.4
exit.domain.ext.	1	IN	A	1.2.3.4

;; CNAME Records
*.exit.domain.ext.	1	IN	CNAME	exit.domain.ext.
```

### Inlet Client Connection
#### Linux
1. Install inlets on client workstation or laptop using https://github.com/inlets/inlets#install-the-cli
2. run following command
```
inlets client --upstream "mysubdomain.exit.domain.ext=http://127.0.0.1:80" --remote "wss://exit.domain.ext" --token "REPLACE_WITH_YOUR_ACTUAL_INLET_SERVER_TOKEN"
```

#### Windows
1. First Download NSSM (it helps creates windows service of inlet client): https://nssm.cc/download
2. For simplycity, I copied nssm.exe & inlets.exe to C:\Windows\
3. created inlet-service-install.cmd file with following
```cmd
nssm install pipli.test C:\Windows\inlets.exe
nssm set pipli.test AppParameters ^"client --upstream=^\^"mysubdomain.exit.domain.ext=http://127.0.0.1:80^\^" --remote=wss://exit.domain.ext --token=REPLACE_WITH_YOUR_ACTUAL_INLET_SERVER_TOKEN^"
nssm set pipli.test AppDirectory C:\Windows
nssm set pipli.test AppExit Default Restart
nssm set pipli.test AppStdout C:\inlets.log
nssm set pipli.test AppStdoutCreationDisposition 2
nssm set pipli.test AppStderr C:\inlets.error.log
nssm set pipli.test AppStderrCreationDisposition 2
nssm set pipli.test AppRotateFiles 1
nssm set pipli.test AppRotateOnline 1
nssm set pipli.test AppRotateSeconds 108000
nssm set pipli.test AppRotateBytes 30000000
nssm set pipli.test AppTimestampLog 1
nssm set pipli.test Description "This service maintains connection to API on localhost:port"
nssm set pipli.test DisplayName "Pipli Test"
nssm set pipli.test ObjectName LocalSystem
nssm set pipli.test Start SERVICE_AUTO_START
nssm set pipli.test Type SERVICE_WIN32_OWN_PROCESS
```

In case of `mysubdomain.exit.domain.ext` above `mysubdomain` part can be changed to anything you may like.
