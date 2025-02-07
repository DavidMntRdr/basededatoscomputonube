# Practica 1: Base de datos, colecciones e inserts

1. Conectarnos con mongosh a mongodb
1. Crear una base de datos llamada curso
1. Comprobar que la base de datos no existe 
1. Crear una colección que se llame factura y mostrarla
``` json
curso> db.createCollection("facturas")
show collections
```
5. Insertar un documento con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |

```json
db.facturas.insertOne(
    {
         cod_factura: 10,
         cliente: "Frutas Ramirez",
         Total: 223
    }
 )
```

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 20 |
| Ciente | Materiales Jorobas |
| Total | 502 |
```json
db.facturas.insertOne(
    {
         cod_factura: 20,
         cliente: "Materiales Jorobas",
         Total: 502
    }
 )
```

6. Crear una nueva colección pero usando directamente el insertOne.
    Insertar un documento en la colección productos con los siguienetes datos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 1 |
| Nombre | Tornillo G-8  5/8"x2" |
| Precio | 4 |
| Unidades | 1500 |

```json
db.productos.insertOne(
    {
         cod_Producto: 1,
         Nombre: "Tornillo G-8  5/8 x 2",
         Precio: 4,
         unidades: 1500
    }
 )
```

7. Crear un nuevo documento de producto que contenga un array con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 2 |
| Nombre | Martillo tubular |
| Precio | 90 |
| Unidades | 50 |
| Fabricantes | fab1, fab2, fab3, fab4 |

```json
db.productos.insertOne(
    {
         cod_Producto: 2,
         Nombre: "Martillo tubular",
         Precio: 90,
         unidades: 50,
         fabricantes: [ "fab1", "fab2", "fab3", "fab4"]
    }
 )
```

8. Borrar la colección facturas y comprobar que se borró
``` json
db.facturas.drop()
show collections
```

9. Insertar un documento en una colección denominada **fabricantes** para probar los subdocumentos y la clave _id personalizada

| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| Nombre | fab1 |
| Localidad | ciudad: buenos aires, pais: argentina, Calle: Calle pez 29, cod_postal:2900 |

```json
db.fabricantes.insertOne(
    {
         id: 1,
         Nombre: "fab1",
         Localidad: {
                 ciudad: "buenos aires", 
                 pais: "argentina", 
                 Calle: "Calle pez 29",
                 cod_postal: "fab4"
                }
    }
 )
```

10. Realizar una inserción de varios documentos en la colección productos

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 3 |
| Nombre | Alicates |
| Precio | 10 |
| Unidades | 25 |
| Fabricantes | fab1, fab2, fab5 |

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 4 |
| Nombre | Arandela |
| Precio | 1 |
| Unidades | 500 |
| Fabricantes | fab1, fab2, fab5 |


```json
db.productos.insertOne(
    {
          {
          cod_Producto: 3,
          Nombre: "Alicates",
          Precio: 10,
          unidades: 25,
          fabricantes: [ "fab1", "fab2", "fab5"]
          },
          {
          cod_Producto: 4,
          Nombre: "Arandela",
          Precio: 1,
          unidades: 500,
          fabricantes: [ "fab1", "fab3", "fab4"]
          }
    }
 )
```