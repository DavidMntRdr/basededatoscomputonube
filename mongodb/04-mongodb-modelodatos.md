# Modelo de datos en MongoDB

- Modelos
    - Embebidos 
    - Referenciados

- Modelos Embebidos 

**Uno a Uno**
```json
db.departamentos.insertMany(
    [{
        _id:1, 
        nombre: "Tecnologías de Información",
        responsable: {
            nombre: "Mónica",
            apellidos: "Martínez Pérez"
        },
        descripcion: "Ejemplo de departamento"
    },
    {
        _id:2, 
        nombre: "Contabilidad",
        responsable: {
            nombre: "Raúl",
            apellidos: "López Hernández"
        },
        descripcion: "Ejemplo de departamento"
    }]
)
```

- Modelos referenciados 
**Uno a Uno**
```json
db.localidades.insertMany(
    [
        {
            _id:'BA',
            ciudad:'Buenos Aires',
            pais: 'Argentina',
            poblacion: '16 millones',
            turismo: ["edificios", "tango", "gastronomia", "museos", "parques"],
            direccion: "Avenida Tortolos",
            cod_dep: 1
        },
        {
            _id:'SA',
            ciudad:'Santiago',
            pais: 'Chile',
            poblacion: '20 millones',
            turismo: ["iglesias", "vino", "gastronomia", "museos"],
            direccion: "Calle soy la vaca del corral",
            cod_dep: 2
        }
    ]
)
```

```json
db.departamentos.aggregate(
    [
        {
            $lookup: {
                from: "localidades",
                localField: "_id",
                foreignField: "cod_dep",
                as: "localidades"
            }
        }
    ]
)
```


```json
db.categoria.insertMany(
    [
        {
            _id:1,
            nombre:'Cremeria',
        },
        {
            _id:2,
            nombre:'Frituras',
        }
    ]
)
```

```json
db.producto.insertMany(
    [
        {
            _id:1,
            nombre:'Leche',
            precio: 42,
            existencia: 30,
            cod_cat: 1
        },
        {
            _id:2,
            nombre:'Sabritas',
            precio: 22,
            existencia: 15,
            cod_cat: 2
        }
    ]
)
```

```json
db.categoria.aggregate(
    [
        {
            $lookup: {
                from: "producto",
                localField: "_id",
                foreignField: "cod_cat",
                as: "productos"
            }
        }
    ]
)
```