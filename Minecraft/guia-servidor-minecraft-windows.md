# CÓMO CREAR UN SERVIDOR DEDICADO PARA MINECRAFT JAVA | WINDOWS

![Minecraft Banner](/IMG/minecraft-background-cfljc4haleghnajo.jpg)

### TABLA DE CONTENIDOS
1. [Requisitos del Sistema](#requisitos-del-sistema)
2. [Instalación de Java](#instalación-de-java)
3. [Descarga del Servidor](#descarga-del-servidor)
4. [Configuración Inicial](#configuración-inicial)
5. [Configuración del Firewall](#configuración-del-firewall)
6. [Configuración Avanzada del Servidor](#configuración-avanzada-del-servidor)
7. [Comandos de Administración](#comandos-de-administración)
8. [Gestión de Jugadores](#gestión-de-jugadores)
9. [Configuración de Whitelist y Blacklist](#configuración-de-whitelist-y-blacklist)
10. [Solución de Problemas](#solución-de-problemas)

---

### REQUISITOS PREVIOS

**Mínimos:**
- **RAM**: 2GB (recomendado 4GB+)
- **CPU**: Procesador de 2 núcleos
- **Espacio en disco**: 1GB libre
- **Sistema Operativo**: Windows 10/11 o Windows Server 2016+

**Recomendados:**
- **RAM**: 8GB o más
- **CPU**: Procesador de 4+ núcleos
- **Espacio en disco**: 10GB+ libre
- **Conexión a Internet**: Estable con buen ancho de banda

---

### INSTALACIÓN DE JAVA

### 1. Verificar Java Instalado
```cmd
java -version
```

### 2. Descargar Java (si no está instalado)
1. Ve a [Oracle Java](https://www.oracle.com/java/technologies/downloads/) o [OpenJDK](https://adoptium.net/)
2. Descarga Java 17 o superior (recomendado Java 21)
3. Instala siguiendo el asistente

### 3. Configurar Variables de Entorno
1. Abre "Variables de entorno del sistema"
2. Agrega `JAVA_HOME` apuntando a la carpeta de Java
3. Agrega `%JAVA_HOME%\bin` al PATH

---

### DESCARGA DEL SERVIDOR

### 1. Crear Carpeta del Servidor
```cmd
mkdir C:\MinecraftServer
cd C:\MinecraftServer
```

### 2. Descargar Server.jar
```cmd
# Opción 1: Descarga directa (reemplaza VERSION con la versión deseada)
curl -o server.jar https://launcher.mojang.com/v1/objects/[HASH]/server.jar

# Opción 2: Usar PowerShell
Invoke-WebRequest -Uri "https://launcher.mojang.com/v1/objects/[HASH]/server.jar" -OutFile "server.jar"
```

### 3. Primera Ejecución
```cmd
java -Xmx2G -Xms1G -jar server.jar nogui
```

---

### CONFIGURACIÓN INICIAL

### 1. Aceptar EULA
Edita `eula.txt` y cambia:
```
eula=false
```
Por:
```
eula=true
```

### 2. Configurar server.properties
```properties
# Configuración básica
server-name=Mi Servidor Minecraft
motd=¡Bienvenido a mi servidor!
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
```

### 3. Script de Inicio (start.bat)
```batch
@echo off
title Servidor Minecraft
java -Xmx4G -Xms2G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -Dusing.aikars.flags=https://mcflags.emc.gs -Daikars.new.flags=true -jar server.jar nogui
pause
```

---

### CONFIGURACIÓN DEL FIREWALL

### 1. Abrir Puerto en Windows Firewall
```cmd
# Abrir puerto 25565
netsh advfirewall firewall add rule name="Minecraft Server" dir=in action=allow protocol=TCP localport=25565
```

### 2. Configurar Router (si es necesario)
- Accede a la configuración del router
- Configura port forwarding para el puerto 25565
- Asigna IP estática al servidor

---

### CONFIGURACIÓN AVANZADA DEL SERVIDOR

### 1. Configuración de Memoria
```batch
# Para servidores pequeños (1-5 jugadores)
java -Xmx2G -Xms1G -jar server.jar nogui

# Para servidores medianos (5-15 jugadores)
java -Xmx4G -Xms2G -jar server.jar nogui

# Para servidores grandes (15+ jugadores)
java -Xmx8G -Xms4G -jar server.jar nogui
```

### 2. Configuración de JVM Optimizada
```batch
java -Xmx4G -Xms2G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -jar server.jar nogui
```

---

### COMANDOS DE ADMINISTRACIÓN

### Comandos Básicos del Servidor
```cmd
# En la consola del servidor
help                    # Lista todos los comandos disponibles
list                    # Muestra jugadores conectados
save-all               # Guarda el mundo
stop                   # Detiene el servidor
reload                 # Recarga configuraciones
```

### Comandos de Información
```cmd
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
```cmd
# Kickear jugador
kick <jugador> [razón]

# Ejemplos:
kick Steve
kick Alex Te has portado mal
```

### Comandos de Ban
```cmd
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
```cmd
# Dar permisos de operador
op <jugador>

# Quitar permisos de operador
deop <jugador>

# Lista de operadores
list ops
```

### Comandos de Gamemode
```cmd
# Cambiar modo de juego
gamemode <modo> [jugador]

# Modos disponibles:
# survival, creative, adventure, spectator

# Ejemplos:
gamemode creative Steve
gamemode survival @a
```

### Comandos de Teleportación
```cmd
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
```cmd
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
```cmd
# Agregar a blacklist (ban permanente)
ban <jugador> [razón]

# Agregar IP a blacklist
ban-ip <IP>

# Ver lista de baneados
banlist
```

## Comandos Avanzados

### Comandos de Mundo
```cmd
# Cambiar dificultad
difficulty <nivel>
# Niveles: peaceful, easy, normal, hard

# Cambiar modo de juego por defecto
defaultgamemode <modo>

# Establecer spawn
setworldspawn <x> <y> <z>
```

### Comandos de Chat
```cmd
# Enviar mensaje a todos
say <mensaje>

# Enviar mensaje a jugador específico
tell <jugador> <mensaje>

# Cambiar formato de chat
gamerule sendCommandFeedback true
```

### Comandos de Reglas del Juego
```cmd
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

### Script de Backup (backup.bat)
```batch
@echo off
set BACKUP_DIR=C:\MinecraftBackups
set SERVER_DIR=C:\MinecraftServer
set DATE=%date:~-4,4%%date:~-10,2%%date:~-7,2%

if not exist %BACKUP_DIR% mkdir %BACKUP_DIR%

echo Creando backup...
xcopy "%SERVER_DIR%\world" "%BACKUP_DIR%\world_%DATE%" /E /I /H /Y
xcopy "%SERVER_DIR%\world_nether" "%BACKUP_DIR%\world_nether_%DATE%" /E /I /H /Y
xcopy "%SERVER_DIR%\world_the_end" "%BACKUP_DIR%\world_the_end_%DATE%" /E /I /H /Y

echo Backup completado: %DATE%
```

### Script de Reinicio Automático (restart.bat)
```batch
@echo off
echo Reiniciando servidor en 10 segundos...
timeout /t 10
taskkill /f /im java.exe
timeout /t 5
start start.bat
```

---

### SOLUCIÓN DE PROBLEMAS

### Problemas Comunes

#### 1. Error "Java no reconocido"
```cmd
# Verificar instalación de Java
java -version
javac -version

# Reinstalar Java si es necesario
```

#### 2. Puerto ya en uso
```cmd
# Verificar qué proceso usa el puerto 25565
netstat -ano | findstr :25565

# Terminar proceso si es necesario
taskkill /PID <PID> /F
```

#### 3. Servidor no responde
```cmd
# Verificar memoria disponible
wmic OS get TotalVisibleMemorySize,FreePhysicalMemory

# Reducir memoria asignada si es necesario
java -Xmx2G -Xms1G -jar server.jar nogui
```

#### 4. Jugadores no pueden conectarse
- Verificar firewall
- Verificar port forwarding
- Verificar IP pública
- Verificar que el servidor esté ejecutándose

### Logs del Servidor
```cmd
# Ver logs en tiempo real
type logs\latest.log

# Buscar errores específicos
findstr "ERROR" logs\latest.log
findstr "WARN" logs\latest.log
```

---

### CONFIGURACIÓN DE PLUGINS (OPCIONAL)

### Instalación de Bukkit/Spigot/Paper
1. Descarga PaperMC desde [papermc.io](https://papermc.io)
2. Reemplaza server.jar con paper.jar
3. Reinicia el servidor

### Plugins Recomendados
- **EssentialsX**: Comandos básicos
- **WorldGuard**: Protección de áreas
- **LuckPerms**: Sistema de permisos
- **Vault**: API de economía
- **WorldEdit**: Edición de mundo

---

### MANTENIMIENTO DEL SERVIDOR

### Tareas Regulares
1. **Backups diarios** del mundo
2. **Actualización de Java** cuando sea necesario
3. **Monitoreo de rendimiento**
4. **Limpieza de logs antiguos**
5. **Actualización del servidor** a nuevas versiones

### Comandos de Mantenimiento
```cmd
# Limpiar memoria
gc

# Ver estadísticas del servidor
tps

# Forzar guardado
save-all flush
```

---

### COMANDOS ÚTILES PARA LA CONSOLA DEL SERVIDOR

```cmd
# Comandos básicos del servidor
help                    # Lista todos los comandos disponibles
list                    # Muestra jugadores conectados
save-all               # Guarda el mundo
stop                   # Detiene el servidor
reload                 # Recarga configuraciones

# Comandos de moderación
kick <jugador> [razón]  # Expulsar jugador
ban <jugador> [razón]   # Banear jugador
pardon <jugador>        # Desbanear jugador
op <jugador>            # Dar permisos de operador
deop <jugador>          # Quitar permisos de operador

# Comandos de mundo
gamemode <modo> [jugador]  # Cambiar modo de juego
tp <jugador> <x> <y> <z>   # Teleportar jugador
time set day               # Cambiar hora a día
weather clear              # Despejar clima
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

¡Disfruta administrando tu servidor de Minecraft en Windows! 🎮⛏️
