---
title: "Starting Domino 10 Community Edition using systemd on Ubuntu 18.04"
date: 2019-04-21
draft: false
---

The included startup scripts with the installer do not work on Ubuntu 18.04, and some of the examples available are more complex than necessary. The following simple systemd stanza will do the job just fine:

    [Unit]
    Description=IBM Domino Server
    After=syslog.target network.target
    
    [Service]
    User=notes
    LimitNOFILE=60000
    ExecStart=/bin/bash /opt/ibm/domino/bin/server
    WorkingDirectory=/local/notesdata
    
    [Install]
    WantedBy=multi-user.target

Copy this file to `/etc/systemd/system`, and issue a `systemctl daemon-reload` command. Then enable the service using `systemctl enable domino` and start Domino 10 with `systemctl start domino`. Domino should automatically start on boot now, and the server log can be viewed by issuing `systemctl status domino`.
