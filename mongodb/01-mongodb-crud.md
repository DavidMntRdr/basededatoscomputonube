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

 # Práctica 1

 ## Cargar datos
 [Libros.json](./data/libros.json)

## Búsquedas. Condiciones Simples de Igualdad. Método find()
 1. Seleccionar todos los documentos de la colección 'libros'

 ```json
 db.libros.find({})
 ```

 2. Seleccionar o mostrar todos los documentos que sean de la editorial biblio

```json
 db.libros.find({editorial:'Biblio'})
 ```

 3. Mostrar todos los documentos que el precio sea 25

```json
 db.libros.find({precio:25})
 ```

 4. Seleccionar todos los documentos dónde el título sea json para todos

 ```json
 db.libros.find({titulo:'JSON para todos'})
 ```

 ## Operadores de Comparación
[Operadores de comparación](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores de comparación](../img/Operadores_relacionales.png)

1. Mostrar todos los documentos donde el precio sea mayor a 25

```json
db.libros.find({ precio: { $gt: 25 } })
```

2. Mostrar los documentos donde el precio sea 25
```json
db.libros.find({ precio: { $eq: 25 } })
```

3. Mostrar los documetos que cuya cantidad sea menor a 5

```json
db.libros.find({ cantidad: { $lt: 5 } })
```

4. Mostrar los documentos que pertenezcan a la editorial Biblio ó Planeta

 ```json
db.libros.find({ editorial: { $in: ['Biblio', 'Planeta'] } })
 ```

 5. Mostrar todos los documentos de libros que cuesten 20 o 25

 ```json
db.libros.find({ precio: { $in: [20, 25] } })
 ```

 6. Mostrar todos los documentos de libros que no cuesten 20 o 25
 
```json
db.libros.find({ precio: { $nin: [20, 25] } })
```

7. Mostrar el primer domento de libros que cuesten 20 o 25

```json
 db.libros.findOne({ precio: { $in: [20, 25] } } )
```
## Operadores lógicos
[Operadores de lógicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores de comparación](../img/Operadores-logicos.png)

### Operador AND

Dos posibles opciones de AND

1. La simple, mediante condiciones separadas por comas

***sintaxis***

db.coleccion.find({condicion1, condicion2}) -> Con esto asume que es una ***and***

2. Usando el operador $and

***sintaxis***

db.coleccion.find( {$and: [ {condicion1}, {condicion2} ] } )

1. Mostrar todos aquellos libros que cuesten más de 25 y cuya cantidad sea inferior a 15

***forma simple***

```json
db.libros.find({ precio: { $gt: 25 }, cantidad: { $lt: 15 } } )```

***AND***

```json
db.libros.find( {$and: [ {precio:{$gt:25}}, {cantidad: {$lt:15}} ] } )
```

2. Mostrar todos aquellos libros que cuesten más de 25 y cuya cantidad sea inferior a 15 y id 

```json
db.libros.find({ precio: { $gt: 25 }, cantidad: { $lt: 15 }, _id:4 } )
```

```json
db.libros.find({ precio: { $gt: 25 }, cantidad: { $lt: 15 }, _id:{$eq:4} } )
```

```json
db.libros.find(
    {
      $and:[
        {precio:{$gt:25}},
        {cantidad: {$lt:15}}
      ]
    }
)
```

### Operador OR

**Sintaxis:**

json
db.libros.find({$or:[{condicion1},{condicion2}]})



#### Ejercicios 

1. Mostrar todos aquellos libros que cuesten más de 25 o cuya cantidad sea inferior a 15 ####
```json
db.libros.find({$or:[{precio:{$gt:25}},{cantidad:{$lt:15}}]})
```

### AND y OR combinados

1. Mostrar los libros de la editorial biblio con precio mayor a 40 o libros de la editorial planeta con precio mayor a 30
```json
db.libros.find(
{
    $or: [
        {$and:[{editorial:'Biblio'}, {precio:{$gt:30}}]},
        {$and:[{editorial:{$eq:'Planeta'}}, {precio:{$gt:20}}]}
    ]
})
```

## Proyección de columnas
```
db.coleccion.find(filtro, columnas)
```

```json
db.libros.find({}, {titulo: 1})
```

1. Seleccionar todos los documentos, mostrando el título y la editorial

```json
db.libros.find({},{titulo:1, editorial:1})
db.libros.find({},{titulo:1, editorial:1, _id:0})

```

2. Seleccionar todos los documentos de la editorial planeta, mostrando solamente el título y la editorial

```json
db.libros.find({editorial:"Planeta"}, {titulo:1, editorial:1, _id:0})
```


## Operador exists(Permite saber si un campo se encuentra o no en un documento)

```json
db.libros.find(
  {
    editorial:{$exists:true}
  }
)

db.libros.insertOne(
{
  _id:10,
  titulo: 'Mongo en entornos gráficos',
  editorial: 'Terra',
  precio: 125
}
)
```
1. Mostrar todos los documentos que no contengan el campo cantidad

```json
db.libros.find(
{
  cantidad: {$exists: false}
}
)
```

## Operador Type (Permite preguntar si un determinado campo corresponde con un tipo)

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostrar todos los documentos donde el precio sean dobles

```json
db.libros.find({precio:{$type:1}})

db.libros.find({precio:{$type:16}})

db.libros.insertOne({
  _id: 13,
  titulo: 'Python para todos',
  editorial: 2001,
  precio: 200,
  cantidad: 30
})


db.libros.insertMany([{
  _id: 12,
  titulo: 'IA',
  editorial: 'Terra',
  precio: 125.5,
  cantidad: 20
}, {
  _id: 13,
  titulo: 'Python para todos',
  editorial: 2001,
  precio: 200,
  cantidad: 30
}])
```

2. Seleccionar los documentos donde la editorial sea de tipo entero
```json
db.libros.find({editorial:{$type:16}})
db.libros.find({editorial:{$type:'int'}})

```
3. Seleccionar todos los documentos donde la editorial sea string
```json
db.libros.find({editorial:{$type:2}})
db.libros.find({editorial:{$type:'string'}})

```


## Pracitca de consultas
1. Instalar las tools de mongodb
[DatabaseTools]

2. Cargar el json empleados (Debemos estar ubicados en la carpeta donde se ecnuentra el JSON empleados)

- En local: 
  comando: 
     mongoimport --db curso --collection empleados --file empleados.json  

- Docker:
       mongoimport --db curso --collection empleados --file empleados.json --port 27 


# Modificando Documentos
## comandos importantes

1. updateOne -> Modificar un solo documento.
2. updateMany -> Modificar múltiples documentos.
3. replaceOne -> Sustituir el contenido completo de un documento.

```json
db.collection.updateOne(
  {filtro},
  {operador: }
)
```

[Operadores Update](https://www.mongodb.com/docs/manual/reference/operator/update/)

## Operador set

1. Modificar un documento
```json
db.libros.updateOne({titulo: 'Python para todos'}, {$set: {titulo:'Java para todos'}})
```

```json
db.libros.updateOne({_id:2}, {$set:{precio:56, existencia:10}})
```

2. Actualizar el precio a 100 y la cantidad a 50 para el _id:10

```json
db.libros.updateOne({_id:10}, {$set: {precio:100, cantidad:50}})
```
### Modificar múltiples documentos

-- Modificar todos los documentos donde el precio sea mayor a 100 a un precio de 150
```json
db.libros.updateMany(
  {precio: {$gt:100}},
  {$set: {precio: 150}}
)
```

2. Operador $inc y $mul
- Actualizar con un incremento de 5 todos los documentos
```json
db.libros.updateMany(
  {},
  {$inc: {precio:5}}
)
```

- Actualizar con multiplicación de 2 todos los documentos que la cantidad sean mayores a 20
```json
db.libros.updateMany(
  {cantidad:{$gt:20}}, {$mul:{cantidad:2}}
)
```

- Actualizar todos los documentos donde el precio sea mayor a 20 y se multipliquen por 2 la cantidad y el precio
```json
db.libros.updateMany(
  {precio:{$gt:20}}, {$mul:{cantidad:2, precio:2}}
)
```

3. Reemplazar documentos (replaceOne)
```json
db.libros.replaceOne({_id:2}, 
    {
      titulo:'Cartas a Milena', 
      autor: 'Franz Kafka',
      precio: 500
    }
)
```

# Borrar documentos
- deleteOne -> Elimina un solo documento
- deleteMany -> Elimina múltiples documentos

1. Eliminar el documento con id 2

```json
db.libros.deleteOne({_id:2})
```

2. Eliminar los documentos donde la cantidad sea mayor o igual a 150

```json
db.libros.deleteMany({cantidad: {$gte: 150}})
```

# Expresiones regulares

1. Buscar los libros que contengan en el título la letra t
```json
db.libros.find({titulo:/t/})
```

2. Buscar los libros que en el título contengan la palabra json
```json
db.libros.find({titulo:/JSON/})
```

3. Buscar todos los documentos que en el titulo terminen en tos 
```json
db.libros.find({titulo:/tos$/})
```

4. Todos los documentos que en el título comiencen con J
```json
db.libros.find({titulo:/^J/})
```

# Operador $regex
[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

- Seleccionar los libros que contengan la palabra para
```json
db.libros.find({titulo:{$regex: 'para'}})
```

```json
db.libros.find({titulo: {$regex: /JSON/}})
```
- Distinguir entre mayúsculas y minúsculas
```json
db.libros.find({titulo: {$regex: {/json/}}}) ->No distingue entre mayúsculas y minúsculas
```

```json
db.libros.find({titulo: {$regex: /json/, $options:"i"}}) -> Si distingue entre mayúsculas y minúsculas
```

```json
db.libros.find({titulo: {$regex: /json/i}})
```

- Seleccionar todos los libros que comiencen con J o j
```json
db.libros.find({titulo: {$regex: /^j/i}})
```

- Seleccionar todos los libros que terminen es
```json
db.libros.find({titulo: {$regex: /es$/i} })

db.libros.find({titulo: {$regex: 'es$', $options: 'i'} })
```

# Método sort (Ordenar documentos)

1. Ordenar los libros de manera ascendente por el precio
```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio: 1})
```

2. Ordenar los libros de manera descendente por el precio
```json
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio: -1})
```

3. Ordenar los libros de manera ascendente por la editorial y de manera descendente por el precio, mostrando el titulo, el precio y la editorial

```json
db.libros.find({}, {titulo:1, precio:1, editorial:1, _id:0}).sort({editorial: 1, precio:-1})
```

# Otros métodos: skip, limit, size



```json
db.libros.find({}).size()

db.libros.find({titulo: {$regex:/Java/i}}).size()
```

- Buscar todos los libros pero mostrando los 2 primeros
```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).limit(2)
```

- Mostrar los 3 últimos libros 
```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).sort({precio:-1}).limit(3)

db.getCollection('libros')
  .find({}, { titulo: 1, editorial: 1, _id: 0 })
  .sort({ titulo: -1 })
  .limit(4);
```

```json
db.libros.find({}, {titulo: 1, editorial:1, precio:1}).skip(2)
```

-Seleccionar todos los libros ordenados por título de forma descendente saltando los dos primeros documentos y mostrando el tamaño

```json
db.libros.find({}, {titulo:1, editorial:1, precio:1, _id:0}).sort({titulo:-1}).skip(2).size()
```

#Borrar colecciones y base de 
```json
use db5
db.createCollection('ejemplo')
show collections

db.ejemplo.insertOne({nombre: 'Chapuin'})

db.ejemplo.drop()

db.dropDatabase()
```

