En este repositorio se explica el paso a paso de la creación de una base de datos NoSQL en VS Code y como se levanta en local mediante MongoDB. 
- Para ello es necesario tener instalado MongoDB Compass y VS Code

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
      2. añade el script de arranque -> `"start": "node index.js"` // Ten cuidado de que aquí escribas el nombre de TÚ archivo principal (el del main)
      3. añade el script de desarrollo -> `"dev": "node --watch index.js"` // Revisa que escribas el nombre de TÚ archivo principal
           - "--watch" se encargará de que Node reinicie automáticamente tu aplicación cada vez que detecta cambios en los archivos del proyecto. Lo que evita que tengas que reiniciar a mano cuando cambies cualquier cosa en el código.
           
  Deberá quedar así:  
```
"scripts": {
  "start": "node index.js",
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

Iremos agregando las rutas que querramos/necesitemos. Siempre puedes seguir los pasos siguientes y si en algún momento necesitas otra ruta más, volver y crearla.
## 1. Ruta no encontrada 
Esta suele ser la ruta final, cuando la URL del cliente no lleva a ningún sitio de nuestra base de datos.
```
 server.use((req, res) => {
  return res.status(404).json({ error: "Route not found" }); //El mensaje de error no es necesario
});
```

# Paso 6 Levantamos el servidor

```
server.listen(PORT, () => {
    console.log("En escucha ") // Este console.log es para que veamos algo, pero podría omitirse o cambiarse con cualquier mensaje
})
```



# Paso 7 parametrizar
A continuación para mayor organización crearemos una carpeta en el proyecto de vs code donde irá todo el código de la API, en este ejemplo yo la llamaré "src"  
|  
|---> Dentro de la carpeta "src" crearemos una nueva carpeta "config" (los nombres son orientativos, puedes cambiarlos)  
.............|--> Dentro de "config" crea un fichero "connect.js" (los nombres son orientativos, puedes cambiarlos)

## connect.js
1. Importamos mongoose para utilizar su método de conexión
  ```const mongoose = require("mongoose");```
2. Definimos una función de conexión con Mongo.  
   Será una función asíncrona de "trycatch" -->
   ```
   const connectDB = async () => {
   try {
   } catch (error) {

   }
   ```

En el "try" añadiremos la función de mongoose con el link de conexión de nuestro MongoDB:  
La palabra que pongas después del link de conexión (en este ejemplo "movies"), será el  nombre de tu base de datos  
```await mongoose.connect("mongodb://localhost:27017/movies")```  
Además de una línea de confirmación (este paso es opcional pero recomendable)  
```console.log("Conectado con la base de datos de MongoDB")```  
En el catch podemos agregar un "console.error" que muestre el mensaje (esto también es opcional pero ayuda en la depuración de código)  
```console.error("Fallo al conectarse a MongoDB", error.message)```  
. . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . |--> "error.message" imprimirá el error en caso de que haya.   
Completada la función quedaría así:  
```
const connectDB = async () => {
  try {
    await mongoose.connect("mongodb://localhost:27017/movies");
    console.log("Conectado con la base de datos de MongoDB");
  } catch (error) {
    console.error("Fallo al conectarse a MongoDB", error.message);
  }
};
```
### Exportar la función de connect.js
Para poder usarla la exportaremos fuera del archivo con ```module.exports = connectDB```  

### Importar la función en index.js
Para usar la función la importamos dentro del archivo "index.js" justo debajo de la importación de express  
```const connectDB = require("./src/config/connect")```  
. . . . . . . . . . . . . . . . . . . . . . . . . . |--> asegúrate que la ruta coincide con el nombre de tus ficheros/carpetas 


Por último llamámos a la función para conectarnos --> ```connectDB()```   
El "index.js" quedaría así:  
```
const express = require("express");
const connectDB = require("./src/config/connect");
const server = express();
const mongoose = require("mongoose");

server.use(express.json());

connectDB();

const PORT = 8080;

// -----RUTAS-------

server.use((req, res) => {
  return res.status(404).json({ error: "Route not found" });
});

//Levantamos el servidor
server.listen(PORT, () => {
  console.log("En escucha ");
});
```

Esa sería la estructura de una base de datos básica levantada en el servidor local con MongoDB. Solo falta personalizarla con tus modelos, semillas, controladores y rutas. :)
