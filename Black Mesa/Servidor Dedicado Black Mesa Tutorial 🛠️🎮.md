# CÓMO CREAR UN SERVIDOR DEDICADO PARA BLACK MESA | DEATHMATCH & COOPERATIVO

![Jeremias Banner](https://raw.githubusercontent.com/Jeremias0618/NextLevelGaming/refs/heads/main/IMG/BM2.png)

### REQUISITOS PREVIOS

Asegúrate de tener los siguientes puertos abiertos en tu router/firewall:

```
Puertos requeridos: 27015 (UDP-TCP) y 27005 (UDP)
```

Esto permitirá que otros jugadores puedan conectarse a tu servidor.

---

### DESCARGAR LA HERRAMIENTA "BLACK MESA DEDICATED SERVER"

1. Descarga la herramienta desde Steam: **Black Mesa Dedicated Server**
2. Una vez instalada, dirígete a la ruta donde se encuentra el archivo `srcds.exe`.
3. Configura `srcds.exe` para que siempre se ejecute como administrador.

---

### CONFIGURACIÓN INICIAL DEL SERVIDOR

1. Abre la carpeta de instalación y entra a la ruta:

```
Black Mesa Dedicated Server\bms\cfg
```

2. Dentro de la carpeta `cfg`, ubica el archivo `server.cfg` y ábrelo para editar.

---

### ARCHIVO DE CONFIGURACIÓN `server.cfg`

```
hostname "Yeremi Server - Black Mesa"    // Nombre del servidor visible para los jugadores
rcon_password "admin123"                 // Contraseña para acceso remoto a la consola (RCON)
sv_password ""                           // Contraseña para ingresar al servidor (déjala vacía si será público)
sv_lan 0                                 // 0 = en línea/público, 1 = solo red local (LAN)
sv_cheats 0                              // 0 = trucos desactivados, 1 = trucos activados
mp_autoteambalance 1                     // Activa el balance automático de equipos
mp_friendlyfire 0                        // Desactiva daño entre compañeros
sv_region 255                            // Región del servidor (255 = automático/mundial)
```

Guarda los cambios realizados antes de cerrar el archivo.

---

### ABRIR LA RUTA DEL SERVIDOR DESDE CMD

Usa el siguiente comando en la consola (CMD) para ir a la carpeta del servidor:

```
cd "C:\Program Files (x86)\Steam\steamapps\common\Black Mesa Dedicated Server"
```

💡 *La ubicación puede variar según dónde instalaste la herramienta.*

---

### INICIAR EL SERVIDOR CON COMANDOS

Usa la siguiente línea de comando para iniciar el servidor online:

```
srcds.exe -console -game bms +ip 192.168.18.1 +map dm_crossfire +maxplayers 32 -port 27015
```

---

### DETALLE DE CADA PARÁMETRO

```
srcds.exe                 // Ejecuta el servidor dedicado
-console                  // Modo consola (sin interfaz gráfica)
-game bms                 // Especifica el juego: Black Mesa
+ip 192.168.18.1          // IP local del servidor (reemplaza con la IP de tu PC)
+map dm_crossfire         // Mapa inicial del servidor
+maxplayers 32            // Número máximo de jugadores
-port 27015               // Puerto por defecto del servidor
```

---

### MAPAS DISPONIBLES PARA DEATHMATCH

```
dm_crossfire    dm_gasworks    dm_stalkyard    dm_lambdabunker
dm_chopper      dm_bounce      dm_power        dm_boot_camp
dm_longjump     dm_office      dm_rail         dm_dynamo
dm_chthon       dm_metal       dm_stack        dm_subway
```

---

### CONECTARSE A TU SERVIDOR

Para que otros jugadores se conecten a tu servidor, deben ingresar la IP pública seguida del puerto 27015 en la consola del juego Black Mesa:

```
connect 205.125.85.66:27015
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

Si tu servidor no tiene contraseña, podría aparecer automáticamente en la lista pública de servidores, permitiendo que cualquier jugador se una libremente.

---

### COMANDOS ÚTILES PARA LA CONSOLA DEL SERVIDOR

```
status                          // Muestra información del servidor
changelevel nombre_del_mapa     // Cambia al mapa indicado sin reiniciar
kick nombre_del_jugador         // Expulsa a un jugador
addip 0 ip_del_jugador          // Banea una IP
banid 0 steamid64               // Banea por SteamID
removeip ip_del_jugador         // Quita el baneo por IP
removeid steamid64              // Quita el baneo por SteamID
hostname "Nuevo Nombre"         // Cambia el nombre del servidor en tiempo real
sv_password "clave"             // Establece una contraseña para entrar
sv_password ""                  // Elimina la contraseña
mp_timelimit minutos            // Establece duración del mapa en minutos
mp_falldamage 1                 // Activa daño realista por caída
```
