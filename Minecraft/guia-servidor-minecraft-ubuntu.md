# CÓMO CREAR UN SERVIDOR DEDICADO PARA MINECRAFT JAVA | UBUNTU SERVER 22.04

![Minecraft Banner](/IMG/minecraft-background-cfljc4haleghnajo.jpg)

### TABLA DE CONTENIDOS
1. [Requisitos del Sistema](#requisitos-del-sistema)
2. [Preparación del Sistema](#preparación-del-sistema)
3. [Instalación de Java](#instalación-de-java)
4. [Creación de Usuario para Minecraft](#creación-de-usuario-para-minecraft)
5. [Descarga y Configuración del Servidor](#descarga-y-configuración-del-servidor)
6. [Configuración del Firewall](#configuración-del-firewall)
7. [Configuración Avanzada del Servidor](#configuración-avanzada-del-servidor)
8. [Configuración como Servicio del Sistema](#configuración-como-servicio-del-sistema)
9. [Comandos de Administración](#comandos-de-administración)
10. [Gestión de Jugadores](#gestión-de-jugadores)
11. [Configuración de Whitelist y Blacklist](#configuración-de-whitelist-y-blacklist)
12. [Automatización y Scripts](#automatización-y-scripts)
13. [Solución de Problemas](#solución-de-problemas)
14. [Mantenimiento del Servidor](#mantenimiento-del-servidor)

---

### REQUISITOS PREVIOS

**Mínimos:**
- **RAM**: 2GB (recomendado 4GB+)
- **CPU**: 2 núcleos
- **Espacio en disco**: 2GB libre
- **Sistema Operativo**: Ubuntu Server 22.04 LTS

**Recomendados:**
- **RAM**: 8GB o más
- **CPU**: 4+ núcleos
- **Espacio en disco**: 20GB+ libre
- **Conexión a Internet**: Estable con buen ancho de banda

---

### PREPARACIÓN DEL SISTEMA

### 1. Actualizar el Sistema
```bash
sudo apt update && sudo apt upgrade -y
```

### 2. Instalar Herramientas Básicas
```bash
sudo apt install -y curl wget unzip screen htop nano vim
```

### 3. Configurar Zona Horaria
```bash
sudo timedatectl set-timezone America/Mexico_City
# O tu zona horaria preferida
```

---

### INSTALACIÓN DE JAVA

### Opción 1: OpenJDK (Recomendado)
```bash
# Instalar OpenJDK 21
sudo apt install -y openjdk-21-jdk

# Verificar instalación
java -version
javac -version
```

### Opción 2: Oracle JDK
```bash
# Agregar repositorio de Oracle
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update

# Instalar Oracle JDK 21
sudo apt install -y oracle-java21-installer

# Configurar como predeterminado
sudo update-alternatives --config java
```

### Verificar Instalación
```bash
# Ver versión de Java
java -version

# Ver ubicación de Java
which java
readlink -f $(which java)
```

---

### CREACIÓN DE USUARIO PARA MINECRAFT

### 1. Crear Usuario Dedicado
```bash
# Crear usuario minecraft
sudo useradd -r -m -U -d /opt/minecraft -s /bin/bash minecraft

# Cambiar al usuario minecraft
sudo su - minecraft
```

### 2. Crear Estructura de Directorios
```bash
# Crear directorios necesarios
mkdir -p /opt/minecraft/{server,backups,scripts,logs}
cd /opt/minecraft/server
```

---

### DESCARGA Y CONFIGURACIÓN DEL SERVIDOR

### 1. Descargar Server.jar
```bash
# Descargar la última versión (reemplaza con la URL actual)
wget https://launcher.mojang.com/v1/objects/[HASH]/server.jar -O server.jar

# O usar PaperMC (recomendado para mejor rendimiento)
wget https://api.papermc.io/v2/projects/paper/versions/1.20.4/builds/445/downloads/paper-1.20.4-445.jar -O server.jar
```

### 2. Primera Ejecución
```bash
# Ejecutar servidor por primera vez
java -Xmx2G -Xms1G -jar server.jar nogui
```

### 3. Configurar EULA
```bash
# Editar eula.txt
nano eula.txt

# Cambiar eula=false por eula=true
```

### 4. Configurar server.properties
```bash
nano server.properties
```

```properties
# Configuración básica del servidor
server-name=Mi Servidor Ubuntu
motd=¡Bienvenido a mi servidor Ubuntu!
server-port=25565
max-players=20
difficulty=normal
gamemode=survival
hardcore=false
pvp=true
allow-flight=false
online-mode=true
white-list=false
enforce-whitelist=false
enable-command-block=false
enable-query=false
enable-rcon=false
rcon.port=25575
rcon.password=
level-name=world
level-seed=
level-type=minecraft\:normal
generator-settings={}
allow-nether=true
level-format=default
enable-jmx-monitoring=false
enable-status=true
broadcast-console-to-ops=true
enable-rcon=false
sync-chunk-writes=true
enable-command-block=false
op-permission-level=4
function-permission-level=2
max-tick-time=60000
force-gamemode=false
rate-limit=0
hardcore=false
white-list=false
broadcast-console-to-ops=true
enable-command-block=false
spawn-npcs=true
spawn-animals=true
spawn-monsters=true
snooper-enabled=true
resource-pack=
resource-pack-sha1=
pack-compression-level=3
entity-broadcast-range-percentage=100
simulation-distance=10
```

---

### CONFIGURACIÓN DEL FIREWALL

### 1. Configurar UFW (Uncomplicated Firewall)
```bash
# Habilitar UFW
sudo ufw enable

# Permitir SSH
sudo ufw allow ssh

# Permitir puerto de Minecraft
sudo ufw allow 25565/tcp

# Verificar estado
sudo ufw status
```

### 2. Configurar iptables (Alternativa)
```bash
# Permitir puerto 25565
sudo iptables -A INPUT -p tcp --dport 25565 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 25565 -j ACCEPT

# Guardar reglas
sudo iptables-save > /etc/iptables/rules.v4
```

---

### CONFIGURACIÓN AVANZADA DEL SERVIDOR

### 1. Script de Inicio Optimizado
```bash
# Crear script de inicio
nano /opt/minecraft/scripts/start.sh
```

```bash
#!/bin/bash

# Configuración del servidor
SERVER_DIR="/opt/minecraft/server"
JAR_FILE="server.jar"
MIN_RAM="2G"
MAX_RAM="4G"
JAVA_OPTS="-XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1"

# Cambiar al directorio del servidor
cd $SERVER_DIR

# Iniciar servidor
java -Xms$MIN_RAM -Xmx$MAX_RAM $JAVA_OPTS -jar $JAR_FILE nogui
```

### 2. Hacer Ejecutable el Script
```bash
chmod +x /opt/minecraft/scripts/start.sh
```

### 3. Configuración de Memoria por Tamaño de Servidor
```bash
# Servidor pequeño (1-5 jugadores)
java -Xms1G -Xmx2G -jar server.jar nogui

# Servidor mediano (5-15 jugadores)
java -Xms2G -Xmx4G -jar server.jar nogui

# Servidor grande (15+ jugadores)
java -Xms4G -Xmx8G -jar server.jar nogui
```

---

### CONFIGURACIÓN COMO SERVICIO DEL SISTEMA

### 1. Crear Archivo de Servicio
```bash
sudo nano /etc/systemd/system/minecraft.service
```

```ini
[Unit]
Description=Minecraft Server
After=network.target

[Service]
Type=simple
User=minecraft
WorkingDirectory=/opt/minecraft/server
ExecStart=/usr/bin/java -Xms2G -Xmx4G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -jar server.jar nogui
ExecStop=/bin/kill -15 $MAINPID
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

### 2. Habilitar y Iniciar el Servicio
```bash
# Recargar systemd
sudo systemctl daemon-reload

# Habilitar servicio
sudo systemctl enable minecraft

# Iniciar servicio
sudo systemctl start minecraft

# Verificar estado
sudo systemctl status minecraft
```

### 3. Comandos de Gestión del Servicio
```bash
# Iniciar servidor
sudo systemctl start minecraft

# Detener servidor
sudo systemctl stop minecraft

# Reiniciar servidor
sudo systemctl restart minecraft

# Ver estado
sudo systemctl status minecraft

# Ver logs
sudo journalctl -u minecraft -f
```

---

### COMANDOS DE ADMINISTRACIÓN

### Comandos Básicos del Servidor
```bash
# Conectarse a la consola del servidor
sudo systemctl status minecraft

# Ver logs en tiempo real
sudo journalctl -u minecraft -f

# Comandos en la consola del servidor
help                    # Lista todos los comandos disponibles
list                    # Muestra jugadores conectados
save-all               # Guarda el mundo
stop                   # Detiene el servidor
reload                 # Recarga configuraciones
```

### Comandos de Información
```bash
# En la consola del servidor
seed                   # Muestra la semilla del mundo
time set day           # Cambia la hora a día
time set night         # Cambia la hora a noche
weather clear          # Despeja el clima
weather rain           # Activa la lluvia
weather thunder        # Activa tormenta
```

---

### GESTIÓN DE JUGADORES

### Comandos de Moderación
```bash
# Kickear jugador
kick <jugador> [razón]

# Ejemplos:
kick Steve
kick Alex Te has portado mal
```

### Comandos de Ban
```bash
# Banear jugador
ban <jugador> [razón]

# Desbanear jugador
pardon <jugador>

# Banear IP
ban-ip <IP>

# Desbanear IP
pardon-ip <IP>

# Lista de baneados
banlist
```

### Comandos de Op (Operador)
```bash
# Dar permisos de operador
op <jugador>

# Quitar permisos de operador
deop <jugador>

# Lista de operadores
list ops
```

### Comandos de Gamemode
```bash
# Cambiar modo de juego
gamemode <modo> [jugador]

# Modos disponibles:
# survival, creative, adventure, spectator

# Ejemplos:
gamemode creative Steve
gamemode survival @a
```

### Comandos de Teleportación
```bash
# Teleportar jugador
tp <jugador1> <jugador2>
tp <jugador> <x> <y> <z>

# Ejemplos:
tp Steve Alex
tp Steve 100 64 200
```

---

### CONFIGURACIÓN DE WHITELIST Y BLACKLIST

### Whitelist (Lista Blanca)
```bash
# Activar whitelist
whitelist on

# Desactivar whitelist
whitelist off

# Agregar jugador a whitelist
whitelist add <jugador>

# Remover jugador de whitelist
whitelist remove <jugador>

# Lista de jugadores en whitelist
whitelist list

# Recargar whitelist
whitelist reload
```

### Configuración en server.properties
```properties
# Activar whitelist
white-list=true

# Forzar whitelist (no permite jugadores no listados)
enforce-whitelist=true
```

### Blacklist (Lista Negra)
```bash
# Agregar a blacklist (ban permanente)
ban <jugador> [razón]

# Agregar IP a blacklist
ban-ip <IP>

# Ver lista de baneados
banlist
```

## Comandos Avanzados

### Comandos de Mundo
```bash
# Cambiar dificultad
difficulty <nivel>
# Niveles: peaceful, easy, normal, hard

# Cambiar modo de juego por defecto
defaultgamemode <modo>

# Establecer spawn
setworldspawn <x> <y> <z>
```

### Comandos de Chat
```bash
# Enviar mensaje a todos
say <mensaje>

# Enviar mensaje a jugador específico
tell <jugador> <mensaje>

# Cambiar formato de chat
gamerule sendCommandFeedback true
```

### Comandos de Reglas del Juego
```bash
# Ver reglas del juego
gamerule

# Cambiar reglas
gamerule <regla> <valor>

# Ejemplos:
gamerule keepInventory true
gamerule doDaylightCycle false
gamerule doMobSpawning false
gamerule doFireTick false
gamerule mobGriefing false
```

---

### AUTOMATIZACIÓN Y SCRIPTS

### Script de Backup
```bash
# Crear script de backup
nano /opt/minecraft/scripts/backup.sh
```

```bash
#!/bin/bash

# Configuración
SERVER_DIR="/opt/minecraft/server"
BACKUP_DIR="/opt/minecraft/backups"
DATE=$(date +%Y%m%d_%H%M%S)
RETENTION_DAYS=7

# Crear directorio de backup si no existe
mkdir -p $BACKUP_DIR

# Detener servidor temporalmente
sudo systemctl stop minecraft

# Crear backup
tar -czf $BACKUP_DIR/minecraft_backup_$DATE.tar.gz -C $SERVER_DIR world world_nether world_the_end

# Reiniciar servidor
sudo systemctl start minecraft

# Limpiar backups antiguos
find $BACKUP_DIR -name "minecraft_backup_*.tar.gz" -mtime +$RETENTION_DAYS -delete

echo "Backup completado: $DATE"
```

### Script de Reinicio Automático
```bash
# Crear script de reinicio
nano /opt/minecraft/scripts/restart.sh
```

```bash
#!/bin/bash

echo "Reiniciando servidor Minecraft..."
sudo systemctl restart minecraft
echo "Servidor reiniciado"
```

### Configurar Cron Jobs
```bash
# Editar crontab
sudo crontab -e

# Agregar tareas programadas
# Backup diario a las 3:00 AM
0 3 * * * /opt/minecraft/scripts/backup.sh

# Reinicio semanal los domingos a las 4:00 AM
0 4 * * 0 /opt/minecraft/scripts/restart.sh
```

### Hacer Ejecutables los Scripts
```bash
chmod +x /opt/minecraft/scripts/*.sh
```

---

### SOLUCIÓN DE PROBLEMAS

### Problemas Comunes

#### 1. Error "Java no encontrado"
```bash
# Verificar instalación de Java
java -version
which java

# Reinstalar Java si es necesario
sudo apt install --reinstall openjdk-21-jdk
```

#### 2. Puerto ya en uso
```bash
# Verificar qué proceso usa el puerto 25565
sudo netstat -tulpn | grep :25565
sudo lsof -i :25565

# Terminar proceso si es necesario
sudo kill -9 <PID>
```

#### 3. Problemas de Permisos
```bash
# Verificar permisos del usuario minecraft
sudo chown -R minecraft:minecraft /opt/minecraft
sudo chmod -R 755 /opt/minecraft
```

#### 4. Servidor no responde
```bash
# Verificar memoria disponible
free -h
htop

# Verificar logs del sistema
sudo journalctl -u minecraft -n 50
```

#### 5. Jugadores no pueden conectarse
```bash
# Verificar firewall
sudo ufw status

# Verificar que el servicio esté ejecutándose
sudo systemctl status minecraft

# Verificar logs del servidor
tail -f /opt/minecraft/server/logs/latest.log
```

### Logs del Servidor
```bash
# Ver logs en tiempo real
tail -f /opt/minecraft/server/logs/latest.log

# Buscar errores específicos
grep -i "error" /opt/minecraft/server/logs/latest.log
grep -i "warn" /opt/minecraft/server/logs/latest.log

# Ver logs del sistema
sudo journalctl -u minecraft -f
```

---

### CONFIGURACIÓN DE PLUGINS (OPCIONAL)

### Instalación de PaperMC
```bash
# Descargar PaperMC
cd /opt/minecraft/server
wget https://api.papermc.io/v2/projects/paper/versions/1.20.4/builds/445/downloads/paper-1.20.4-445.jar -O server.jar

# Reiniciar servidor
sudo systemctl restart minecraft
```

### Plugins Recomendados
```bash
# Crear directorio de plugins
mkdir -p /opt/minecraft/server/plugins

# Descargar plugins esenciales
cd /opt/minecraft/server/plugins

# EssentialsX
wget https://github.com/EssentialsX/Essentials/releases/download/2.20.1/EssentialsX-2.20.1.jar

# WorldGuard
wget https://dev.bukkit.org/projects/worldguard/files/latest

# LuckPerms
wget https://github.com/lucko/LuckPerms/releases/download/v5.4.101/luckperms-bukkit-5.4.101.jar
```

---

### MANTENIMIENTO DEL SERVIDOR

### Tareas Regulares
```bash
# Script de mantenimiento
nano /opt/minecraft/scripts/maintenance.sh
```

```bash
#!/bin/bash

echo "Iniciando mantenimiento del servidor..."

# Limpiar logs antiguos
find /opt/minecraft/server/logs -name "*.log.gz" -mtime +30 -delete

# Limpiar backups antiguos
find /opt/minecraft/backups -name "*.tar.gz" -mtime +30 -delete

# Verificar espacio en disco
df -h /opt/minecraft

# Verificar memoria
free -h

# Verificar estado del servicio
sudo systemctl status minecraft

echo "Mantenimiento completado"
```

### Comandos de Mantenimiento
```bash
# Limpiar memoria del servidor
sudo systemctl restart minecraft

# Ver estadísticas del sistema
htop
iotop
nethogs

# Verificar espacio en disco
df -h
du -sh /opt/minecraft/*
```

### Monitoreo del Servidor
```bash
# Script de monitoreo
nano /opt/minecraft/scripts/monitor.sh
```

```bash
#!/bin/bash

# Verificar si el servidor está ejecutándose
if ! systemctl is-active --quiet minecraft; then
    echo "Servidor no está ejecutándose, reiniciando..."
    sudo systemctl start minecraft
fi

# Verificar memoria
MEMORY_USAGE=$(free | grep Mem | awk '{printf("%.2f", $3/$2 * 100.0)}')
if (( $(echo "$MEMORY_USAGE > 90" | bc -l) )); then
    echo "Uso de memoria alto: $MEMORY_USAGE%"
fi

# Verificar espacio en disco
DISK_USAGE=$(df /opt/minecraft | tail -1 | awk '{print $5}' | sed 's/%//')
if [ $DISK_USAGE -gt 90 ]; then
    echo "Espacio en disco bajo: $DISK_USAGE%"
fi
```

---

### CONFIGURACIÓN DE SSH PARA ADMINISTRACIÓN REMOTA

### 1. Configurar SSH
```bash
# Instalar OpenSSH si no está instalado
sudo apt install openssh-server

# Configurar SSH
sudo nano /etc/ssh/sshd_config
```

### 2. Configurar Acceso sin Contraseña
```bash
# Generar clave SSH
ssh-keygen -t rsa -b 4096

# Copiar clave pública al servidor
ssh-copy-id usuario@ip_del_servidor
```

### 3. Script de Administración Remota
```bash
# Crear script para administración remota
nano /opt/minecraft/scripts/admin.sh
```

```bash
#!/bin/bash

case $1 in
    start)
        sudo systemctl start minecraft
        echo "Servidor iniciado"
        ;;
    stop)
        sudo systemctl stop minecraft
        echo "Servidor detenido"
        ;;
    restart)
        sudo systemctl restart minecraft
        echo "Servidor reiniciado"
        ;;
    status)
        sudo systemctl status minecraft
        ;;
    logs)
        sudo journalctl -u minecraft -f
        ;;
    backup)
        /opt/minecraft/scripts/backup.sh
        ;;
    *)
        echo "Uso: $0 {start|stop|restart|status|logs|backup}"
        exit 1
        ;;
esac
```

---

### COMANDOS ÚTILES PARA LA CONSOLA DEL SERVIDOR

```bash
# Gestión del servicio
sudo systemctl start minecraft
sudo systemctl stop minecraft
sudo systemctl restart minecraft
sudo systemctl status minecraft

# Ver logs
sudo journalctl -u minecraft -f
tail -f /opt/minecraft/server/logs/latest.log

# Backup manual
/opt/minecraft/scripts/backup.sh

# Administración remota
ssh usuario@servidor '/opt/minecraft/scripts/admin.sh status'
```

---

### CONECTARSE A TU SERVIDOR

Para que otros jugadores se conecten a tu servidor, deben ingresar la IP pública seguida del puerto 25565 en la consola del juego Minecraft:

```
connect 205.125.85.66:25565
```

📌 *Reemplaza el ejemplo con tu propia IP pública.*

---

### ¿CÓMO SABER TU IP PÚBLICA?

Puedes consultar tu IP pública en el siguiente sitio web:

```
https://canyouseeme.org/
```

---

### VISIBILIDAD DEL SERVIDOR

Si tu servidor no tiene whitelist activada, podría aparecer automáticamente en la lista pública de servidores, permitiendo que cualquier jugador se una libremente.

---

¡Disfruta administrando tu servidor de Minecraft en Ubuntu! 🎮⛏️
