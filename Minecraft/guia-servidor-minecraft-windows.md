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

---

## CONFIGURACIÓN DETALLADA DE SERVER.PROPERTIES

### Configuración Básica del Servidor
```properties
# Mensaje del día (MOTD) - aparece en la lista de servidores
motd=A Minecraft Server

# Puerto del servidor (por defecto 25565)
server-port=25565

# Número máximo de jugadores conectados simultáneamente
max-players=20

# IP del servidor (dejar vacío para todas las interfaces)
server-ip=
```

### Configuración de Juego
```properties
# Dificultad del juego
# Valores: peaceful, easy, normal, hard
difficulty=easy

# Modo de juego por defecto para nuevos jugadores
# Valores: survival, creative, adventure, spectator
gamemode=survival

# Si el servidor está en modo hardcore (una vida)
# true = hardcore, false = normal
hardcore=false

# Si los jugadores pueden volar (modo creativo)
allow-flight=false

# Si se fuerza el modo de juego especificado
force-gamemode=false

# Si se generan estructuras (aldeas, fortalezas, etc.)
generate-structures=true
```

### Configuración de Red y Seguridad
```properties
# Verificación de cuentas premium de Minecraft
# true = solo jugadores con cuenta premium, false = permite piratas
online-mode=true

# Lista blanca de jugadores
# true = solo jugadores en whitelist pueden entrar
white-list=false

# Si se aplica la whitelist estrictamente
enforce-whitelist=false

# Límite de conexiones por segundo por IP
# 0 = sin límite
rate-limit=0

# Si se previenen conexiones a través de proxy
prevent-proxy-connections=false

# Si se registran las IPs de los jugadores
log-ips=true
```

### Configuración de Mundo
```properties
# Nombre de la carpeta del mundo principal
level-name=world

# Semilla del mundo (dejar vacío para aleatoria)
level-seed=

# Tipo de generación del mundo
# Valores: minecraft:normal, minecraft:flat, minecraft:large_biomes, minecraft:amplified
level-type=minecraft:normal

# Configuración personalizada del generador (JSON)
generator-settings={}

# Tamaño máximo del mundo en bloques
max-world-size=29999984
```

### Configuración de Spawn y Protección
```properties
# Radio de protección del spawn (en bloques)
# 0 = sin protección, 16 = protección de 16 bloques
spawn-protection=16

# Si se ocultan jugadores en línea
hide-online-players=false

# Tiempo de pausa cuando no hay jugadores (segundos)
pause-when-empty-seconds=60

# Tiempo de inactividad antes de kickear jugador (0 = deshabilitado)
player-idle-timeout=0
```

### Configuración de Comandos y Permisos
```properties
# Si están habilitados los bloques de comando
enable-command-block=false

# Nivel de permisos para operadores
# 1-4, donde 4 es el más alto
op-permission-level=4

# Nivel de permisos para funciones
# 1-4, donde 4 es el más alto
function-permission-level=2
```

### Configuración de RCON (Control Remoto)
```properties
# Habilitar RCON para control remoto
enable-rcon=false

# Puerto para RCON
rcon.port=25575

# Contraseña para RCON (cambiar por seguridad)
rcon.password=
```

### Configuración de Query
```properties
# Puerto para consultas de estado
query.port=25565

# Habilitar consultas de estado
enable-query=false
```

### Configuración de Rendimiento
```properties
# Tiempo máximo por tick en milisegundos
# Si se excede, el servidor se reinicia
max-tick-time=60000

# Si se sincronizan las escrituras de chunks
sync-chunk-writes=true

# Distancia de simulación (chunks)
# Valores: 4-32, menor = mejor rendimiento
simulation-distance=10

# Distancia de renderizado (chunks)
# Valores: 3-32, menor = mejor rendimiento
view-distance=10

# Porcentaje del rango de transmisión de entidades
# 100 = rango completo, menor = mejor rendimiento
entity-broadcast-range-percentage=100

# Número máximo de actualizaciones de vecinos encadenados
max-chained-neighbor-updates=1000000

# Si se usa transporte nativo
use-native-transport=true

# Umbral de compresión de red (bytes)
# Paquetes más grandes que este valor se comprimen
network-compression-threshold=256
```

### Configuración de Monitoreo
```properties
# Habilitar monitoreo JMX
enable-jmx-monitoring=false

# Habilitar estado del servidor
enable-status=true

# Si se envían datos de uso a Mojang
snooper-enabled=true

# Intervalo de latido de estado (0 = deshabilitado)
status-heartbeat-interval=0
```

### Configuración de Chat y Mensajes
```properties
# Si se transmiten mensajes de consola a operadores
broadcast-console-to-ops=true

# Si se transmiten mensajes RCON a operadores
broadcast-rcon-to-ops=true

# Si se envía feedback de comandos
send-command-feedback=true
```

### Configuración de Resource Packs
```properties
# URL del resource pack (opcional)
resource-pack=

# Hash SHA1 del resource pack (opcional)
resource-pack-sha1=

# ID del resource pack (opcional)
resource-pack-id=

# Mensaje de prompt del resource pack
resource-pack-prompt=

# Si se requiere el resource pack
require-resource-pack=false

# Nivel de compresión del pack (1-9)
pack-compression-level=3
```

### Configuración de Gestión de Servidor
```properties
# Si se aceptan transferencias
accepts-transfers=false

# Habilitar servidor de gestión
management-server-enabled=false

# Host del servidor de gestión
management-server-host=localhost

# Puerto del servidor de gestión
management-server-port=0

# Secreto del servidor de gestión
management-server-secret=G8D2BbquPLgf4n7luB10xg7PKpzPqFy2aWSw4gFF

# Habilitar TLS en servidor de gestión
management-server-tls-enabled=true

# Almacén de claves TLS
management-server-tls-keystore=

# Contraseña del almacén de claves TLS
management-server-tls-keystore-password=
```

### Configuración de Código de Conducta
```properties
# Habilitar código de conducta
enable-code-of-conduct=false
```

### Configuración de Perfil Seguro
```properties
# Forzar perfil seguro
enforce-secure-profile=true
```

### Configuración de Packs de Datos
```properties
# Packs de datos iniciales deshabilitados
initial-disabled-packs=

# Packs de datos iniciales habilitados
initial-enabled-packs=vanilla
```

### Configuración de Compresión de Regiones
```properties
# Algoritmo de compresión de archivos de región
# Valores: deflate, lz4, none
region-file-compression=deflate
```

### Configuración de Filtrado de Texto
```properties
# Configuración de filtrado de texto
text-filtering-config=

# Versión de filtrado de texto
text-filtering-version=0
```

### Configuración de Enlaces
```properties
# Enlace para reportar bugs
bug-report-link=
```

### Ejemplo de Configuración Completa
```properties
# Configuración básica
motd=A Minecraft Server
server-port=25565
server-ip=
max-players=20

# Configuración de juego
difficulty=easy
gamemode=survival
hardcore=false
allow-flight=false
force-gamemode=false
generate-structures=true

# Configuración de red y seguridad
online-mode=true
white-list=false
enforce-whitelist=false
rate-limit=0
prevent-proxy-connections=false
log-ips=true

# Configuración de mundo
level-name=world
level-seed=
level-type=minecraft:normal
generator-settings={}
max-world-size=29999984

# Configuración de spawn y protección
spawn-protection=16
hide-online-players=false
pause-when-empty-seconds=60
player-idle-timeout=0

# Configuración de comandos y permisos
op-permission-level=4
function-permission-level=2

# Configuración de RCON
enable-rcon=false
rcon.port=25575
rcon.password=

# Configuración de Query
query.port=25565
enable-query=false

# Configuración de rendimiento
max-tick-time=60000
sync-chunk-writes=true
simulation-distance=10
view-distance=10
entity-broadcast-range-percentage=100
max-chained-neighbor-updates=1000000
use-native-transport=true
network-compression-threshold=256

# Configuración de monitoreo
enable-jmx-monitoring=false
enable-status=true
status-heartbeat-interval=0

# Configuración de chat y mensajes
broadcast-console-to-ops=true
broadcast-rcon-to-ops=true

# Configuración de resource packs
resource-pack=
resource-pack-sha1=
resource-pack-id=
resource-pack-prompt=
require-resource-pack=false

# Configuración de gestión de servidor
accepts-transfers=false
management-server-enabled=false
management-server-host=localhost
management-server-port=0
management-server-secret=VUw7hKtVTUJdEq5usVgyAE6aMQDcTNtvBHAn5ko9
management-server-tls-enabled=true
management-server-tls-keystore=
management-server-tls-keystore-password=

# Configuración de código de conducta
enable-code-of-conduct=false

# Configuración de perfil seguro
enforce-secure-profile=true

# Configuración de packs de datos
initial-disabled-packs=
initial-enabled-packs=vanilla

# Configuración de compresión de regiones
region-file-compression=deflate

# Configuración de filtrado de texto
text-filtering-config=
text-filtering-version=0

# Configuración de enlaces
bug-report-link=
```

### Consejos de Configuración

#### Para Servidores Públicos
```properties
# Configuración recomendada para servidores públicos
online-mode=true
white-list=false
pvp=true
allow-flight=false
enable-command-block=false
op-permission-level=2
```

#### Para Servidores Privados
```properties
# Configuración recomendada para servidores privados
online-mode=false
white-list=true
pvp=false
allow-flight=true
enable-command-block=true
op-permission-level=4
```

#### Para Mejor Rendimiento
```properties
# Configuración para mejor rendimiento
simulation-distance=8
entity-broadcast-range-percentage=80
sync-chunk-writes=false
snooper-enabled=false
```

#### Para Servidores de Construcción
```properties
# Configuración para servidores de construcción
gamemode=creative
allow-flight=true
pvp=false
spawn-monsters=false
difficulty=peaceful
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
