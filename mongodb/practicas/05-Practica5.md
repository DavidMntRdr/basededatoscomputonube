# Agregaciones

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “productos.json”
3. Debes poner el resultado de las consultas en cada apartado

- Cuenta los productos de tipo “medio”, usando un método básico
```json
db.productos.find({ tipo: "medio" }).count()

Resultado:
25
```

- Indicar con un distinct, las empresas (fabricantes) que hay en la colección
```json
db.productos.distinct("fabricante")

Resultado:
[
  'A.O. Smith',
  'Alere',
  'American Tire Distributors Holdings',
  'Anthem',
  'Archrock',
  'Ascena Retail Group',
  'AutoNation',
  'Best Buy',
  'CIT Group',
  'Cabot',
  'Comcast',
  'Comerica',
  'Core-Mark Holding',
  'DST Systems',
  'Darling Ingredients',
  'Delta Air Lines',
  'Delta Tucker Holdings',
  "Dick's Sporting Goods",
  'First Solar',
  'HCA Holdings',
  'Hanesbrands',
  'Hartford Financial Services Group',
  'Hawaiian Holdings',
  'HealthSouth',
  'Hyatt Hotels',
  'Kar Auction Services',
  'Kelly Services',
  'Kemper',
  'Kimberly-Clark',
  'Lennar',
  'Mercury General',
  'Mondelez International',
  'Motorola Solutions',
  'Nasdaq OMX Group',
  'National Oilwell Varco',
  'Nordstrom',
  'OneMain Holdings',
  'Oneok',
  'Orbital ATK',
  'Pep Boys-Mann',
  'Pool',
  'Precision Castparts',
  'Primoris Services',
  'Raymond James Financial',
  'Seaboard',
  'Securian Financial Group',
  'Simon Property Group',
  'State Farm Insurance Cos.',
  'State Street Corp.',
  'SunPower',
  'TEGNA',
  'Telephone & Data Systems',
  'Total System Services',
  'Tractor Supply',
  'TransDigm Group',
  'Trinity Industries',
  'TrueBlue',
  'Universal American',
  'Universal Health Services',
  'WGL Holdings',
  "Wendy's",
  'Werner Enterprises',
  'WestRock',
  'Williams-Sonoma'
]
```

- Usando aggregate, visualizar los productos que tengan más de 80 unidades
```json
db.productos.aggregate([{ 
    $match: { 
        unidades: { $gt: 80 } 
        } 
    }
])

Resultado:
[
  {
    _id: ObjectId('67d8e0fc6e3084d2cfeface8'),
    codigo: 0,
    nombre: 'Fantastic Wooden Fish',
    unidades: 95,
    precio: 291,
    fabricante: 'Kimberly-Clark',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefacea'),
    codigo: 2,
    nombre: 'Small Soft Fish',
    unidades: 96,
    precio: 189,
    fabricante: 'Primoris Services',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefacf4'),
    codigo: 12,
    nombre: 'Refined Concrete Salad',
    unidades: 90,
    precio: 129,
    fabricante: 'Universal Health Services',
    tipo: 'avanzado'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefad06'),
    codigo: 30,
    nombre: 'Small Rubber Pants',
    unidades: 89,
    precio: 16,
    fabricante: 'Hanesbrands',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefad09'),
    codigo: 33,
    nombre: 'Generic Concrete Hat',
    unidades: 82,
    precio: 70,
    fabricante: 'American Tire Distributors Holdings',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefad1d'),
    codigo: 53,
    nombre: 'Licensed Plastic Hat',
    unidades: 96,
    precio: 38,
    fabricante: 'Best Buy',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefad1e'),
    codigo: 54,
    nombre: 'Generic Metal Sausages',
    unidades: 84,
    precio: 77,
    fabricante: 'DST Systems',
    tipo: 'medio'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefad25'),
    codigo: 61,
    nombre: 'Sleek Rubber Keyboard',
    unidades: 82,
    precio: 33,
    fabricante: 'Alere',
    tipo: 'basico'
  },
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefad2a'),
    codigo: 66,
    nombre: 'Incredible Concrete Fish',
    unidades: 96,
    precio: 336,
    fabricante: 'Darling Ingredients',
    tipo: 'medio'
  }
]
```

- Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades
```json
db.productos.aggregate([{ 
    $match: { 
        unidades: { $lt: 10 } } 
        }, 
        { 
            $project: { _id:0, nombre: 1, unidades: 1, precio: 1 } 
        }])


Resultado:
[
  { nombre: 'Ergonomic Metal Ball', unidades: 5, precio: 246 },
  { nombre: 'Handmade Plastic Hat', unidades: 7, precio: 253 },
  { nombre: 'Ergonomic Metal Table', unidades: 0, precio: 94 },
  { nombre: 'Practical Frozen Chips', unidades: 0, precio: 305 },
  { nombre: 'Fantastic Metal Pants', unidades: 5, precio: 129 },
  { nombre: 'Intelligent Frozen Sausages', unidades: 3, precio: 111 },
  { nombre: 'Rustic Plastic Mouse', unidades: 5, precio: 24 }
]
```

- Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior
```json
db.productos.aggregate([{ 
    $match: { 
        unidades: { $lt: 10 } } 
    },
    { 
        $project: { _id:0, nombre: 1, unidades: 1, precio: 1, empresa:"$fabricante" } }
])


Resultado;
[
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard'
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods"
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services'
  },
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines'
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings'
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith'
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK'
  }
]
```

- Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.
```json
db.productos.aggregate([{ 
    $match: { 
        unidades: { $lt: 10 } } 
    }, 
    { $project: { 
        _id:0, nombre: 1, unidades: 1, precio: 1, empresa: "$fabricante", total: { 
            $multiply: ["$precio", "$unidades"] 
            } 
        } 
    }
])


Resultado
[
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    empresa: 'Seaboard',
    total: 1230
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    empresa: "Dick's Sporting Goods",
    total: 1771
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    empresa: 'Kelly Services',
    total: 0
  },
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    empresa: 'Delta Air Lines',
    total: 0
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    empresa: 'OneMain Holdings',
    total: 645
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    empresa: 'A.O. Smith',
    total: 333
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    empresa: 'Orbital ATK',
    total: 120
  }
]
```

- Hacer que el nombre salga en mayúsculas con el operador $toUpper
```json
db.productos.aggregate([{ 
    $project: { 
        nombre: { $toUpper: "$nombre" }, 
        unidades: 1, precio: 1 
        } 
    }
])
```

- Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”
```json
db.productos.aggregate([{ 
    $project: { _id:0, nombre: 1, tipo: 1, completo: { 
        $concat: ["$nombre", " - ", "$tipo"] } 
        } 
    }
])

Resultado:
[
  {
    nombre: 'Fantastic Wooden Fish',
    tipo: 'avanzado',
    completo: 'Fantastic Wooden Fish - avanzado'
  },
  {
    nombre: 'Rustic Concrete Pants',
    tipo: 'medio',
    completo: 'Rustic Concrete Pants - medio'
  },
  {
    nombre: 'Small Soft Fish',
    tipo: 'medio',
    completo: 'Small Soft Fish - medio'
  },
  {
    nombre: 'Practical Soft Pants',
    tipo: 'avanzado',
    completo: 'Practical Soft Pants - avanzado'
  },
  {
    nombre: 'Ergonomic Metal Ball',
    tipo: 'medio',
    completo: 'Ergonomic Metal Ball - medio'
  },
  {
    nombre: 'Small Steel Gloves',
    tipo: 'avanzado',
    completo: 'Small Steel Gloves - avanzado'
  },
  {
    nombre: 'Ergonomic Wooden Shirt',
    tipo: 'avanzado',
    completo: 'Ergonomic Wooden Shirt - avanzado'
  },
  {
    nombre: 'Handmade Steel Chair',
    tipo: 'medio',
    completo: 'Handmade Steel Chair - medio'
  },
  {
    nombre: 'Handcrafted Soft Gloves',
    tipo: 'basico',
    completo: 'Handcrafted Soft Gloves - basico'
  },
  {
    nombre: 'Fantastic Concrete Salad',
    tipo: 'basico',
    completo: 'Fantastic Concrete Salad - basico'
  },
  {
    nombre: 'Handmade Plastic Hat',
    tipo: 'medio',
    completo: 'Handmade Plastic Hat - medio'
  },
  {
    nombre: 'Refined Wooden Tuna',
    tipo: 'avanzado',
    completo: 'Refined Wooden Tuna - avanzado'
  },
  {
    nombre: 'Refined Concrete Salad',
    tipo: 'avanzado',
    completo: 'Refined Concrete Salad - avanzado'
  },
  {
    nombre: 'Unbranded Soft Fish',
    tipo: 'basico',
    completo: 'Unbranded Soft Fish - basico'
  },
  {
    nombre: 'Small Concrete Fish',
    tipo: 'medio',
    completo: 'Small Concrete Fish - medio'
  },
  {
    nombre: 'Refined Concrete Bike',
    tipo: 'basico',
    completo: 'Refined Concrete Bike - basico'
  },
  {
    nombre: 'Tasty Cotton Pants',
    tipo: 'basico',
    completo: 'Tasty Cotton Pants - basico'
  },
  {
    nombre: 'Incredible Granite Gloves',
    tipo: 'avanzado',
    completo: 'Incredible Granite Gloves - avanzado'
  },
  {
    nombre: 'Practical Metal Mouse',
    tipo: 'basico',
    completo: 'Practical Metal Mouse - basico'
  },
  {
    nombre: 'Handcrafted Steel Chicken',
    tipo: 'medio',
    completo: 'Handcrafted Steel Chicken - medio'
  }
]
```

- Ordena el resultado por el campo “total”
```json
db.productos.aggregate([{ 
    $project: { _id:0, nombre: 1, unidades: 1, precio: 1, total: { 
        $multiply: ["$precio", "$unidades"] 
           } 
        } 
    },
    { $sort: { total: 1 } }
])


Resultado:

[
  {
    nombre: 'Practical Frozen Chips',
    unidades: 0,
    precio: 305,
    total: 0
  },
  {
    nombre: 'Ergonomic Metal Table',
    unidades: 0,
    precio: 94,
    total: 0
  },
  {
    nombre: 'Rustic Plastic Mouse',
    unidades: 5,
    precio: 24,
    total: 120
  },
  {
    nombre: 'Intelligent Frozen Sausages',
    unidades: 3,
    precio: 111,
    total: 333
  },
  {
    nombre: 'Awesome Soft Gloves',
    unidades: 35,
    precio: 18,
    total: 630
  },
  {
    nombre: 'Fantastic Metal Pants',
    unidades: 5,
    precio: 129,
    total: 645
  },
  {
    nombre: 'Ergonomic Steel Fish',
    unidades: 10,
    precio: 94,
    total: 940
  },
  {
    nombre: 'Practical Cotton Keyboard',
    unidades: 43,
    precio: 25,
    total: 1075
  },
  {
    nombre: 'Generic Fresh Keyboard',
    unidades: 69,
    precio: 16,
    total: 1104
  },
  {
    nombre: 'Handmade Frozen Salad',
    unidades: 13,
    precio: 85,
    total: 1105
  },
  {
    nombre: 'Ergonomic Metal Ball',
    unidades: 5,
    precio: 246,
    total: 1230
  },
  {
    nombre: 'Tasty Wooden Ball',
    unidades: 18,
    precio: 76,
    total: 1368
  },
  {
    nombre: 'Small Rubber Pants',
    unidades: 89,
    precio: 16,
    total: 1424
  },
  {
    nombre: 'Licensed Concrete Soap',
    unidades: 21,
    precio: 68,
    total: 1428
  },
  {
    nombre: 'Small Steel Gloves',
    unidades: 45,
    precio: 37,
    total: 1665
  },
  {
    nombre: 'Intelligent Soft Towels',
    unidades: 19,
    precio: 93,
    total: 1767
  },
  {
    nombre: 'Handmade Plastic Hat',
    unidades: 7,
    precio: 253,
    total: 1771
  },
  { nombre: 'Small Metal Tuna', unidades: 46, precio: 43, total: 1978 },
  {
    nombre: 'Ergonomic Metal Hat',
    unidades: 25,
    precio: 85,
    total: 2125
  },
  {
    nombre: 'Intelligent Metal Bike',
    unidades: 16,
    precio: 141,
    total: 2256
  }
]
```

- Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto
```json
db.productos.aggregate([{ 
    $group: 
        { _id: "$tipo", totalProductos: { $sum: 1 } } 
    }
])
```

- Añadir el valor mayor y el menor
```json
db.productos.aggregate([{ 
    $group: 
        { _id: null, 
            maxPrecio: 
                { $max: "$precio" }, 
            minPrecio: 
                { $min: "$precio" } 
        } 
    }
])

Resultado:

[
  { _id: 'medio', totalProductos: 25 },
  { _id: 'avanzado', totalProductos: 18 },
  { _id: 'basico', totalProductos: 24 }
]
```

- Añade el total de unidades por cada tipo
```json
db.productos.aggregate([{ 
    $group: 
        { _id: "$tipo", 
        totalUnidades: { $sum: "$unidades" } } 
    }
])


[
  { _id: 'medio', totalUnidades: 1224 },
  { _id: 'avanzado', totalUnidades: 773 },
  { _id: 'basico', totalUnidades: 1067 }
]
```

- Con el operador $set y el operador “$substr” visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.
```json
db.productos.aggregate([{ 
    $match: { nombre: "Small Metal Tuna" } 
    },
    { 
        $set: { primeros5: { 
            $substr: ["$nombre", 0, 5] 
            } 
        } 
    }
])

[
  {
    _id: ObjectId('67d8e0fc6e3084d2cfefad1f'),
    codigo: 55,
    nombre: 'Small Metal Tuna',
    unidades: 46,
    precio: 43,
    fabricante: 'Raymond James Financial',
    tipo: 'medio',
    primeros5: 'Small'
  }
]
```

- Creamos una salida que tenga el nombre del articulo y el total (precio por unidades) y lo guardamos en una colección denominada productos2
```json
db.productos.aggregate([{ 
    $project: { 
        _id:0, nombre: 1, total: { 
            $multiply: ["$precio", "$unidades"] 
            } 
        } 
    },
    { $out: "productos2" }
])
```

- Comprobamos que se ha creado
```json
db.showCollections()

Resultado:
[
  'personas',
  'productos',
  'libros',
  'cazas',
  'ciudades',
  'productos2'
]
```

- Hacemos un find para comprobar el resultado
```json
db.productos2.find()

Resultado:
[
  {
    _id: ObjectId('67d8e891675a05dae9013c4f'),
    nombre: 'Fantastic Wooden Fish',
    total: 27645
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c50'),
    nombre: 'Rustic Concrete Pants',
    total: 18414
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c51'),
    nombre: 'Small Soft Fish',
    total: 18144
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c52'),
    nombre: 'Practical Soft Pants',
    total: 2747
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c53'),
    nombre: 'Ergonomic Metal Ball',
    total: 1230
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c54'),
    nombre: 'Small Steel Gloves',
    total: 1665
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c55'),
    nombre: 'Ergonomic Wooden Shirt',
    total: 14994
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c56'),
    nombre: 'Handmade Steel Chair',
    total: 5392
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c57'),
    nombre: 'Handcrafted Soft Gloves',
    total: 4512
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c58'),
    nombre: 'Fantastic Concrete Salad',
    total: 12985
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c59'),
    nombre: 'Handmade Plastic Hat',
    total: 1771
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c5a'),
    nombre: 'Refined Wooden Tuna',
    total: 8480
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c5b'),
    nombre: 'Refined Concrete Salad',
    total: 11610
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c5c'),
    nombre: 'Unbranded Soft Fish',
    total: 9434
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c5d'),
    nombre: 'Small Concrete Fish',
    total: 5040
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c5e'),
    nombre: 'Refined Concrete Bike',
    total: 2700
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c5f'),
    nombre: 'Tasty Cotton Pants',
    total: 3536
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c60'),
    nombre: 'Incredible Granite Gloves',
    total: 20590
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c61'),
    nombre: 'Practical Metal Mouse',
    total: 6650
  },
  {
    _id: ObjectId('67d8e891675a05dae9013c62'),
    nombre: 'Handcrafted Steel Chicken',
    total: 6215
  }
]
```

- Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual
```json
db.productos.aggregate([
    {
        $project: {
            _id:0, nombre: 1, precio: 1,
            valoracion: {
                $cond: [
                    { $lt: ["$precio", 250] },
                    "barato",
                    "caro"
                ]
            }
        }
    }
])

REsultado:
[
  { nombre: 'Fantastic Wooden Fish', precio: 291, valoracion: 'caro' },
  { nombre: 'Rustic Concrete Pants', precio: 279, valoracion: 'caro' },
  { nombre: 'Small Soft Fish', precio: 189, valoracion: 'barato' },
  { nombre: 'Practical Soft Pants', precio: 67, valoracion: 'barato' },
  { nombre: 'Ergonomic Metal Ball', precio: 246, valoracion: 'barato' },
  { nombre: 'Small Steel Gloves', precio: 37, valoracion: 'barato' },
  {
    nombre: 'Ergonomic Wooden Shirt',
    precio: 238,
    valoracion: 'barato'
  },
  { nombre: 'Handmade Steel Chair', precio: 337, valoracion: 'caro' },
  {
    nombre: 'Handcrafted Soft Gloves',
    precio: 282,
    valoracion: 'caro'
  },
  {
    nombre: 'Fantastic Concrete Salad',
    precio: 265,
    valoracion: 'caro'
  },
  { nombre: 'Handmade Plastic Hat', precio: 253, valoracion: 'caro' },
  { nombre: 'Refined Wooden Tuna', precio: 212, valoracion: 'barato' },
  {
    nombre: 'Refined Concrete Salad',
    precio: 129,
    valoracion: 'barato'
  },
  { nombre: 'Unbranded Soft Fish', precio: 178, valoracion: 'barato' },
  { nombre: 'Small Concrete Fish', precio: 126, valoracion: 'barato' },
  {
    nombre: 'Refined Concrete Bike',
    precio: 180,
    valoracion: 'barato'
  },
  { nombre: 'Tasty Cotton Pants', precio: 52, valoracion: 'barato' },
  {
    nombre: 'Incredible Granite Gloves',
    precio: 290,
    valoracion: 'caro'
  },
  {
    nombre: 'Practical Metal Mouse',
    precio: 190,
    valoracion: 'barato'
  },
  {
    nombre: 'Handcrafted Steel Chicken',
    precio: 113,
    valoracion: 'barato'
  }
]
```
