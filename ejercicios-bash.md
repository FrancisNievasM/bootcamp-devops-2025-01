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
