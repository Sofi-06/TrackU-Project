# instalaciones:

npm install express multer csv-parser mysql2
npm install bcrypt
npm install ejs
npm install sequelize mysql2 bcryptjs express-session body-parser
npm install bcryptjs

# ejs:

EJS (Embedded JavaScript) es un motor de plantillas que permite generar HTML dinámico en aplicaciones web utilizando JavaScript.

- La sintaxis de EJS:
  Permite insertar variables y estructuras de control directamente en el HTML:
- utilizando los delimitadores: <%= %> para la salida de variables y <% %> para ejecutar código JavaScript.
- Por ejemplo:
  <%= user.name %> inserta el nombre del usuario en la página, y el código <% users.forEach(user => { %> permite iterar sobre un array de usuarios.


# Sequelize

Sequelize es un ORM (Object-Relational Mapping) para Node.js que facilita la interacción con bases de datos relacionales como MySQL, PostgreSQL, SQLite y MariaDB. En lugar de escribir consultas SQL manualmente, Sequelize permite manejar la base de datos mediante modelos que representan tablas, lo que simplifica la manipulación de datos.

  
# Multer:
Es un middleware para Node.js que se utiliza para gestionar la carga de archivos en aplicaciones web. 
Está diseñado para trabajar con formularios que contienen archivos, permitiendo que el servidor reciba 
y almacene estos archivos en una ubicación especificada.

## Principales características de Multer:
* Procesamiento de archivos enviados desde el cliente: 
Multer analiza los datos del formulario que contienen archivos y los convierte en objetos que se pueden gestionar fácilmente en Node.js.

* Almacenamiento: 
Multer ofrece opciones para almacenar los archivos cargados en el sistema de archivos local o en la memoria RAM.

* DiskStorage: 
Permite definir la ubicación (carpeta) y el nombre de los archivos que se van a almacenar.

* MemoryStorage: 
Almacena el archivo en la memoria RAM como un buffer.

* Control sobre el tamaño de los archivos: 
Multer puede configurarse para limitar el tamaño de los archivos que se suben, 
lo que es útil para evitar que se suban archivos demasiado grandes que puedan afectar el rendimiento del servidor.

* Filtro de archivos: 
Se puede filtrar qué tipos de archivos son aceptados o rechazados según el tipo MIME (Multipurpose Internet Mail Extensions).


# base de datos

DROP DATABASE proyecto_pasantia;
CREATE DATABASE IF NOT EXISTS proyecto_pasantia;
USE proyecto_pasantia;

CREATE TABLE users (
id INT(11) NOT NULL AUTO_INCREMENT,
name VARCHAR(100) NOT NULL,
identification VARCHAR(50) NOT NULL UNIQUE,
password VARCHAR(255) NOT NULL,
email VARCHAR(100) NOT NULL UNIQUE,
role ENUM('admin', 'docente', 'estudiante') NOT NULL,
username VARCHAR(50) NOT NULL UNIQUE,
PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE projects (
id INT(11) NOT NULL AUTO_INCREMENT,
project_code VARCHAR(50) NOT NULL UNIQUE,
project_name VARCHAR(100) NOT NULL UNIQUE,
project_type ENUM('pasantía', 'artículo', 'trabajo de grado') NOT NULL,
teacher_id INT(11) DEFAULT NULL,
createdAt DATETIME DEFAULT CURRENT_TIMESTAMP,
updatedAt DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (id),
FOREIGN KEY (teacher_id) REFERENCES users(id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE deliverables (
id INT(11) NOT NULL AUTO_INCREMENT,
project_id INT(11) NOT NULL,
name VARCHAR(100) NOT NULL,
description TEXT,
submission_time DATETIME DEFAULT CURRENT_TIMESTAMP,
due_date DATE NOT NULL,
status ENUM('pendiente', 'entregado', 'revisado') NOT NULL,
PRIMARY KEY (id),
FOREIGN KEY (project_id) REFERENCES projects(id)

) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE files (
id INT(11) NOT NULL AUTO_INCREMENT,
deliverable_id INT(11) NOT NULL,
autor VARCHAR(100) NOT NULL,
originalname VARCHAR(100) NOT NULL,
filename VARCHAR(100) NOT NULL,
filepath VARCHAR(255) NOT NULL,
status ENUM('pendiente', 'entregado', 'revisado') NOT NULL,
score INT(11) NULL,
PRIMARY KEY (id),
FOREIGN KEY (deliverable_id) REFERENCES deliverables(id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

CREATE TABLE project_users (
id INT(11) NOT NULL AUTO_INCREMENT,
project_id INT(11) NOT NULL,
user_id INT(11) NOT NULL,
createdAt DATETIME DEFAULT CURRENT_TIMESTAMP,
updatedAt DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
PRIMARY KEY (id),
FOREIGN KEY (project_id) REFERENCES projects(id),
FOREIGN KEY (user_id) REFERENCES users(id)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



- createdAt:
  Se establece con un valor predeterminado de CURRENT_TIMESTAMP para almacenar la fecha y hora en la que se inserta el registro.
- updatedAt:
  Se establece con CURRENT_TIMESTAMP como valor predeterminado, y se actualiza automáticamente cada vez que el registro es modificado, gracias a la cláusula ON UPDATE CURRENT_TIMESTAMP.


  # Estructura del proyecto


login/
│
├── models/
|   ├── Deliverables.js  
|   ├── File.js
|   ├── Project.js
|   ├── ProjectUser.js
|   ├── User.js
|
|
├── node_modules/
|
├── public/
│   ├── css/
│   ├── img/
│
├── uploads/
│   └── (Almacenamiento temporal de archivos CSV y archivos subidos)
│
├── views/
|
|
├── app.js
├── db.js
├── createAdmin.js
└── package.json


# 

login/
├── models/         # Modelos Sequelize (e.g., Deliverables.js, File.js)
├── public/         # Recursos estáticos (CSS, imágenes)
├── uploads/        # Almacenamiento temporal de archivos CSV y archivos
├── views/          # Plantillas EJS para las vistas del sistema
├── app.js          # Punto de entrada de la aplicación
├── db.js           # Configuración de la conexión a MySQL
├── createAdmin.js  # Script para crear un usuario administrador inicial
└── package.json    # Configuración de dependencias
