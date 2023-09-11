# Project heating
## FEHM / NGINX Docker
### FHEM Docker
Docker-Compose für den Zugriff auf die GPIOS:
    privileged: true
    volumes:
        - /sys:/sys
    devices:
        - "/dev/gpiomem:/dev/gpiomem"
    environment:
        GPIO_GID: 997
