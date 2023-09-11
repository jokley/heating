# Project heating
## FEHM / NGINX Docker
### FHEM
+ Docker-Compose für den Zugriff auf die GPIOS:
    + -privileged: true
    + -volumes: - /sys:/sys
    + -devices: - "/dev/gpiomem:/dev/gpiomem"
    + -environment: GPIO_GID: 997
      
+ Root Berechtigung GPIO
```ruby
chown -R root:gpio /sys/class/gpio && chmod -R 770 /sys/class/gpio
```
+ Watchdog installieren
```ruby
sudo apt-get install watchdog
sudo modprobe bcm2835_wdt
echo "bcm2835_wdt" | sudo tee -a /etc/modules
```
+ Watchdog konfigurieren
```ruby
sudo nano /etc/watchdog.conf
```
```ruby
watchdog-device = /dev/watchdog
max-load-1      = 24
file            = /home/pi/Projects/heating/fhem/log/fhem.heartbeat
change          = 1200
```
+ Watchdog Verzögerung beim Starten
```ruby
Sudo nano /etc/crontab
```
```ruby
@reboot root /bin/sleep 300; /usr/bin/touch / home/pi/Projects/heating/fhem/log/fhem.heartbeat; /etc/init.d/watchdog start
```

### NGINX
+ Damit die ICONS den Status ohne refresh ändern, muss in der NGINX config das buffering ausgeschaltet werden.
```ruby
proxy_buffering off;
proxy_ignore_client_abort off;
break;
```

+ .htpasswd File erstellen
```ruby
sudo nano nginx/.htpasswd
```
+ Password erstellen und in .htpasswd einfügen
[web2generators](https://www.web2generators.com/apache-tools/htpasswd-generator)
