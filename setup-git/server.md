## Setup a git server with gitea

---
Hope this guide help you can create your own self-hosted git server.
- [Install git source control](#install-git-and-postgresql)
- [Install PostgreSQL and create database to store gitea data.](#install-git-and-postgresql)
- Install gitea
  - [Download gitea](#download-gitea)
  - [Install gitea](#install-gitea)
  - [Config gitea](#gitea-configuration)
  - [Test gitea](#test-gitea-instance)
  - [Create gitea auto start services](#create-gitea-auto-start-service)
  - [Create first admin user](#gitea-cli---create-login-account-for-admin)
---

### About gitea
Gitea is a painless self-hosted Git service. It is similar to GitHub, Bitbucket, and GitLab. Gitea is a fork of Gogs. See the Gitea Announcement blog post to read about the justification for a fork.
Full document can be found [here](https://docs.gitea.io/en-us/).

### Environment
* Operating system: Windows 10. Following method can be applied with Windows 7, Windows 11
* Git source control.
* Gitea latest version
* PosgreSQL to store gitea's database

---
### Install Git and PostgreSQL
- [Install Git source control.](install-git.md)
- [Install PostgreSQL.](../install-posgresql.md)
  - Create database `gitea-database` on PostgreSQL server, leave it blank. This will be used in configuration at following steps. (Use HeidiSQL - see [here](../install-posgresql.md#test-postgresql))
  ![Create database](images/create-gitea-database.png)
  - Connect to `gitea-database` to verify it created. Leave the database blank, gitea will automatically init the database later.
  ![gitea-database test](images/postgre-database-connection.png)


### Download gitea.
I use version 1.16.8 is the latest version in 21-my-2022.
* Download latest gitea [here](https://github.com/go-gitea/gitea/releases). You can use browser to download.  
Or use PowerShell
```sh
>mkdir d:\git-docs
>cd d:\git-docs
>Invoke-WebRequest https://github.com/go-gitea/gitea/releases/download/v1.16.8/gitea-1.16.8-gogit-windows-4.0-amd64.exe -OutFile gitea-1.16.8.exe
```
Download progress.  
![Dowloading gitea](images/gitea-download-s1.png)

### Install gitea
* Make a directory in drive C `C:\gitea-exe\` to store `gitea` executable files and configurations. Copy downloaded gitea executable file into `C:\gitea-exe\`

```sh
>mkdir C:\gitea-exe
>cd C:\gitea-exe\
>Copy-Item -Path D:\git-docs\gitea-1.16.8.exe -Destination C:\gitea-exe\
```

![Install gitea](images/gitea-install-s1.png)

### Gitea configuration
All document can be found in gitea official site [here](https://docs.gitea.io/en-us/).
* Create folder `C:\gitea-repositories\` to store all repositories create on the server. The directory can be change to safe place and MUST BE BACKED UP FREQUENTLY.
* Create configuration file `C:\gitea-exe\custom\conf\app.ini`. Sample configuration file can be download [here](https://github.com/go-gitea/gitea/blob/main/custom/conf/app.example.ini).
* Create a file to store gitea's configuration `C:\gitea-exe\custom\conf\app.ini`. Edit the file `C:\gitea-exe\custom\conf\app.ini` with you favorite editor - here I use notepad++. Copy and past sample config as below.  
NOTE: Following configuration is just a sample. Must adjust configuration file follow documents [here](https://docs.gitea.io/en-us/config-cheat-sheet/) and [here](https://docs.gitea.io/en-us/config-cheat-sheet/) as your requirement.
```ini
; ; App name that shows in every page title
APP_NAME = Genius git server
; ; RUN_USER will automatically detect the current user - but you can set it here change it if you run locally
RUN_USER = 
; ; Application run mode, affects performance and debugging. Either "dev", "prod" or "test", default is "prod"
RUN_MODE = prod

[security]
; ; Whether the installer is disabled (set to true to disable the installer)
INSTALL_LOCK   = true
; ; Global secret key that will be used - if blank will be regenerated.
SECRET_KEY     = 
; ; Secret used to validate communication within Gitea binary.
INTERNAL_TOKEN = eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYmYiOjE2NTMxMDIyOTB9.xvkAJ1KKF_Uz1oLqXPp31fUj77AOio8weiGwEYax0F4

[server]
; ; The protocol the server listens on. One of 'http', 'https', 'unix' or 'fcgi'. Defaults to 'http'
PROTOCOL         = http
SSH_DOMAIN       = 
; ; Set the domain for the server
DOMAIN           = localhost
; ; The address to listen on. Either a IPv4/IPv6 address or the path to a unix socket.
HTTP_ADDR        = 0.0.0.0
; ; The port to listen on. Leave empty when using a unix socket.
HTTP_PORT        = 8899
; ; Local (DMZ) URL for Gitea workers (such as SSH update) accessing web service.
; ; In most cases you do not need to change the default value.
; ; Alter it only if your SSH server node is not the same as HTTP node.
; ; Do not set this variable if PROTOCOL is set to 'unix'.
LOCAL_ROOT_URL   = %(PROTOCOL)s://%(HTTP_ADDR)s:%(HTTP_PORT)s/
; ; Overwrite the automatically generated public URL. Necessary for proxies and docker.
ROOT_URL         = %(PROTOCOL)s://%(DOMAIN)s:%(HTTP_PORT)s/
DISABLE_SSH      = true
SSH_PORT         = 22
LFS_START_SERVER = true
LFS_CONTENT_PATH = C:/gitea/data/lfs
LFS_JWT_SECRET   = GkpWy30WlmXP54WVRmnXb4yOhIrrZCiC-SbPjWjxN4g
OFFLINE_MODE     = false

[database]
DB_TYPE  = postgres
HOST     = 127.0.0.1:5432
NAME     = postgre-database
USER     = postgres
PASSWD   = <passowd here>
SSL_MODE = disable
SCHEMA   = public

[git]
; The path of git executable. If empty, Gitea searches through the PATH environment.
PATH                         = 
; Disables highlight of added and removed changes
DISABLE_DIFF_HIGHLIGHT       = false
; Max number of lines allowed in a single file in diff view
MAX_GIT_DIFF_LINES           = 1000
; Max number of allowed characters in a line in diff view
MAX_GIT_DIFF_LINE_CHARACTERS = 5000
; Max number of files shown in diff view
MAX_GIT_DIFF_FILES           = 100
; Arguments for command 'git gc', e.g. "--aggressive --auto"
; see more on http://git-scm.com/docs/git-gc/
GC_ARGS                      = 
; If use git wire protocol version 2 when git version >= 2.18, default is true, set to false when you always want git wire protocol version 1
EnableAutoGitWireProtocol    = true

; Operation timeout in seconds
[git.timeout]
DEFAULT = 360
MIGRATE = 600
MIRROR  = 300
CLONE   = 300
PULL    = 300
GC      = 60

[repository]
ROOT = C:/gitea-repositories

; [repository.signing]
; SIGNING_KEY = none
; SIGNING_NAME =
; SIGNING_EMAIL =
; INITIAL_COMMIT = always
; CRUD_ACTIONS = pubkey, twofa, parentsigned
; WIKI = never
; MERGES = never
[mailer]
ENABLED = false

[service]
REGISTER_EMAIL_CONFIRM            = false
ENABLE_NOTIFY_MAIL                = false
DISABLE_REGISTRATION              = true
ALLOW_ONLY_EXTERNAL_REGISTRATION  = false
ENABLE_CAPTCHA                    = false
REQUIRE_SIGNIN_VIEW               = false
DEFAULT_KEEP_EMAIL_PRIVATE        = false
DEFAULT_ALLOW_CREATE_ORGANIZATION = true
DEFAULT_ENABLE_TIMETRACKING       = true
NO_REPLY_ADDRESS                  = noreply.example.org

[picture]
DISABLE_GRAVATAR        = false
ENABLE_FEDERATED_AVATAR = true
GRAVATAR_SOURCE         = gravatar

[openid]
ENABLE_OPENID_SIGNIN = true
ENABLE_OPENID_SIGNUP = false

[session]
PROVIDER = file

[log]
MODE      = file
LEVEL     = Debug
ROOT_PATH = C:/gitea-exe/log

[oauth2]
JWT_SECRET = V3wrGccnSjDkq41zBIQY1afVMDufvm80AjYYAOtJy2w


```

* Run the gitea. Back to Powershell.
```sh
>cd C:\gitea-exe
```

### Test gitea instance
Run following command with PowerShell
```sh
>cd C:\gitea-exe
>.\gitea-1.16.8.exe web --config C:\gitea-exe\custom\conf\app.ini
```

You should see as following.
![Gitea cli initial](images/gitea-cli-init-s1.png)

Open browser and access url [`http://localhost:8899`](http://localhost:8899). You should see the home page of gitea - that mean your gitea configuration is working.  
Jump to [this section](#gitea-cli---create-login-account-for-admin) to create first admin account to manage gitea server.  
Jump to [this section](#create-gitea-auto-start-service) to setup gitea service, this will make your gitea instance automatically start every time the physical server restart.
![Gitea home page](images/gitea-home-s1.png)

### Create gitea auto start service
To register Gitea as a Windows service, open a Powershell as an Administrator, then run the following command:
```sh
>New-Service -Name genius-gitea1 -StartupType Automatic -BinaryPathName "C:\gitea-exe\gitea-1.16.8.exe web --config=C:\gitea-exe\custom\conf\app.ini"
>Start-Service -Name genius-gitea
```  
You should see as following.  
![Gites service creating](images/gitea-service-creating.png)  

Gitea service is ready. open browser to [http://localhost:8899/](http://localhost:8899/) to test gitea service.  

To remove gitea service. Open PowerShell as an Administrator, then run following command.
```sh
>Stop-Service -Name genius-gitea
>Remove-Service -Name genius-gitea
```
![Gitea service remote](images/gitea-service-remove.png)

### Gitea CLI - create login account for admin.
All document for admin manage using CLI can be found [here](https://docs.gitea.io/en-us/command-line/#admin). This document very useful to trouble shoot you issue during working with gitea.  

Create first admin account to access gitea server.   
Open PowerShell with admin privileges (Run ad Administrator).   
Here we create first user with admin role.
* Username: genius.gitea
* Password: P@ssw0rd 

NOTES:
- Remove `--admin` flag is you want to create normal user.
- Change `username` and `password` as your secret.
```sh
>cd C:\gitea-exe
>.\gitea-1.16.8.exe admin user create --username="genius.gitea" --password P@ssw0rd --email="dungjk@gmail.com" --admin
```

Open browser to gitea server and login with credential above.
![Gitea login](images/gitea-signin.png)
![Gitea logged in](images/gitea-signed-in.png)


List all users.  
```sh
>.\gitea-1.16.8.exe admin user list
```

![Gitea list user](images/gitea-list-user.png)
