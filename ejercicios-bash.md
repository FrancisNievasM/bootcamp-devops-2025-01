# Ejercicios de Bash en Linux

## Ejercicio 1

### a) Identificación de Archivos Grandes
```bash
cd /var/log
ls -lhS | grep -v '^d' | head -n 5
```
**Explicación:**
- `ls -lhS`: Lista archivos por tamaño (-S) con tamaños legibles (-h).
- `grep -v '^d'`: Muestra solo archivos (excluye directorios).
- `head -n 5`: Muestra los cinco primeros resultados.

---

### b) Archivos Recientemente Modificados
```bash
ls -lt | grep -v '^d' | head -n 5
```
**Explicación:**
- `ls -lt`: Lista por fecha de modificación (-t) en orden descendente.
- `grep -v '^d'`: Filtra para mostrar solo archivos.
- `head -n 5`: Muestra los cinco primeros resultados.

---

### c) Monitorización de Archivos Dinámicos
```bash
tail -n 20 syslog
```
Esto muestra las últimas 20 líneas del archivo `syslog`.

```bash
tail -f syslog
```
**Explicación:**
- `tail -f`: Muestra en tiempo real las líneas que se añaden al archivo.

---

### d) Búsqueda de Términos en Archivos
```bash
grep -rl "DHCP"
```
**Explicación:**
- `grep -r`: Busca recursivamente dentro de los archivos.
- `-l`: Devuelve solo los nombres de los archivos que contienen el término.
- Opción adicional: Usar `-i` para realizar una búsqueda que no distinga entre mayúsculas y minúsculas.

---

### e) Copia de Archivos Grandes
```bash
mkdir ~/logs_large
find /var/log -type f -size +4k -exec cp {} ~/logs_large/ \;
```
**Explicación:**
- `find /var/log -type f`: Busca solo archivos en el directorio `/var/log`.
- `-size +4k`: Filtra archivos mayores a 4 KB.
- `-exec cp {} ~/logs_large/ \;`: Copia cada archivo encontrado a la carpeta `~/logs_large`.

---

### f) Copia de Archivos Basado en Contenido
```bash
mkdir ~/logs_system
grep -rl "system" /var/log | xargs -I {} cp {} ~/logs_system/
```
**Explicación:**
- `grep -rl "system" /var/log`: Busca archivos que contienen el término "system".
- `xargs -I {} cp {}`: Copia cada archivo encontrado a la carpeta `~/logs_system`.

---

### g) Copia de Archivos por Fecha de Modificación
```bash
mkdir ~/logs_jan8
find /var/log -type f -newermt "2025-01-08" ! -newermt "2025-01-09" -exec cp {} ~/logs_jan8/ \;
```
**Explicación:**
- `-newermt "2025-01-08"`: Filtra archivos modificados desde el 8 de enero de 2025.
- `! -newermt "2025-01-09"`: Excluye archivos modificados después del 8 de enero de 2025.

---

## Ejercicio 2: Espacio Disponible
### Consulta del Espacio Libre en Particiones
```bash
df -h
```
**Explicación:**
- `df`: Muestra el uso de espacio en disco.
- `-h`: Presenta la salida en un formato legible para humanos (por ejemplo, usando KB, MB o GB).

**Ejemplo de Salida:**
```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   20G   30G  40% /
tmpfs           1.9G     0  1.9G   0% /dev/shm
```
En esta salida:
- **Filesystem**: El sistema de archivos o partición.
- **Size**: Tamaño total de la partición.
- **Used**: Espacio utilizado.
- **Avail**: Espacio disponible.
- **Use%**: Porcentaje de uso del espacio.
- **Mounted on**: Punto de montaje de la partición.

---

## Ejercicio 3: Exploración del Espacio de Archivos

### a) Carpeta Más Pesada en Raíz
Desde la raíz del sistema (`/`), encuentra cuál es la carpeta que ocupa más espacio:
```bash
du -sh /* | sort -rh | head -n 1
```
**Explicación:**
- `du -sh /*`: Muestra el tamaño total de cada carpeta en la raíz (`/`) en un formato legible (-h).
- `sort -rh`: Ordena los resultados por tamaño en orden descendente.
- `head -n 1`: Muestra solo la carpeta más pesada.

---

### b) Carpeta Más Pesada en un Nivel de Subdirectorios
Identifica cuál es la carpeta más pesada considerando tres niveles de subdirectorios dentro de `/`:
```bash
du -sh /*/*/* 2>/dev/null | sort -rh | head -n 1
```
**Explicación:**
- `du -sh /*/*/*`: Calcula el tamaño de carpetas con tres niveles de subdirectorios desde la raíz (`/`).
- `2>/dev/null`: Oculta los errores si alguna carpeta no tiene suficientes niveles.
- `sort -rh`: Ordena los resultados por tamaño en orden descendente.
- `head -n 1`: Muestra solo la carpeta más pesada.

**Ejemplo:**
Si el resultado es:
```
2G    /home/josesito/clasificado
```
Esto indica que la carpeta `/home/josesito/clasificado/` ocupa 2 GB.

---

## Ejercicio 4: Búsqueda de Archivos

### a) Encontrar el Archivo "sleep"
Encuentra el archivo llamado `sleep` en todo el sistema y verifica en qué directorios se encuentra:
```bash
find / -name "sleep" 2>/dev/null
```
**Explicación:**
- `find /`: Busca desde la raíz del sistema (`/`).
- `-name "sleep"`: Busca archivos que coincidan exactamente con el nombre `sleep`.
- `2>/dev/null`: Oculta los errores de permisos.

### b) ¿Qué Hace el Archivo `sleep`?
El comando `sleep` pausa la ejecución de un proceso durante un período especificado.

**Ejemplo:**
```bash
sleep 5
```
Este comando detiene la ejecución durante 5 segundos.

### c) ¿La Carpeta Donde se Encuentra es Especial?
El archivo `sleep` generalmente se encuentra en `/bin` o `/usr/bin`:
- **`/bin`**: Contiene comandos esenciales para el sistema operativo que son necesarios para su funcionamiento básico, incluso en modo monousuario.
- **`/usr/bin`**: Contiene comandos y aplicaciones adicionales para usuarios.

Ambas carpetas son cruciales para el funcionamiento de Linux, ya que almacenan herramientas y utilidades esenciales.

---

## Ejercicio 5: Manipulación de Archivos de Texto

### Descargar el Archivo de Configuración de Docker
Para descargar el archivo de configuración de Docker desde la terminal:
```bash
wget https://get.docker.com -O docker-config.sh
```
**Explicación:**
- `wget`: Utilidad para descargar archivos desde internet.
- `https://get.docker.com`: URL del archivo.
- `-O docker-config.sh`: Guarda el archivo descargado con el nombre `docker-config.sh`.

---

### a) Reorganización de Líneas
Mueve el texto de la línea 316 a la línea 519:
```bash
sed -n '316,519p' docker-config.sh >> docker-config.sh
sed -i '316,519d' docker-config.sh
```
**Explicación:**
- `sed -n '316,519p'`: Extrae las líneas 316 a 519.
- `>> docker-config.sh`: Añade estas líneas al final del archivo.
- `sed -i '316,519d'`: Elimina las líneas 316 a 519 del archivo original.

---

### b) Contar Cantidad de Palabras
Cuenta cuántas veces aparece la palabra "echo" en el archivo:
```bash
grep -o "\becho\b" docker-config.sh | wc -l
```
**Explicación:**
- `grep -o "\becho\b"`: Encuentra cada ocurrencia exacta de "echo".
- `wc -l`: Cuenta el número de líneas, que corresponde al número de ocurrencias.

---

### c) Reemplazo de Texto
Sustituye cada ocurrencia de "echo" con "printf":
```bash
sed -i 's/\becho\b/printf/g' docker-config.sh
```
**Explicación:**
- `sed -i`: Edita el archivo en su lugar.
- `'s/\becho\b/printf/g'`: Sustituye todas las apariciones exactas de "echo" por "printf".

---

### d) Manipulación de Segmentos de Línea
Mueve las líneas de la 140 a la 146 al final del archivo:
```bash
sed -n '140,146p' docker-config.sh >> docker-config.sh
sed -i '140,146d' docker-config.sh
```
**Explicación:**
- `sed -n '140,146p'`: Extrae las líneas 140 a 146.
- `>> docker-config.sh`: Añade estas líneas al final del archivo.
- `sed -i '140,146d'`: Elimina las líneas 140 a 146 del archivo original.

---

## Ejercicio 6: Configuración de DHCP

### Descargar el Archivo de Configuración de DHCP
Para descargar el archivo de configuración de DHCP:
```bash
wget https://drive.google.com/uc?id=163FKn6XKCGHVJQOBqTBr6ep_Lpn6iyLM -O dhcp.conf
```
**Explicación:**
- `wget`: Utilidad para descargar archivos desde internet.
- `https://drive.google.com/uc?id=163FKn6XKCGHVJQOBqTBr6ep_Lpn6iyLM`: URL directa del archivo.
- `-O dhcp.conf`: Guarda el archivo descargado con el nombre `dhcp.conf`.

---

### a) Listar IPs y Hostnames
Enumera todas las IPs y nombres de host que están actualmente en uso:
```bash
grep -oP 'host \w+|fixed-address \d+\.\d+\.\d+\.\d+' dhcp.conf | paste - -
```
**Explicación:**
- `grep -oP 'host \w+|fixed-address \d+\.\d+\.\d+\.\d+'`: Busca nombres de host e IPs.
- `paste - -`: Combina líneas consecutivas para mostrar nombre de host e IP juntos.

---

### b) Detectar IPs Disponibles
Lista las IPs disponibles dentro del rango `192.168.88.1` a `192.168.88.254`:
```bash
comm -23 <(seq 192.168.88.1 192.168.88.254 | sort) <(grep -oP 'fixed-address \K\d+\.\d+\.\d+\.\d+' dhcp.conf | sort)
```
**Explicación:**
- `seq 192.168.88.1 192.168.88.254`: Genera todas las IPs del rango.
- `grep -oP 'fixed-address \K\d+\.\d+\.\d+\.\d+'`: Extrae las IPs usadas en el archivo.
- `comm -23`: Muestra las IPs del primer conjunto que no están en el segundo.

---

### c) Buscar Entradas
Encuentra entradas basadas en IP, MAC o nombre de host:
```bash
read -p "Ingrese IP, MAC o nombre de host: " criterio
grep -i "$criterio" dhcp.conf -A 1
```
**Explicación:**
- `read -p`: Solicita al usuario que ingrese un criterio de búsqueda.
- `grep -i "$criterio" -A 1`: Busca el criterio y muestra la entrada correspondiente junto con la línea siguiente.

---

### d) Añadir Nuevas Entradas
Añade una nueva entrada verificando duplicados:
```bash
read -p "Área: " area
read -p "Hostname: " hostname
read -p "MAC: " mac
read -p "IP: " ip
grep -q "$ip" dhcp.conf || grep -q "$mac" dhcp.conf || {
  echo "host $hostname {hardware ethernet $mac; fixed-address $ip;}" >> dhcp.conf
}
```
**Explicación:**
- `grep -q`: Verifica si ya existe la IP o MAC.
- `echo`: Añade una nueva entrada si no hay duplicados.

---

### e) Eliminar Entradas
Elimina una entrada específica:
```bash
read -p "Ingrese el hostname o IP de la entrada a eliminar: " criterio
sed -i "/$criterio/,/}/d" dhcp.conf
```
**Explicación:**
- `sed -i "/$criterio/,/}/d"`: Elimina líneas desde el criterio hasta el cierre de la entrada (`}`).

---

### f) Modificar Entradas
Cambia detalles de una entrada:
```bash
read -p "Hostname/IP a modificar: " criterio
read -p "Nuevo hostname: " new_host
read -p "Nueva MAC: " new_mac
read -p "Nueva IP: " new_ip
sed -i "/$criterio/ s/host \w\+/host $new_host/" dhcp.conf
sed -i "/$criterio/ s/hardware ethernet [^;\n]\+/hardware ethernet $new_mac/" dhcp.conf
sed -i "/$criterio/ s/fixed-address [^;\n]\+/fixed-address $new_ip/" dhcp.conf
```
**Explicación:**
- `sed`: Modifica hostname, MAC e IP en la entrada especificada.

---

### g) Cambiar Área de una Entrada
Modifica el área de una entrada específica:
```bash
read -p "Hostname a modificar: " criterio
read -p "Nueva área: " new_area
sed -i "/$criterio/ s/#\w\+/#$new_area/" dhcp.conf
```
**Explicación:**
- `sed`: Cambia el comentario que indica el área de la entrada.
