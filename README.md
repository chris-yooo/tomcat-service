# Tomcat als systemctl service einrichten

Mit der anleitung von @bartfastiel https://github.com/bartfastiel/spring-boot-tomcat-deployment tomcat 
soweit einrichten bis zu dem punkt an dem tomcat mit der /bin/startup.sh gestartet wird.

oder

https://github.com/chris-yooo/tomcat-deployment

### dann folgendes:

`tomcat.service` in `/home/ec2-user` anlegen

`nano tomcat.service`

inhalt hineinkopieren:

```
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
User=ec2-user
Group=ec2-user
WorkingDirectory=/opt/webserver/apache-tomcat-9.0.70/bin
ExecStart=/opt/webserver/apache-tomcat-9.0.70/bin/startup.sh
ExecStop=/opt/webserver/apache-tomcat-9.0.70/bin/shutdown.sh
Type=forking
UMask=0007
RestartSec=10
Restart=always

Environment=CATALINA_BASE=/opt/webserver/apache-tomcat-9.0.70
Environment=CATALINA_HOME=/opt/webserver/apache-tomcat-9.0.70
Environment=CATALINA_TMPDIR=/opt/webserver/apache-tomcat-9.0.70/temp
Environment=JRE_HOME=/opt/webserver/jdk-19
Environment=JAVA_HOME=/opt/webserver/jdk-19
Environment=CLASSPATH=/opt/webserver/apache-tomcat-9.0.70/bin/bootstrap.jar:/opt/webserver/apache-tomcat-9.0.70/bin/tomcat-juli.jar
Environment=CATALINA_OPTS=
Environment=CATALINA_PID=/opt/webserver/apache-tomcat-9.0.70/temp/tomcat.pid

[Install]
WantedBy=multi-user.target
```

`sudo chown root:root /home/ec2-user/tomcat.service && sudo chmod 0755 /home/ec2-user/tomcat.service && sudo mv /home/ec2-user/tomcat.service /usr/lib/systemd/system`

`sudo systemctl daemon-reload`

`sudo systemctl enable tomcat`

### nun startet tomcat schon bei neustart der Maschine und lässt sich mit folgenden befehlen starten/stoppen etc...

status:
`sudo systemctl status tomcat`

re-start:
`sudo systemctl restart tomcat`

start:
`sudo systemctl start tomcat`

stop:
`sudo systemctl stop tomcat`
