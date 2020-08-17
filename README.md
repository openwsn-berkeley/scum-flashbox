# scum-programmer

## hardware required

- a raspberry Pi
- a button
- a power bank
- a box container for all above

## installation

- install rPi with Raspbian
- install supervisor on rPi with `sudo apt-get install supervisor`
- create folder `/home/pi/hornet_tracker`
- under the created folder, git clone https://github.com/pisterlab/scum-test-code (where the bootload script located)
- switch branch to develop
- replace the content for `supervisord.conf` under `/etc/supervisor/` with following content

```
[unix_http_server]
file=/var/run/supervisor.sock
chmod=0700

[supervisord]
user        = root
logfile     = /tmp/supervisord.log
logfile=/var/log/supervisor/supervisord.log ; (main log file;default $CWD/supervisord.log)
pidfile=/var/run/supervisord.pid ; (supervisord pidfile;default supervisord.pid)
childlogdir=/var/log/supervisor            ; ('AUTO' child log dir, default $TEMP)

[supervisorctl]
serverurl=unix:///var/run/supervisor.sock

[inet_http_server]
port        = 127.0.0.1:9001

[program:bootload]
command         = python /home/pi/hornet_tracker/scum-test-code/scm_v3c/bootload/bootload.py -tp /dev/ttyACM0 -i  /home/pi/hornet_tracker/target.bin
autostart       = true
autorestart     = true
directory       = /home/pi/hornet_tracker
redirect_stderr = true
stdout_logfile  = /tmp/bootload.log

[rpcinterface:supervisor]
supervisor.rpcinterface_factory = supervisor.rpcinterface:make_main_rpcinterface
```
