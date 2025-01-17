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
