# **Guía completa para instalar y ejecutar N8N \+ plantillas de automatización**

A continuación tienes todos los comandos necesarios para instalar, verificar, actualizar o eliminar **n8n** en tu equipo local o VPS. Esta guía complementa lo explicado en el tutorial en vídeo.

## **🔧 Instalación local**

Ejecuta en tu terminal:

**npm install n8n \-g**

*La instalación puede tardar unos 3 minutos, pero solo necesitas hacerlo una única vez.* Si te diera error prueba con:

**sudo npm install n8n \-g**

## **🚀 Iniciar n8n**

Una vez instalado, simplemente ejecuta:

**n8n**

*Se abrirá automáticamente una instancia local que podrás utilizar sin limitaciones.*

## **🌐 Acceso a n8n**

Por defecto, n8n estará disponible en:

[**http://localhost:5678**](http://localhost:5678)

## **🔍 Verificar versión instalada**

Para comprobar qué versión tienes instalada actualmente:
**n8n \--version**

## **🔄 Actualizar n8n**

Si quieres actualizar tu instalación a la última versión:

**npm update n8n \-g**

## **🗑️ Desinstalar n8n**

Para eliminar n8n por completo:

**`npm uninstall n8n -g`**

## **🚀 Ejecutar n8n 24/7 con VPS (recomendado)**

Si prefieres tener **n8n** corriendo continuamente en un servidor online, puedes hacerlo fácilmente en tres clics con Hostinger:

👉 Enlace especial con descuento: [**https://hostinger.com/alejavivps**](https://hostinger.com/alejavivps)
Añade el cupón **ALEJAVI** para un 10% de DTO adicional

Simplemente sigue las instrucciones en pantalla. Con esta opción, tendrás **n8n** funcionando constantemente y accesible desde cualquier parte del mundo.

## **🔎 Validación de requisitos en CentOS 7**

Antes de instalar n8n en tu VPS con CentOS 7, es importante verificar que el sistema cumple con los requisitos necesarios. Ejecuta estos comandos como administrador (root o con sudo):

### **Verificar puertos disponibles**

Comprueba si el puerto 5678 (usado por n8n por defecto) está disponible:

```bash
sudo netstat -tulpn | grep 5678
```

Si no aparece ningún resultado, el puerto está disponible. Si está en uso, puedes configurar n8n para usar otro puerto más adelante.

Para ver todos los puertos en uso:

```bash
sudo netstat -tulpn
```

### **Verificar recursos del sistema**

Comprueba la memoria RAM disponible:

```bash
free -h
```

Comprueba el espacio en disco:

```bash
df -h
```

Comprueba la información del CPU:

```bash
lscpu
```

n8n funciona bien con al menos 1GB de RAM y 2GB de espacio en disco.

### **Verificar versión de Node.js**

Comprueba la versión de Node.js instalada:

```bash
node -v
```

n8n tiene los siguientes requisitos de Node.js:
- **Versión mínima**: Node.js 16 (para versiones antiguas de n8n)
- **Versión recomendada**: Node.js 18 o superior (para versiones recientes de n8n)

#### **Opción 1: Instalar Node.js 16 (para versiones antiguas de n8n)**

Para instalar Node.js 16 en CentOS 7:

```bash
# Eliminar versiones anteriores de Node.js (opcional)
sudo yum remove nodejs npm -y

# Instalar el repositorio NodeSource
curl -fsSL https://rpm.nodesource.com/setup_16.x | sudo bash -

# Instalar Node.js 16
sudo yum install -y nodejs

# Verificar la instalación
node -v
npm -v
```

#### **Opción 2: Instalar Node.js 18 usando NVM con compilación compatible con CentOS 7**

Node.js 18 oficialmente no es compatible con CentOS 7 debido a requisitos de glibc, pero puedes usar una compilación no oficial compatible:

```bash
# Instalar NVM si no lo tienes
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

# Cargar NVM
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# Instalar Node.js 18 usando compilación compatible con glibc 2.17 (CentOS 7)
NVM_NODEJS_ORG_MIRROR=https://unofficial-builds.nodejs.org/download/release/ NVM_NODEJS_ORG_ARCH=x64-glibc-217 nvm install 18

# Usar Node.js 18
nvm use 18

# Establecer como predeterminado (opcional)
nvm alias default 18
```

Esta compilación no oficial permite ejecutar Node.js 18 en CentOS 7, lo que te permitirá usar versiones más recientes de n8n.

#### **Opción 3: Gestionar múltiples versiones con NVM**

Si necesitas mantener varias versiones de Node.js:

```bash
# Instalar NVM
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.3/install.sh | bash

# Cargar NVM
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"

# Instalar Node.js 16
nvm install 16

# Instalar Node.js 18 (compilación compatible)
NVM_NODEJS_ORG_MIRROR=https://unofficial-builds.nodejs.org/download/release/ NVM_NODEJS_ORG_ARCH=x64-glibc-217 nvm install 18

# Cambiar entre versiones según necesites
nvm use 16  # Para usar Node.js 16
nvm use 18  # Para usar Node.js 18
```

### **Verificar MariaDB**

Comprueba que MariaDB está instalado y funcionando:

```bash
sudo systemctl status mariadb
```

Si está activo, deberías ver "active (running)" en la salida.

Verifica la versión de MariaDB:

```bash
mysql --version
```

## **💾 Configuración de MariaDB para n8n**

n8n puede utilizar MariaDB como base de datos en lugar de SQLite. Sigue estos pasos para configurar MariaDB:

### **Crear base de datos y usuario para n8n**

Accede a MariaDB como root:

```bash
sudo mysql -u root -p
```

Crea una base de datos y un usuario para n8n (cambia 'tu_contraseña_segura' por una contraseña fuerte):

```sql
CREATE DATABASE n8n_db;
CREATE USER 'n8n_user'@'localhost' IDENTIFIED BY 'tu_contraseña_segura';
GRANT ALL PRIVILEGES ON n8n_db.* TO 'n8n_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## **⚙️ Instalación y configuración de n8n en CentOS 7**

### **Instalación de n8n**

Existen varias opciones para instalar n8n en CentOS 7, dependiendo de tus necesidades y la versión de Node.js que estés utilizando.

#### **Opción 1: Instalación estándar con npm**

Asegúrate de que estás utilizando la versión correcta de Node.js. Si has configurado nvm como se indicó anteriormente, actívalo con `nvm use 16` o `nvm use 18` según corresponda.

```bash
# Instalar n8n globalmente
npm install n8n -g
```

#### **Opción 2: Instalación con parámetros adicionales para resolver problemas comunes**

Si encuentras errores durante la instalación, prueba estas opciones:

```bash
# Ignorar advertencias de compatibilidad de motor (para Node.js 16 con versiones recientes de n8n)
npm install n8n -g --ignore-engines

# Aumentar memoria disponible para Node.js durante la instalación (para errores de memoria)
NODE_OPTIONS=--max_old_space_size=4096 npm install n8n -g

# Combinar ambas opciones
NODE_OPTIONS=--max_old_space_size=4096 npm install n8n -g --ignore-engines
```

#### **Opción 3: Instalar una versión específica compatible con Node.js 16**

Si estás usando Node.js 16 y tienes problemas con la última versión de n8n:

```bash
# Instalar una versión específica de n8n compatible con Node.js 16
npm install n8n@0.214.0 -g
```

#### **Opción 4: Instalación con dependencias para SQLite**

Si encuentras errores relacionados con SQLite durante la instalación:

```bash
# Instalar dependencias para compilar SQLite
sudo yum install -y gcc-c++ make python3 sqlite-devel

# Luego instalar n8n
npm install n8n -g
```

#### **Opción 5: Instalación sin compilar SQLite**

Si planeas usar MariaDB exclusivamente y quieres evitar problemas con SQLite:

```bash
# Instalar n8n sin compilar dependencias nativas (como SQLite)
npm install n8n -g --ignore-scripts
```

#### **Opción 6: Instalación con Docker (alternativa recomendada)**

Si tienes Docker disponible en tu sistema, esta es la opción más sencilla y evita problemas de compatibilidad. A continuación, se presentan dos opciones: una con SQLite (más sencilla) y otra con MariaDB.

##### **Opción 6.1: Instalación con Docker y SQLite**

Esta es la opción más sencilla y recomendada para la mayoría de los casos:

```bash
# Instalar Docker en CentOS 7
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker

# Verificar la instalación de Docker
docker --version

# Crear un volumen para los datos de n8n
docker volume create n8n_data

# Ejecutar n8n con Docker y SQLite
docker run -d --name n8n \
  -p 5678:5678 \
  -e GENERIC_TIMEZONE=America/Bogota \
  -e N8N_ENCRYPTION_KEY=tu_clave_de_encriptacion_segura \
  -e NODE_ENV=production \
  -e DB_TYPE=sqlite \
  -e N8N_SECURE_COOKIE=false \
  -v n8n_data:/home/node/.n8n \
  --restart always \
  docker.n8n.io/n8nio/n8n
```

> Nota: La opción `N8N_SECURE_COOKIE=false` permite acceder a n8n a través de HTTP. Para entornos de producción, es recomendable configurar HTTPS.

##### **Opción 6.2: Instalación con Docker y Docker Compose**

Para una configuración más avanzada, puedes usar Docker Compose:

```bash
# Instalar Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose

# Verificar la instalación
docker-compose --version

# Crear un directorio para n8n
mkdir -p ~/n8n
cd ~/n8n

# Crear el archivo docker-compose.yml
cat > docker-compose.yml << EOF
version: '3'
services:
  n8n:
    image: docker.n8n.io/n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - PORT=5678
      - GENERIC_TIMEZONE=America/Bogota
      - N8N_ENCRYPTION_KEY=tu_clave_de_encriptacion_segura
      - NODE_ENV=production
      - DB_TYPE=sqlite
      - DB_SQLITE_VACUUM_ON_STARTUP=true
      - N8N_SECURE_COOKIE=false
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  n8n_data:
    external: false
EOF

# Iniciar n8n con Docker Compose
docker-compose up -d
```

##### **Opción 6.3: Instalación con Docker y MariaDB**

Si prefieres usar MariaDB como base de datos:

```bash
# Ejecutar n8n con Docker conectado a MariaDB
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  -e DB_TYPE=mysqldb \
  -e DB_MYSQLDB_DATABASE=n8n_db \
  -e DB_MYSQLDB_HOST=host.docker.internal \
  -e DB_MYSQLDB_PORT=3306 \
  -e DB_MYSQLDB_USER=n8n_user \
  -e DB_MYSQLDB_PASSWORD=tu_contraseña_segura \
  -e N8N_SECURE_COOKIE=false \
  docker.n8n.io/n8nio/n8n
```

> Nota: Si usas Docker, es posible que necesites ajustar la configuración de red para que n8n pueda conectarse a tu base de datos MariaDB. En algunos casos, deberás usar la IP de tu host en lugar de `host.docker.internal`. En sistemas Linux, puedes usar `--network="host"` o `--add-host=host.docker.internal:host-gateway` para permitir que el contenedor acceda a servicios en el host.

### **Configuración de variables de entorno para MariaDB**

Crea un archivo de configuración para n8n:

```bash
sudo mkdir -p /etc/n8n
sudo nano /etc/n8n/env
```

Añade las siguientes líneas (reemplaza 'tu_contraseña_segura' con la contraseña que configuraste anteriormente):

```
DB_TYPE=mysqldb
DB_MYSQLDB_DATABASE=n8n_db
DB_MYSQLDB_HOST=localhost
DB_MYSQLDB_PORT=3306
DB_MYSQLDB_USER=n8n_user
DB_MYSQLDB_PASSWORD=tu_contraseña_segura

# Puerto en el que se ejecutará n8n (opcional, por defecto es 5678)
PORT=5678

# Configuración de zona horaria (opcional)
GENERIC_TIMEZONE=America/Bogota

# Configuración de seguridad (opcional pero recomendado para producción)
N8N_ENCRYPTION_KEY=una_clave_segura_de_al_menos_32_caracteres
```

> Nota: Asegúrate de cambiar `GENERIC_TIMEZONE` a tu zona horaria y generar una clave de encriptación segura para `N8N_ENCRYPTION_KEY`.

### **Configuración de servicio systemd**

Hay dos opciones para configurar el servicio systemd para n8n: ejecutarlo como un usuario específico (recomendado para producción) o como root (más simple para pruebas).

#### **Opción 1: Ejecutar n8n como un usuario específico (recomendado para producción)**

```bash
# Crear un usuario específico para n8n
sudo useradd -m -s /bin/bash n8n_user

# Crear directorio de trabajo
sudo mkdir -p /home/n8n_user/n8n
sudo chown -R n8n_user:n8n_user /home/n8n_user/n8n
```

Crea un archivo de servicio systemd para n8n:

```bash
sudo nano /etc/systemd/system/n8n.service
```

Añade el siguiente contenido:

```
[Unit]
Description=n8n workflow automation
After=network.target mariadb.service
Wants=mariadb.service

[Service]
Type=simple
User=n8n_user
Group=n8n_user
WorkingDirectory=/home/n8n_user/n8n
EnvironmentFile=/etc/n8n/env
ExecStart=/usr/bin/n8n start
Restart=always
RestartSec=10
KillMode=process
SuccessExitStatus=0

# Hardening
ProtectSystem=full
PrivateTmp=true
NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

#### **Opción 2: Ejecutar n8n como root (más simple para pruebas)**

Si n8n está instalado bajo el usuario root (por ejemplo, usando nvm), puede ser más sencillo ejecutarlo como root:

```bash
sudo nano /etc/systemd/system/n8n.service
```

Añade el siguiente contenido:

```
[Unit]
Description=n8n workflow automation
After=network.target mariadb.service
Wants=mariadb.service

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/root
EnvironmentFile=/etc/n8n/env
ExecStart=/usr/bin/n8n start
Restart=always
RestartSec=10
KillMode=process
SuccessExitStatus=0

[Install]
WantedBy=multi-user.target
```

#### **Opción 3: Ejecutar n8n con nvm (si usas Node Version Manager)**

Si has instalado Node.js usando nvm, necesitas cargar nvm antes de ejecutar n8n:

```bash
sudo nano /etc/systemd/system/n8n.service
```

Añade el siguiente contenido:

```
[Unit]
Description=n8n workflow automation
After=network.target mariadb.service
Wants=mariadb.service

[Service]
Type=simple
User=root
Group=root
WorkingDirectory=/root
EnvironmentFile=/etc/n8n/env
ExecStart=/bin/bash -c 'export NVM_DIR="/root/.nvm" && [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" && nvm use 16 && /usr/bin/n8n start'
Restart=always
RestartSec=10
KillMode=process
SuccessExitStatus=0

[Install]
WantedBy=multi-user.target
```

> **Importante**: Asegúrate de que el ejecutable de n8n esté correctamente enlazado en `/usr/bin/n8n`. Si no es así, deberás crear un enlace simbólico:
> ```bash
> # Buscar la ubicación real del ejecutable de n8n
> find / -name n8n -type f -executable 2>/dev/null
>
> # Crear un enlace simbólico (ajusta la ruta según tu instalación)
> ln -sf /ruta/al/ejecutable/n8n /usr/bin/n8n
>
> # Verificar que el enlace funcione
> /usr/bin/n8n --version
> ```

### **Configuración de firewall**

Si tienes firewalld activo, permite el puerto 5678:

```bash
sudo firewall-cmd --permanent --add-port=5678/tcp
sudo firewall-cmd --reload
```

## **🚀 Puesta en marcha y verificación**

### **Iniciar el servicio n8n**

Habilita e inicia el servicio n8n:

```bash
sudo systemctl daemon-reload
sudo systemctl enable n8n
sudo systemctl start n8n
```

### **Verificar el estado del servicio**

Comprueba que n8n está funcionando correctamente:

```bash
sudo systemctl status n8n
```

### **Acceder a n8n**

Ahora puedes acceder a n8n desde tu navegador usando la IP de tu VPS:

```
http://tu_ip_del_servidor:5678
```

### **Configuración de un proxy inverso (opcional)**

Si ya tienes un servidor web como Apache o Nginx ejecutándose en tu VPS y quieres acceder a n8n a través de un subdominio, puedes configurar un proxy inverso.

#### Para Nginx:

```bash
sudo nano /etc/nginx/conf.d/n8n.conf
```

Añade la siguiente configuración:

```
server {
    listen 80;
    server_name n8n.tudominio.com;

    location / {
        proxy_pass http://localhost:5678;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
```

Reinicia Nginx:

```bash
sudo nginx -t
sudo systemctl restart nginx
```

#### Para Apache:

```bash
sudo nano /etc/httpd/conf.d/n8n.conf
```

Añade la siguiente configuración:

```
<VirtualHost *:80>
    ServerName n8n.tudominio.com

    ProxyPreserveHost On
    ProxyPass / http://localhost:5678/
    ProxyPassReverse / http://localhost:5678/

    RewriteEngine on
    RewriteCond %{HTTP:Upgrade} websocket [NC]
    RewriteCond %{HTTP:Connection} upgrade [NC]
    RewriteRule ^/?(.*) "ws://localhost:5678/$1" [P,L]
</VirtualHost>
```

Reinicia Apache:

```bash
sudo apachectl configtest
sudo systemctl restart httpd
```

### **Solución de problemas comunes**

Si encuentras problemas, revisa los logs del servicio:

```bash
sudo journalctl -u n8n -f
```

Si necesitas cambiar la configuración, edita el archivo de entorno y reinicia el servicio:

```bash
sudo nano /etc/n8n/env
sudo systemctl restart n8n
```

#### Problemas comunes y soluciones:

1. **Error de conexión a la base de datos**:
   - Verifica que MariaDB esté en ejecución: `sudo systemctl status mariadb`
   - Comprueba las credenciales en el archivo de entorno
   - Asegúrate de que el usuario tenga los permisos correctos: `GRANT ALL PRIVILEGES ON n8n_db.* TO 'n8n_user'@'localhost';`

2. **Error de SQLite durante la instalación**:
   - Instala las dependencias necesarias: `sudo yum install -y gcc-c++ make python3 sqlite-devel`
   - Reinstala n8n: `npm install n8n -g`
   - Alternativa: instala sin compilar SQLite: `npm install n8n -g --ignore-scripts`

3. **Problemas con Node.js**:
   - Asegúrate de estar usando Node.js 16 o superior: `node -v`
   - Si tienes nvm instalado, activa la versión correcta: `nvm use 16` o `nvm use 18`
   - Para Node.js 18 en CentOS 7, usa la compilación no oficial: `NVM_NODEJS_ORG_MIRROR=https://unofficial-builds.nodejs.org/download/release/ NVM_NODEJS_ORG_ARCH=x64-glibc-217 nvm install 18`

4. **n8n no inicia**:
   - Verifica los permisos del directorio de trabajo: `sudo chown -R n8n_user:n8n_user /home/n8n_user/n8n`
   - Comprueba que el archivo de entorno existe y tiene los permisos correctos: `sudo chmod 644 /etc/n8n/env`
   - Verifica que el ejecutable de n8n esté correctamente enlazado: `ls -la /usr/bin/n8n`
   - Si el enlace está "colgando", crea uno nuevo: `ln -sf /ruta/al/ejecutable/n8n /usr/bin/n8n`

5. **Error de memoria durante la instalación**:
   - Aumenta la memoria disponible para Node.js: `NODE_OPTIONS=--max_old_space_size=4096 npm install n8n -g`
   - Libera memoria en el sistema antes de instalar: `sudo sync && sudo echo 3 > /proc/sys/vm/drop_caches`

6. **Advertencias de motor no compatible (EBADENGINE)**:
   - Ignora las advertencias si usas Node.js 16: `npm install n8n -g --ignore-engines`
   - Instala una versión específica compatible: `npm install n8n@0.214.0 -g`
   - Actualiza a Node.js 18 usando la compilación no oficial para CentOS 7

7. **Conflictos con otros servicios**:
   - Cambia el puerto de n8n en el archivo de entorno: `PORT=8080`
   - Configura un proxy inverso con Nginx o Apache para acceder a n8n a través de un subdominio

8. **No se puede acceder a n8n desde el navegador**:
   - Verifica que n8n esté escuchando en todas las interfaces: `netstat -tulpn | grep 5678`
   - Comprueba que puedes acceder localmente: `curl http://localhost:5678/`
   - Verifica si hay algún firewall activo: `systemctl status firewalld`
   - Si el firewall está activo, abre el puerto: `firewall-cmd --permanent --add-port=5678/tcp && firewall-cmd --reload`

#### **Verificar si un puerto está en uso**

Antes de cambiar el puerto de n8n, es recomendable verificar si el nuevo puerto está disponible:

```bash
# Instalar herramientas de red si no están disponibles
yum install -y net-tools

# Verificar si el puerto 8080 está en uso
netstat -tulpn | grep 8080
```

Si no hay salida, el puerto está libre. Si ves una salida como esta, el puerto está en uso:
```
tcp        0      0 0.0.0.0:8080            0.0.0.0:*               LISTEN      12345/proceso
```

Otros comandos útiles para verificar puertos:

```bash
# Usando ss (sustituto moderno de netstat)
ss -tulpn | grep 8080

# Usando lsof (necesita instalarse: yum install -y lsof)
lsof -i :8080

# Ver todos los puertos en uso
netstat -tulpn

# Identificar qué proceso está usando un puerto
fuser 8080/tcp
```

Si el puerto está libre, puedes modificar la configuración de n8n:

```bash
# Editar el archivo de configuración
nano /etc/n8n/env
```

Cambia la línea `PORT=5678` a `PORT=8080` y reinicia el servicio:

```bash
systemctl restart n8n
```

Después, n8n estará accesible a través de:
```
http://tu_ip_del_servidor:8080/
```

## **✅ Comandos útiles (resumen)**

### Comandos de Node.js
* Verificar versión de Node.js: **`node -v`**
* Verificar versión de npm: **`npm -v`**
* Activar Node.js 16 con nvm: **`nvm use 16`**
* Activar Node.js 18 con nvm: **`nvm use 18`**
* Instalar Node.js 18 compatible con CentOS 7: **`NVM_NODEJS_ORG_MIRROR=https://unofficial-builds.nodejs.org/download/release/ NVM_NODEJS_ORG_ARCH=x64-glibc-217 nvm install 18`**

### Comandos de n8n
* Instalar n8n: **`npm install n8n -g`**
* Instalar n8n ignorando advertencias de motor: **`npm install n8n -g --ignore-engines`**
* Instalar n8n con más memoria: **`NODE_OPTIONS=--max_old_space_size=4096 npm install n8n -g`**
* Instalar n8n sin compilar SQLite: **`npm install n8n -g --ignore-scripts`**
* Instalar versión específica de n8n: **`npm install n8n@0.214.0 -g`**
* Iniciar n8n manualmente: **`n8n`**
* Comprobar versión de n8n: **`n8n --version`**
* Actualizar n8n: **`npm update n8n -g`**
* Desinstalar n8n: **`npm uninstall n8n -g`**

### Comandos de Docker para n8n
* Instalar Docker en CentOS 7: **`sudo yum install -y docker-ce docker-ce-cli containerd.io`**
* Iniciar y habilitar Docker: **`sudo systemctl start docker && sudo systemctl enable docker`**
* Crear volumen para datos: **`docker volume create n8n_data`**
* Ejecutar n8n con Docker y SQLite: **`docker run -d --name n8n -p 5678:5678 -e DB_TYPE=sqlite -e N8N_SECURE_COOKIE=false -v n8n_data:/home/node/.n8n --restart always docker.n8n.io/n8nio/n8n`**
* Ejecutar n8n con Docker y MariaDB: **`docker run -d --name n8n -p 5678:5678 -v n8n_data:/home/node/.n8n -e DB_TYPE=mysqldb -e DB_MYSQLDB_DATABASE=n8n_db -e DB_MYSQLDB_HOST=host.docker.internal -e DB_MYSQLDB_PORT=3306 -e DB_MYSQLDB_USER=n8n_user -e DB_MYSQLDB_PASSWORD=tu_contraseña_segura -e N8N_SECURE_COOKIE=false --restart always docker.n8n.io/n8nio/n8n`**
* Ver logs de n8n en Docker: **`docker logs -f n8n`**
* Detener contenedor de n8n: **`docker stop n8n`**
* Iniciar contenedor de n8n: **`docker start n8n`**
* Reiniciar contenedor de n8n: **`docker restart n8n`**
* Eliminar contenedor de n8n: **`docker rm n8n`**
* Actualizar imagen de n8n: **`docker pull docker.n8n.io/n8nio/n8n`**
* Ver contenedores en ejecución: **`docker ps`**
* Ver todos los contenedores: **`docker ps -a`**
* Ver volúmenes de Docker: **`docker volume ls`**
* Ejecutar comandos dentro del contenedor: **`docker exec -it n8n /bin/sh`**

### Comandos de Docker Compose para n8n
* Instalar Docker Compose: **`sudo curl -L "https://github.com/docker/compose/releases/download/v2.20.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose && sudo chmod +x /usr/local/bin/docker-compose`**
* Verificar instalación: **`docker-compose --version`**
* Iniciar servicios: **`docker-compose up -d`**
* Ver logs: **`docker-compose logs -f`**
* Detener servicios: **`docker-compose down`**
* Reiniciar servicios: **`docker-compose restart`**
* Actualizar imágenes: **`docker-compose pull`**
* Ver servicios en ejecución: **`docker-compose ps`**
* Ejecutar comandos en un servicio: **`docker-compose exec n8n /bin/sh`**

### Comandos de systemd
* Recargar configuración de systemd: **`sudo systemctl daemon-reload`**
* Habilitar servicio n8n: **`sudo systemctl enable n8n`**
* Iniciar servicio n8n: **`sudo systemctl start n8n`**
* Ver estado del servicio: **`sudo systemctl status n8n`**
* Reiniciar servicio: **`sudo systemctl restart n8n`**
* Detener servicio: **`sudo systemctl stop n8n`**
* Ver logs: **`sudo journalctl -u n8n -f`**

### Comandos de MariaDB
* Acceder a MariaDB: **`mysql -u root -p`**
* Verificar estado de MariaDB: **`sudo systemctl status mariadb`**
* Reiniciar MariaDB: **`sudo systemctl restart mariadb`**
* Crear base de datos: **`CREATE DATABASE n8n_db;`**
* Crear usuario: **`CREATE USER 'n8n_user'@'localhost' IDENTIFIED BY 'tu_contraseña_segura';`**
* Otorgar permisos: **`GRANT ALL PRIVILEGES ON n8n_db.* TO 'n8n_user'@'localhost';`**

### Comandos de firewall
* Añadir puerto al firewall: **`sudo firewall-cmd --permanent --add-port=5678/tcp`**
* Recargar configuración del firewall: **`sudo firewall-cmd --reload`**
* Ver puertos abiertos: **`sudo firewall-cmd --list-ports`**

### Comandos de gestión de memoria
* Liberar caché de memoria: **`sudo sync && sudo echo 3 > /proc/sys/vm/drop_caches`**
* Ver uso de memoria: **`free -h`**
* Ver procesos por uso de memoria: **`ps aux --sort=-%mem | head -10`**

## **🐳 Ventajas de usar contenedores Docker para n8n**

La instalación de n8n mediante contenedores Docker ofrece numerosas ventajas, especialmente en sistemas como CentOS 7:

### **Aislamiento y consistencia**
- **Entorno aislado**: Los contenedores proporcionan un entorno aislado que no interfiere con el sistema host.
- **Consistencia**: El mismo contenedor funcionará de manera idéntica en cualquier entorno que soporte Docker.
- **Reproducibilidad**: Facilita la recreación exacta del entorno de ejecución.

### **Gestión de dependencias**
- **Resolución de problemas de compatibilidad**: Evita conflictos con las dependencias del sistema.
- **Independencia de glibc**: Supera las limitaciones de glibc 2.17 en CentOS 7, que puede causar problemas con Node.js 18+.
- **Paquetes preinstalados**: La imagen de Docker de n8n ya incluye todas las dependencias necesarias.

### **Facilidad de instalación y actualización**
- **Instalación simplificada**: Un solo comando para instalar n8n con todas sus dependencias.
- **Actualizaciones sencillas**: Actualizar n8n es tan simple como descargar la nueva imagen y reiniciar el contenedor.
- **Rollback sencillo**: Fácil retorno a versiones anteriores si hay problemas.

### **Gestión de recursos**
- **Control de recursos**: Posibilidad de limitar CPU y memoria asignados a n8n.
- **Escalabilidad**: Facilita la implementación de múltiples instancias para escalar horizontalmente.

### **Seguridad**
- **Menor superficie de ataque**: El contenedor limita el acceso al sistema host.
- **Aislamiento de procesos**: Los procesos dentro del contenedor están aislados del sistema host.
- **Ejecución sin privilegios**: Posibilidad de ejecutar como usuario no root.

### **Persistencia de datos**
- **Volúmenes de datos**: Permite mantener los datos persistentes entre reinicios del contenedor.
- **Respaldos simplificados**: Facilita la creación de copias de seguridad de los datos.

## **🔒 Solución de problemas con cookies seguras**

Si al acceder a n8n ves un mensaje de error sobre cookies seguras, tienes varias opciones:

### **Opción 1: Desactivar las cookies seguras (solución rápida)**

Esta es la solución más rápida, aunque menos segura:

```bash
# Detener el contenedor actual si está en ejecución
docker stop n8n
docker rm n8n

# Ejecutar n8n con cookies seguras desactivadas
docker run -d --name n8n \
  -p 5678:5678 \
  -e GENERIC_TIMEZONE=America/Bogota \
  -e N8N_ENCRYPTION_KEY=tu_clave_de_encriptacion_segura \
  -e NODE_ENV=production \
  -e DB_TYPE=sqlite \
  -e N8N_SECURE_COOKIE=false \
  -v n8n_data:/home/node/.n8n \
  --restart always \
  docker.n8n.io/n8nio/n8n
```

Si estás usando Docker Compose, añade `N8N_SECURE_COOKIE=false` a las variables de entorno en tu archivo `docker-compose.yml` y reinicia los contenedores con `docker-compose up -d`.

### **Opción 2: Configurar HTTPS con un proxy inverso (recomendado para producción)**

Para entornos de producción, es recomendable configurar HTTPS. Puedes usar Nginx, Apache o Caddy como proxy inverso.

#### **Ejemplo con Caddy (muy sencillo)**

```bash
# Instalar Caddy
sudo yum install -y yum-plugin-copr
sudo yum copr enable @caddy/caddy
sudo yum install -y caddy

# Configurar Caddy
sudo tee /etc/caddy/Caddyfile > /dev/null << EOF
n8n.tudominio.com {
    reverse_proxy localhost:5678
}
EOF

# Iniciar Caddy
sudo systemctl enable caddy
sudo systemctl start caddy
```

#### **Ejemplo con Nginx**

Consulta la sección "Configuración de un proxy inverso" más arriba en esta guía.

### **Opción 3: Acceder a través de localhost**

Si estás accediendo a n8n desde el mismo servidor, puedes usar `localhost` en lugar de la dirección IP:

```
http://localhost:5678
```

## **🔗 Enlaces útiles**

* [Documentación oficial de n8n](https://docs.n8n.io/)
* [Plantilla con más de 1.600 Automatizaciones](https://n8n.io/workflows/)
* [Foro de la comunidad n8n](https://community.n8n.io/)
* [Repositorio de GitHub de n8n](https://github.com/n8n-io/n8n)
* [Documentación de Docker para n8n](https://docs.n8n.io/hosting/installation/docker/)
* [Guía de solución de problemas de n8n](https://docs.n8n.io/hosting/troubleshooting/)
* [Compilaciones no oficiales de Node.js para sistemas antiguos](https://unofficial-builds.nodejs.org/download/release/)

[**🏫 Reserva tu plaza en mi academia (cerramos este mes)**](https://academiartificial.com/)
