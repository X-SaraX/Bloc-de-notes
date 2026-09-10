En este repositorio se explica el paso a paso de la creación de una base de datos NoSQL en VS Code y como se levanta en local mediante MongoDB. 

# Paso 1 Crear el Package.json
Se puede hacer de dos formas:
  - De forma manual: `npm init` ->  Este comando te preguntará datos(name, version, etc...) para que los ingreses a mano.
  - De forma automática `npm init -y` -> Este comando rellenará todos los campos con la información del proyecto.
Tras este comando se creará un archivo de nombre "package.json"

# Paso 2 Instalar Express y Mongoose
Instalaremos Express pues permite crear servidores HTTP en Node.js.   
Instalaremos Mongoose porque permite conectar la base de datos a MongoDB y facilita el manejo de datos.

Con el comando: `npm install express mongoose`
Tras la instalación se creará un archivo de nombre "package-lock.json" y una carpeta de "node_modules" donde se guardarán todas las dependencias del proyecto. 
Como es un archivo pesado, suele ocultarse.
  - ### Como ocultar node_modules
      1. En la raiz del proyecto se crea un archivo llamado ".gitignore"
      2. Dentro de este archivo se escribe el nombre de lo que se quiere ocultar, en este caso "node_modules" (se escribe sin comillas, exactamente igual a como está escrito en el proyecto)        

# Paso 3 Preparar el Archivo .js
Si has creado el package.json de forma manual, deberás crear un archivo del mismo nombre que le pusiste al "main".   
Por otro lado, si has creado el package.json de forma automática tu archivo main tiene el nombre de "index.js" -> En esta guía usaremos esta opción.

Creamos un archivo y le ponemos el nombre que aparece en el main del package.json (en este caso index.js) 
  - ### Scripts de arranque
    Una vez tenemos lo anterior listo, deberemos volver a "package.json" y añadir unos scripts para facilitar el arranque y el desarrollo de la BD.
      1. Borra la línea de código dentro de los "scripts" de "package.json"
      2. añade el script de arranque -> `"start": "index.js"` // Ten cuidado de que aquí escribas el nombre de TÚ archivo principal (el del main)
      3. añade el script de desarrollo -> `"dev": "node --watch index.js"` // Revisa que escribas el nombre de TÚ archivo principal
           - "--watch" se encargará de que Node reinicie automáticamente tu aplicación cada vez que detecta cambios en los archivos del proyecto. Lo que evita que tengas que reiniciar a mano cuando cambies cualquier cosa en el código.
           
  Deberá quedar así:  
```
"scripts": {
  "start": "index.js",
  "dev": "node --watch index.js"
},
```



# Paso 4 Index.js
Dentro de tu archivo principal deberás importar express (que fue previamente instalado en la terminal)   
|--> `const express = require("express");`  
|  
A continuación se crea el servidor  
|--> `const server = express();`  
|  
Indicamos que se los datos que se van a tratar son de tipo json  
|--> `server.use(express.json());`  
|  
Definimos el puerto  
|-->`const PORT = 8080` //Puedes poner el puerto de tu preferencia (8080, 3000, etc...)

Deberá quedar así: 
```
const express = require("express");
const server = express();
const mongoose = require("mongoose");

server.use(express.json());

const PORT = 8080;
```


# Paso 5 Rutas

## 1. Ruta no encontrada 
Esta suele ser la ruta final, cuando la URL del cliente no lleva a ningún sitio de nuestra base de datos.
```
 server.use((req, res) => {
  return res.status(404).json({ error: "Route not found" });
});
```

# Paso 6 Levantamos el servidor

```
server.listen(PORT, () => {
    console.log("En escucha ") // Este console.log es para que veamos algo, pero podría omitirse o cambiarse con cualquier mensaje
})
```
