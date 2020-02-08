***PRUEBA TÉCNICA DE MICROSERVICIOS PARA PLAYVOX -  FEBRERO 2020***

**1. DESCRIPCIÓN**

Se desarrolló un proyecto que permite efectuar operaciones CRUD de usuarios y notas, a través de Micro-Servicios Web RestFul, utilizando el Framework Django. Se implementaron cuatro (4) contenedores en docker para los microservicios:

1. UserServices (PORT: 8000, CRUD de Usuarios)
2. MongoService (DB: Users, PORT: 27017)
3. NoteServices (PORT: 8001, CRUD de Notas)
4. MongoService (DB: Notes, PORT: 27017)

El Micro-Servicio alojado en el tercer contenedor (NoteServices), se conecta al primero (UserServices), cuando se consulta o se pretender crear una nueva nota, con el fin de validar la existencia de los usuarios.

&nbsp;
&nbsp;

**2. CONFIGURACIÓN**

Los MicroServicios en Django, fueron desplegados en modo Debug (No los desplegué con Gunicorn), y utilizan los puertos 8000 y 8001, ambos se configuraron para correr en la máquina host, con la dirección _localhost._ Las bases de datos corren en contenedores diferentes, a través del puerto 27017.

Se desarrolló una página Web básica para interactuar con los diferentes MicroServicios, utilizando el framework Vue.js. Con el fin de no desplegar un contendor adicional para la página Web, utilicé las librerias CDN de Vue, por lo tanto, para ejecutar la página no se requiere ningún servidor Web, únicamente se abre el archivo con el navegador.

Los puertos 8000 y 8001 no pueden estar ocupados en el equipo host.

&nbsp;
&nbsp;

**3. VERSIONAMIENTO**

```
docker Server v. 19.03.1 docker Client v. 19.03.1 
docker compose version 1.21.0-3 
mongoDB v. 4.0.11
django v. 2.2.3
django rest framework v. 3.10.2 
djongo v. 1.2.33 

```

&nbsp;
&nbsp;

**4. INSTALACIÓN**

Clonar el proyecto utilizando GIT CLONE.

Ejecutar los siguientes comandos, teniendo instalado docker, con sus diferentes líbrerias en el equipo host:

```
docker pull maresp85/testplayvox:userservices

docker pull maresp85/testplayvox:noteservices

```
Ubicarse por consola en la ruta donde se clonó el proyecto y ejecutar el siguiente comando, con el fin de utilizar el archivo **docker-compose.yml**, y de esta manera, traer la imagen de mongo y crear los contenedores:


```
docker-compose up -d
```

Verificar que los cuatro contenedores estén instalados y corriendo, para esto se ejecuta el comando: docker ps -a

Abrir la página **users.html** para testear los MicroServicios. También se puede utilizar POSTMAN para verificar el funcionamiento de los microservicios.

&nbsp;
&nbsp;

**5. URIs**

| **GET** (listar usuarios), **POST** (crear usuarios), **PUT** (actualizar usuarios),   **DELETE** (borrar usuarios) | http://localhost:8000/v1/users/ |
| --- | --- |
| **GET** (paginación, todos los usuarios) | http://localhost:8000/v1/userslist/ |
| **GET** (con parámetro query, para búsqueda de usuarios por nombre) | http://localhost:8000/v1/usersearch/ |
| **GET** (con parámetro id, para validar la existencia de un usuario | http://localhost:8000/v1/usercheck/ |
| **GET** (con parámetro id, para búsqueda de notas de un sólo usuario) | [http://localhost:8001/v1/notes/1](http://localhost:8001/v1/notes/1) |
| **POST** (para crear notas) | http://localhost:8001/v1/notes |


&nbsp;
&nbsp;

**6. TESTING**

Se efectuaron pruebas con el aplicativo POSTMAN de cada una de las URI con sus correspondientes métodos.

&nbsp;
&nbsp;


**7. PAGINACIÓN**

A modo de demostración, la URI: [http://localhost:8000/v1/userslist/](http://localhost:8000/v1/userslist/), retorna todos los usuarios y la información para realizar la paginación en el frontend indicando cantidad de usuarios en la página, total de páginas, siguiente y previa URI.

&nbsp;
&nbsp;

**8. AUTENTICACIÓN**

No se implementó autenticación por token o JWT, sin embargo, ambos métodos los he implementado en otros proyectos.

&nbsp;
&nbsp;

**9. CONSIDERACIONES**

Para evitar duplicidad de usuarios, se realiza validación con el campo e-mail. La búsqueda de usuario se configuró con el campo first_name.

No se desplegó el proyecto con el servidor Web Gunicorn u otro equivalente. Para efectos de prueba, se utilizó el servidor Web debug integrado de Django.
