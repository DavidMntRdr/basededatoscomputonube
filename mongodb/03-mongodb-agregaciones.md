# Agregaciones en MongoDB (Framework)

## Métodos para realizar agregaciones simples

- distinct() : Devuelve valores no duplicados
- countDocuments(): Cuenta los documentos en una colección
- estimateDocumentCount(): Cuenta de manera aproximada durante un periodo de tiempo

## Una Aggregation pipeline consta de una o más etapas (stage) que procesan documentos

1. Cada etapa realiza una operación en los documentos de entrada. Por ejemplo, una fase puede filtrar documentos, agrupar documentos y calcular valores.

2. Los documentos que se generan en una fase pasan a la siguiente fase.

3. Puede devolver resultados para grupos de documentos como totales, máximo, mínimo, etc.

### Se utiliza la cláusula "aggregate"

- Existe una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distintos tipos de etapa, de comparación, booleanos, aritméticos, de cadena, etc.

## Métodos simples: countDocuments() y distinct()

1. Contar los documentos de la colección libros.
```json
db.libros.countDocuments()
```

2. Contar los documentos de la editorial Terra.
```json
db.libros.countDocuments({editorial:{$eq:'Terra'}})

db.libros.countDocuments({editorial:'Terra'})
```

3. Mostrar todos los libros mostrando solamente la editorial.
```json
db.libros.find({},{_id:0, editorial:1})
```

4. Mostrar todas las distintas editoriales.
```json
db.libros.distinct('editorial')
```

[Documentación de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline básica
### Tienen funciones de etapa

```json
db.libros.aggregate(
    [
        {
        $match:{editorial:"Terra"}
        }
    ]
)
```

## $project. Incluir y renombrar campos

```json
db.libros.aggregate(
    [
        {
            $match: {editorial:'Terra'}
        },
        {
            $project:{
                _id:0,
                titulo:1,
                precio:1,
                NombreEditorial: "$editorial",
                editorial: 1
            }
        }
    ]
)
```

### Crear un campo calculado con $project

```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  }
]
```

## $sort. Ordenamiento 
```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        editorial: "Terra"
      }
  },
  {
    $project:
      /**
       * specifications: The fields to
       *   include or exclude.
       */
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre Editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        precio: 1
      }
  }
]
```

## $group. Agrupaciones

-- Cuántos documentos hay por cada una de las editoriales

```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero de documentos": {
          $count: {}
        }
      }
  }
]
```

-- Cuántos documentos hay por cada una de las editoriales por número de documentos de manera descendentes
```json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero de documentos": {
          $count: {}
        }
      }
  }
]
```


--Cuantos documentos por tipo de propiedad, mostrando el númmero de propiedades y el promedio de sus precios, utilizando MongoAtlas en la base de datos sample_airbnb

```json
[
  {
    $group:
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  }
]
```

-- Operador $set
```json
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $set:
      /**
       * field: The field name
       * expression: The expression.
       */
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  }
]
```


 - Operador $unset
```json
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $unset:

      "Media"
  }
]
```

con más de un campo: 
```json
[
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$property_type",
        Numero: {
          $count: {}
        },
        Media: {
          $avg: "$price"
        }
      }
  },
  {
    $unset:

      ["Media", "Media_Total"]
  }
]
```


Creando nuevas colecciones utilizando el operador $out
Nota: debe ser el último en la agregación

```json
{
  db: "sample_airbnb",
  coll: "media_propiedades"
}
```

- Ejemplos con operadores de comparación
```json
[
  {
    $project:
      {
        _id: 0,
        price: 1,
        name: 1,
        room_type: 1,
        caro: {
          $gt: ["$price", 300]
        },
        medio: {
          $and: [
            {
              $gte: ["$price", 100]
            },
            {
              $lte: ["$price", 300]
            }
          ]
        },
        barato: {
          $lt: ["$price", 100]
        }
      }
  }
]
```

- $cond. Devuelve valores según una condición (es parecido a un operador ternario de un lenguaje de programación).

```json
_id: 0,
  price: 1,
  name: 1,
  room_type: 1,
  caro: {
    $cond: [
      {
        $gt: ["$price", 300]
      },
      "Si",
      "NOOOOO."
    ]
  },
  medio: {
    $cond: [
      {
        $and: [
          {
            $gte: ["$price", 100]
          },
          {
            $lte: ["$price", 300]
          }
        ]
      },
      "Si",
      "No"
    ]
  },
  barato: {
    $cond: [
      {
        $lt: ["$price", 100]
      },
      "Si",
      "No"
    ]
  }
```

- Views
```json
db.createView("ganancias_libros"),
"libros",
[
  {
    $match:
      {
        editorial: "Biblio"
      }
  },
  {
    $project:
      {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        "Nombre editorial": "$editorial",
        "Total de ganancias": {
          $multiply: ["$precio", "$cantidad"]
        }
      }
  },
  {
    $sort:
      {
        "Total de ganancias": -1
      }
  }
]
```
```json
db.ganancias_Libros.find({'Total ganacias':{$lte:240}}, {titulo:1, 'Total ganancias':1}).sort({titulo:-1})
```