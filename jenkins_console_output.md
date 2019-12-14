```
Started by user jenkins
Running in Durability level: MAX_SURVIVABILITY
[Pipeline] Start of Pipeline
[Pipeline] node
Running on Jenkins in /var/lib/jenkins/workspace/Project_wiki
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] stage
[Pipeline] { (Build and run docker for Dokuwiki)
[Pipeline] stage
[Pipeline] { (Clone Git)
[Pipeline] git
No credentials specified
 > git rev-parse --is-inside-work-tree # timeout=10
Fetching changes from the remote Git repository
 > git config remote.origin.url git@github.com:andreilukashonak/project.git # timeout=10
Fetching upstream changes from git@github.com:andreilukashonak/project.git
 > git --version # timeout=10
 > git fetch --tags --progress git@github.com:andreilukashonak/project.git +refs/heads/*:refs/remotes/origin/* # timeout=10
 > git rev-parse refs/remotes/origin/master^{commit} # timeout=10
 > git rev-parse refs/remotes/origin/origin/master^{commit} # timeout=10
Checking out Revision 860291653f667d72e03d9c1f93e3a567a9037d95 (refs/remotes/origin/master)
 > git config core.sparsecheckout # timeout=10
 > git checkout -f 860291653f667d72e03d9c1f93e3a567a9037d95 # timeout=10
 > git branch -a -v --no-abbrev # timeout=10
 > git branch -D master # timeout=10
 > git checkout -b master 860291653f667d72e03d9c1f93e3a567a9037d95 # timeout=10
Commit message: "third commit"
 > git rev-list --no-walk 860291653f667d72e03d9c1f93e3a567a9037d95 # timeout=10
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Build image)
[Pipeline] script
[Pipeline] {
[Pipeline] isUnix
[Pipeline] sh
+ docker build -t andreilukashonak/project:40 .
Sending build context to Docker daemon 90.11 kB

Step 1/7 : FROM alpine:latest
Trying to pull repository docker.io/library/alpine ... 
latest: Pulling from docker.io/library/alpine
89d9c30c1d48: Pulling fs layer
89d9c30c1d48: Download complete
89d9c30c1d48: Pull complete
Digest: sha256:c19173c5ada610a5989151111163d28a67368362762534d8a8121ce95cf2bd5a
Status: Downloaded newer image for docker.io/alpine:latest
 ---> 965ea09ff2eb
Step 2/7 : RUN apk update &&   apk add wget nginx php7 php7-common php7-mbstring php7-session php7-json php7-fpm &&   wget -q http://download.dokuwiki.org/src/dokuwiki/dokuwiki-stable.tgz -P /tmp &&   mkdir /var/www/dokuwiki &&   tar -xzf /tmp/dokuwiki-stable.tgz -C /var/www/dokuwiki --strip-components 1 &&   chown -R nginx:nginx /var/www/dokuwiki &&   chmod -R 777 /var/www/dokuwiki
 ---> Running in a8474912befd

[91m[0mfetch http://dl-cdn.alpinelinux.org/alpine/v3.10/main/x86_64/APKINDEX.tar.gz
fetch http://dl-cdn.alpinelinux.org/alpine/v3.10/community/x86_64/APKINDEX.tar.gz
v3.10.3-77-g9739986c1e [http://dl-cdn.alpinelinux.org/alpine/v3.10/main]
v3.10.3-68-ge1e42c5d6c [http://dl-cdn.alpinelinux.org/alpine/v3.10/community]
OK: 10341 distinct packages available
(1/16) Installing pcre (8.43-r0)
(2/16) Installing nginx (1.16.1-r1)
Executing nginx-1.16.1-r1.pre-install
(3/16) Installing php7-common (7.3.11-r0)
(4/16) Installing argon2-libs (20171227-r2)
(5/16) Installing ncurses-terminfo-base (6.1_p20190518-r0)
(6/16) Installing ncurses-terminfo (6.1_p20190518-r0)
(7/16) Installing ncurses-libs (6.1_p20190518-r0)
(8/16) Installing libedit (20190324.3.1-r0)
(9/16) Installing pcre2 (10.33-r0)
(10/16) Installing libxml2 (2.9.9-r2)
(11/16) Installing php7 (7.3.11-r0)
(12/16) Installing php7-fpm (7.3.11-r0)
(13/16) Installing php7-json (7.3.11-r0)
(14/16) Installing php7-mbstring (7.3.11-r0)
(15/16) Installing php7-session (7.3.11-r0)
(16/16) Installing wget (1.20.3-r0)
Executing busybox-1.30.1-r2.trigger
OK: 28 MiB in 30 packages
 ---> d6327c5d4e06
Removing intermediate container a8474912befd
Step 3/7 : COPY nginx/sites/default /etc/nginx/conf.d/default.conf
 ---> 83482c01187a
Removing intermediate container b0e60daf37b1
Step 4/7 : RUN mkdir /run/nginx/ && touch /run/nginx/nginx.pid &&   touch /var/log/nginx/access.log && touch /var/log/nginx/error.log
 ---> Running in 4f1d106d668e

[91m[0m ---> 29efbba919b9
Removing intermediate container 4f1d106d668e
Step 5/7 : COPY start.sh /start.sh
 ---> fa3949c0031e
Removing intermediate container 9452167ba682
Step 6/7 : EXPOSE 80
 ---> Running in 839b812c204a
 ---> 8da709c21709
Removing intermediate container 839b812c204a
Step 7/7 : CMD sh /start.sh
 ---> Running in e6c1361048c9
 ---> 5a2ea5827620
Removing intermediate container e6c1361048c9
Successfully built 5a2ea5827620
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Push image)
[Pipeline] script
[Pipeline] {
[Pipeline] withEnv
[Pipeline] {
[Pipeline] withDockerRegistry
$ docker login -u andreilukashonak -p ******** https://index.docker.io/v1/
Login Succeeded
[Pipeline] {
[Pipeline] isUnix
[Pipeline] sh
+ docker tag andreilukashonak/project:40 andreilukashonak/project:40
[Pipeline] isUnix
[Pipeline] sh
+ docker push andreilukashonak/project:40
The push refers to a repository [docker.io/andreilukashonak/project]
f22c0fb55e04: Preparing
3ec6c4b88adb: Preparing
bdb19e42df9e: Preparing
83857cc89bb8: Preparing
77cae8ab23bf: Preparing
f22c0fb55e04: Layer already exists
77cae8ab23bf: Layer already exists
3ec6c4b88adb: Pushed
bdb19e42df9e: Pushed
83857cc89bb8: Pushed
40: digest: sha256:5253185a1bff3f2c9adc3da66d9b5a16b199d62f9419eaf54d980c45b24f7215 size: 1361
[Pipeline] }
[Pipeline] // withDockerRegistry
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // script
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Run docker image)
[Pipeline] sh
+ echo andreilukashonak/project:40
andreilukashonak/project:40
[Pipeline] sh
+ docker run -d -p 4000:80 --name dokuwiki andreilukashonak/project:40
bbfd515b8790ed119814d8d3ad45784b08c4c9f0e875ba0e9aec447944beaa3b
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Remove Dokuwiki)
Stage "Remove Dokuwiki" skipped due to when conditional
[Pipeline] stage
[Pipeline] { (Stop and delete docker container)
Stage "Remove Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Remove docker image)
Stage "Remove Dokuwiki" skipped due to when conditional
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // stage
[Pipeline] stage
[Pipeline] { (Declarative: Post Actions)
[Pipeline] slackSend
Slack Send Pipeline step running, values are - baseUrl: <empty>, teamDomain: sa-itacademy-by, channel: builds, color: #00FF00, botUser: false, tokenCredentialId: X0WwrK40AjwFRZu0WxjdVYo2, notifyCommitters: false, iconEmoji: <empty>, username: <empty>
[Pipeline] }
[Pipeline] // stage
[Pipeline] }
[Pipeline] // withEnv
[Pipeline] }
[Pipeline] // node
[Pipeline] End of Pipeline
Finished: SUCCESS
```
