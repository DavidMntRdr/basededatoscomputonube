# CRUD y consultas en MongoDB

## Crear una base de datos
Solo se crea si contiene por lo menos una colección
use bd1

## Cómo crear una colección
use bd1
db.createCollection("Empleado")

## Mostrar las colecciones
show collections

## Insertar un documento
```json
db.coleccion.insertOne(
    {
        nombre: 'Estefany',
        apellido1: 'Vazquez',
        edad: 22,
        ciudad: 'Huehuetoca'
    }
)
```

## Inserción de un documento más complejo con array
```json
db.alumnos.insertOne(
 {
   nombre: "Martha",
   apellido: "Trinidad",
   apellido2: "Hernandez",
   edad: 19,
   aficiones: [
                 "Dibujo", "Videojuegos", "Musica"
              ]
 }
 )
 ```

 ## Inserción de documentos más complejos con documentos anidados
 ```json
db.alumnos.insertOne(
 {
    nombre: "Naomi",
    apellido: "Mondragon",
    apellido2: "Martinez",
    edad: 20,
    estudios: [
      "Licenciatura en Derecho",
      "Licenciatura en Psicología",
      "Licenciatura en Logística"
    ],
       esperiencia: {
        lenguaje: "SQL", sbd: "SQL Server", AniosExp: 2
        }
 }
 )

db.alumnos.insertOne(
 {
    _id: 3,
    nombre: "Sorge",
    apellido: "Huaso",
    apellido2: "Villegas",
    edad: 23,
    aficiones: [
      "Dinero",
      "Hombres",
      "Fiesta"
    ],
       talenos: { 
        embriagarse: true,
        bañarse: false
        }
 }
 ) 
 ```

## Insertar múltiples documentos 
 ```json
db.alumnos.insertMany([
  { 
    _id: 12, 
    nombre: "Nancy", 
    apellido: "Gonzales", 
    edad: 24, 
    descripcion: "Es una chistosa" 
  },
  { 
    nombre: "Dan", 
    apellido: "Urtiz", 
    edad: 24, 
    habilidades: ["Programar", "Redactar", "Analizar"], 
    Direcciones: { 
      calle: "Santa Anita", 
      numero: "123" 
    }, 
    esposas: [ 
      { 
        nombre: "Yanahi", 
        edad: 23, 
        pension: "2800", 
        hijos: ["Maximiliano", "Alan"]
      },
      {
        nombre: "Yolotzin", 
        edad: 32, 
        pension: "6500.35", 
        complaciente: true 
      }
    ]
  }
])
 ```

 # Práctica

 1. Bases de Datos, colecciones e inserts

    - Nos conectamos con mongosh al MongoDB
    . Crear una base de datos denominada curso
    ```json
    use curso
    ```
    
    - Verificar que la base de datos no existe
     ```json
    show dbs
    ```

    - Crear una colección denominada facturas y comprobar que aparece tanto la colección como la base de dato
     ```json
    db.createCollection("facturas")
    ```

