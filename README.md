# Prestashop AWS EC2
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![MariaDB](https://img.shields.io/badge/MariaDB-003545?style=for-the-badge&logo=mariadb&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)
![PHP](https://img.shields.io/badge/php-%23777BB4.svg?style=for-the-badge&logo=php&logoColor=white)
![Apache](https://img.shields.io/badge/apache-%23D42029.svg?style=for-the-badge&logo=apache&logoColor=white)


Guía **paso a paso** para crear una instancia de Amazon EC2, instalar, configurar y personalizar una tienda virtual.

### Creación de una instancia EC2 en Amazon AWS

1. Accede a la Consola de AWS:
- Inicia sesión en tu cuenta de Amazon Web Services (AWS).
- Ve al servicio EC2 mediante el buscador
![Paso #1](img/paso1v1.jpeg)
<br>o tambien desde el panel servicios > informatica > EC2
![Paso #1](img/paso1v2.jpeg)

2. Lanzamiento de la instancia:
- Haz clic en **Lanzar la instancia**.
![Paso #2](img/paso2.jpeg)
- Asigna un nombre a la instancia.
![Paso #3](img/paso3.jpeg)
- Puedes agregar etiquetas adicionales para identificar la instancia si asi lo deseas.
![Paso #4](img/paso4.jpeg)
- Selecciona la imagen ubuntu como sistema operativo.
![Paso #5](img/paso5.jpeg)
- Elige el tipo de instancia (por ejemplo, t2.micro para una instancia de prueba gratuita).
![Paso #6](img/paso6.jpeg)
- Creamos un par de claves para acceder a la instancia de forma segura
![Paso #7](img/paso7.jpeg)
<br>o por el contrario asignamos un par de claves ya existen.<br>
![Paso #8](img/paso8.jpeg)
- Configuración de red.<br>
![Paso #9](img/paso9.jpeg)
- Configura el almacenamiento (puedes usar el valor predeterminado o personalizarlo).
![Paso #10](img/paso10.jpeg)
- Verifica la configuración antes de lanzar la instancia.
- Detalles de la instancia ec2 creada.
![Paso #11](img/paso11.jpeg)

3. Asignar y asociar ip elastica:
- click en asignar ip elastica
![Paso #12](img/paso12.jpeg)
- luego en asignar
![Paso #13](img/paso13.jpeg)
- seleccionamos la ip elestica creada, desplegamos las opciones de **acciones** y damos click en asociar.
![Paso #14](img/paso14.jpeg)
- seleccionamos la instancia la cual sera asociada esta ip elastica y marcamos la casilla **Permitir que se vuelva a asociar esta dirección IP elastica**
![Paso #15](img/paso15.jpeg)
esto nos evitara que la ip publica cambie con el pasar del tiempo.

4. Conectar la instancia por medio del protocolo **ssh**:
    
- Volvemos al apartado de instancias, teniendo seleccionada la instancia damos clic en **conectar**
![Paso #16](img/paso16.jpeg)

- luego nos digimos a la opcion **cliente ssh** y utiliza el par de claves para conectarte a la instancia mediante SSH.
![Paso #17](img/paso17.jpeg)

- Entramos a cmd desde la carpeta donde guardo el par de claves.
![Paso #18](img/paso18.png)
    
- pegamos el ssh y ya nos encontrariamos dentro de nuestra instancia.
![Paso #19](img/paso19.png)

### Instalación y configuración de la tienda virtual

- Primero actualizamos los repositorios
➡️ ```sudo apt-get update```
![Paso #20](img/paso20.png)

- Luego instalamos **MySQL, Apache y PHP**
➡️ ```sudo apt install -y apache2 libapache2-mod-php php-mysql mariadb-server```
![Paso #21](img/paso21.png)

- Revisamos que **MySQL y Apache** esten en ejecución
➡️ ```sudo systemctl status apache2 mariadb```
![Paso #22](img/paso22.png)

- Iniciamos sesión en **MySQL** ➡️ ```sudo mysql -u root -p```
![Paso #23](img/paso23.png)

- <a id="bd"></a>Creamos nuestra base de datos para **Prestashop** ➡️ ```create database burgerland charset utf8mb4 collate utf8mb4_unicode_ci;```
![Paso #24](img/paso24.png)
**Nota:** renombrar **burgerland** por el nombre que desees poner a tu base de datos.

- <a id="user"></a>Creamos el usuario ➡️ ```create user user_burgerland@localhost identified by '123';```
![Paso #25](img/paso25.png)
**Nota:** renombrar **user_burgerland** por el nombre de usuario que desees poner.

- Brindamos los permisos necesarios al usuario previamente creado ➡️ ```grant all privileges on burgerland.* to user_burgerland@localhost;```
![Paso #26](img/paso26.png)
**Nota:** renombrar **burgerland** y **user_burgerland** por la base de datos y el nombre de usuario que previamente creamos.

- Recargar los privilegios del sistema ➡️ ```flush privileges;```
![Paso #27](img/paso27.png)

- Ingresamos a la ruta ➡️ ```cd /var/www/html/```, luego descargamos **Prestashop v8.1.3** ➡️ ```sudo wget https://github.com/PrestaShop/PrestaShop/releases/download/8.1.3/prestashop_8.1.3.zip```
![Paso #28](img/paso28.png)
**Nota:** Si deseas descargar otra versión diferente dar click **[aquí](https://github.com/PrestaShop/PrestaShop/releases)**, pero tener en cuenta que **dependencias** necesita dicha versión

- Instalamos el módulo **zip** ➡️ ```sudo apt install zip unzip```
![Paso #29](img/paso29.png)

- Descomprimir el archivo **.zip** descargado ➡️ ```sudo unzip prestashop_8.1.3.zip```
![Paso #30](img/paso30.png)

- <a id="carpeta"></a>Crear una carpeta para el nombre de la tienda ➡️ ```sudo mkdir burgerland```, luego movemos el **.zip** que se nos genero a la carpeta previamente creada ➡️ ```sudo mv prestashop.zip burgerland``` y por ultimo ingresamos a la carpeta ➡️ ```cd burgerland```
<a name="carpeta">![Paso #31](img/paso31.png)</a>

- Descomprimir **prestashop.zip** ➡️ ```sudo unzip prestashop.zip```
![Paso #32](img/paso32.png)

- Nos vamos hacia ➡️ ```cd /var/www/``` y damos permiso a la carpeta **html** ➡️ ```sudo chown -R www-data:www-data html```
![Paso #33](img/paso33-dar%20permisos.png)

- Instalamos las dependencias que necesita **Prestashop** ➡️ ```sudo apt install -y php-{cli,common,curl,zip,gd,mysql,xml,mbstring,json,intl}```
![Paso #34](img/paso34.png)

- Reiniciamos el servicio **apache2** ➡️ ```sudo service apache2 restart```
![Paso #35](img/paso35-reiniciamos%20los%20servicios%20despues%20de%20instalar%20todo.png)

- <a id="web"></a>Copiamos la **IP elástica** ➡️ ```44.193.189.248/burgerland```
![Paso #36](img/paso36.png)
**Nota:** **44.193.189.248** ➡️ es la **IP elástica** de su instancia donde instalo prestashop, **burgerland** ➡️ es el nombre de la carpeta creada en este **[paso](#carpeta)**

- Seleccionamos el **idioma** de preferencia
![Paso #37](img/paso37.png)

- Aceptamos **términos y condiciones**
![Paso #38](img/paso38.png)

- Nos saldrá este error porque aun no tenemos habilitado el módulo de **a2enmod rewrite**
![Paso #39](img/paso39.png)
para poder habilitar el modulo de **a2enmod rewrite** ejecutamos el siguiente comando ➡️ ```sudo a2enmod rewrite```
![Paso #40](img/paso40-activar%20modulo,%20reiniciar%20servicio.png)
y damos clic en **Actualizar información**
![Paso #41](img/paso41.png)

- Registramos **la información sobre tu tienda**
![Paso #42](img/paso42.png)

- Dejamos esta parte **por defecto**
![Paso #43](img/paso43.png)

- Registramos el nombre de la **[base de datos](#bd)**, el nombre de **[usuario y contraseña](#user)** y por ultimo **comprobamos la conexion con la base de datos**
![Paso #44](img/paso44.png)
![Paso #45](img/paso45.png)
- **Prestashop** nos recomienda eliminar la carpeta **install** por razones de seguridad
![Paso #46](img/paso46.png)
entramos a la carpeta de nuestra tienda ➡️ ```cd html/burgerland``` y ejecutamos el siguiente comando para borrarla ➡️ ```sudo rm -r install```
![Paso #47](img/paso47.png)

- <a id="por_defecto"></a>Para entrar a la página ➡️ ```44.193.189.248/burgerland``` teniendo en cuenta estas **[especificaciones](#web)**
![Paso #48](img/paso48.png)

- <a id="admin"></a>Para entrar al administrador ➡️ ```44.193.189.248/burgerland/admin/```
![Paso #49](img/paso49.png)

- Puedes realizar los cambios necesarios para personalizar tu tienda ya que por defecto viene instalado este **[tema](#por_defecto)**.
![Paso #50](img/paso50.png)

- Aqui un simple cambio
![Paso #51](img/paso51.png)

> [!IMPORTANT]
> en caso de encontrarse con el siguiente error al ingresar al administrador de esta **[forma](#admin)**<br>
![Error-admin](img/error-admin.jpeg)<br>
Ingrese a la carpeta donde instalo **prestashop** ➡️ ```cd /var/www/html/burgerland``` y renombra la carpeta **admin...** ➡️ ```sudo mv admin313mltutiw29zaay9xz/ admin```
![Error-admin](img/importante.png)

> [!NOTE]
> <br>**admin313mltutiw29zaay9xz** ➡️ los números y letras **313mltutiw29zaay9xz** despues de **admin** varian para cada persona, busca la carpeta que empiece por **admin**.<br>
**admin** ➡️ es el nuevo nombre de la carpeta **admin313mltutiw29zaay9xz**
