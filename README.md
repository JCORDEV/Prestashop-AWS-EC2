# Prestashop AWS EC2

Guía **paso a paso** para crear una instancia de Amazon EC2, instalar, configurar y personalizar una tienda virtual.

Creación de una instancia EC2 en Amazon AWS



1. Accede a la Consola de AWS:
- Inicia sesión en tu cuenta de Amazon Web Services (AWS).
- Ve al servicio EC2 mediante el buscador
![Paso #1](img/paso1v1.jpeg)
<br>o tambien desde el panel servicios > informatica > EC2
![Paso #1](img/paso1v2.jpeg)

2. Lanzamiento de la instancia:
- Haz clic en **Lanzar la instancia**.
![Paso #1](img/paso2.jpeg)
- Asigna un nombre a la instancia.
![Paso #1](img/paso3.jpeg)
- Puedes agregar etiquetas adicionales para identificar la instancia si asi lo deseas.
![Paso #1](img/paso4.jpeg)
- Selecciona la imagen ubuntu como sistema operativo.
![Paso #1](img/paso5.jpeg)
- Elige el tipo de instancia (por ejemplo, t2.micro para una instancia de prueba gratuita).
![Paso #1](img/paso6.jpeg)
- Creamos un par de claves para acceder a la instancia de forma segura
![Paso #1](img/paso7.jpeg)
<br>o por el contrario asignamos un par de claves ya existen.<br>
![Paso #1](img/paso8.jpeg)
- Configuración de red.<br>
![Paso #1](img/paso9.jpeg)
- Configura el almacenamiento (puedes usar el valor predeterminado o personalizarlo).
![Paso #1](img/paso10.jpeg)
- Verifica la configuración antes de lanzar la instancia.
- Detalles de la instancia ec2 creada.
![Paso #1](img/paso11.jpeg)

3. Asignar y asociar ip elastica:
- click en asignar ip elastica
![Paso #1](img/paso12.jpeg)
- luego en asignar
![Paso #1](img/paso13.jpeg)
- seleccionamos la ip elestica creada, desplegamos las opciones de **acciones** y damos click en asociar.
![Paso #1](img/paso14.jpeg)
- seleccionamos la instancia la cual sera asociada esta ip elastica y marcamos la casilla **Permitir que se vuelva a asociar esta dirección IP elastica**
![Paso #1](img/paso15.jpeg)
esto nos evitara que la ip publica cambie con el pasar del tiempo.

4. Conectar la instancia por medio del protocolo **ssh**:
    
- Volvemos al apartado de instancias, teniendo seleccionada la instancia damos clic en **conectar**
![Paso #1](img/paso16.jpeg)

- luego nos digimos a la opcion **cliente ssh** y utiliza el par de claves para conectarte a la instancia mediante SSH.
![Paso #1](img/paso17.jpeg)

- Entramos a cmd desde la carpeta donde guardo el par de claves.
![Paso #1](img/paso18.png)
    
- pegamos el ssh y ya nos encontrariamos dentro de nuestra instancia.
![Paso #1](img/paso19.png)

## Instalación y configuración de la tienda virtual

- Primero actualizamos los repositorios
➡️ ```sudo apt-get update```
![Paso #1](img/paso20.png)

- Luego instalamos **MySQL, Apache y PHP**
➡️ ```sudo apt install -y apache2 libapache2-mod-php php-mysql mariadb-server```
![Paso #1](img/paso21.png)

- Revisamos que **MySQL y Apache** esten en ejecución
➡️ ```sudo systemctl status apache2 mariadb```
![Paso #1](img/paso22.png)

- Iniciamos sesión en **MySQL** ➡️ ```sudo mysql -u root -p```
![Paso #1](img/paso23.png)

- Creamos nuestra base de datos para **Prestashop** ➡️ ```create database burgerland charset utf8mb4 collate utf8mb4_unicode_ci;```
![Paso #1](img/paso24.png)
**Nota:** renombrar **burgerland** por el nombre que desees poner a tu base de datos.

- Creamos el usuario ➡️ ```create user user_burgerland@localhost identified by '123';```
![Paso #1](img/paso25.png)
**Nota:** renombrar **user_burgerland** por el nombre de usuario que desees poner.

- Brindamos los permisos necesarios al usuario previamente ➡️ ```grant all privileges on burgerland.* to user_burgerland@localhost;```
![Paso #1](img/paso26.png)
**Nota:** renombrar **burgerland** y **user_burgerland** por la base de datos y el nombre de usuario que previamente creamos.

- Recargar los privilegios del sistema ➡️ ```flush privileges;```
![Paso #1](img/paso27.png)

- Ingresamos a la ruta ➡️ ```cd /var/www/html/```, luego descargamos **Prestashop v8.1.3** ➡️ ```sudo wget https://github.com/PrestaShop/PrestaShop/releases/download/8.1.3/prestashop_8.1.3.zip```
![Paso #1](img/paso28.png)
**Nota:** Si deseas descargar otra versión diferente dar click **[aqui](https://github.com/PrestaShop/PrestaShop/releases)** 

- Instalamos el módulo **zip** ➡️ ```sudo apt install zip unzip```
![Paso #1](img/paso29.png)

- Descomprimir el archivo **.zip** descargado ➡️ ```sudo unzip prestashop_8.1.3.zip```
![Paso #1](img/paso30.png)

- Crear una carpeta para el nombre de la tienda ➡️ ```sudo mkdir burgerland```, luego movemos el **.zip** que se nos genero a la carpeta previamente creada ➡️ ```sudo mv prestashop.zip burgerland``` y por ultimo ingresamos a la carpeta ➡️ ```cd burgerland```
![Paso #1](img/paso31.png)

- Descomprimir **prestashop.zip** ➡️ ```sudo unzip prestashop.zip```
![Paso #1](img/paso32.png)

- Nos direccionamos hacia ➡️ ```cd /var/www/``` y damos permiso a la carpeta **html** ➡️ ```sudo chown -R www-data:www-data html```
![Paso #1](img/paso33-dar%20permisos.png)

- Instalamos las dependencias que necesita **Prestashop** ➡️ ```sudo apt install -y php-{cli,common,curl,zip,gd,mysql,xml,mbstring,json,intl}```
![Paso #1](img/paso34.png)

- Reiniciamos el servicio **apache2** ➡️ ```sudo service apache2 restart```
![Paso #1](img/paso35-reiniciamos%20los%20servicios%20despues%20de%20instalar%20todo.png)