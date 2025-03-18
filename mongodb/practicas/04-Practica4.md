# Arrays y documentos anidados

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “personas.json”
3. Debes poner el resultado de las consultas en cada apartado


- Buscar las personas que vivan en Colombia
```json
db.personas.find({ "direccion.pais": "Colombia" })

Resultado:
{
  _id: ObjectId('67d8e0ec6e3084d2cfefacdc'),
  nombre: 'Anni Niño',
  direccion: {
    calle: '207 Taylor Cliffs',
    ciudad: 'Buenaventura',
    pais: 'Colombia'
  },
  colores: [
    'yellow',
    'salmon',
    'yellow'
  ],
  ingresos: 7387
}
```

- Personas a las que le guste el color rosa
```json
db.personas.find({ colores: "pink" })


Resultado:
{
  _id: ObjectId('67d8e0ec6e3084d2cfefacd4'),
  nombre: 'Laura Oquendo',
  direccion: {
    calle: '2800 Samir Trail',
    ciudad: 'Bijelo Polje',
    pais: 'Montenegro'
  },
  colores: [
    'pink',
    'blue',
    'teal'
  ],
  ingresos: 8707
}

{
  _id: ObjectId('67d8e0ec6e3084d2cfeface1'),
  nombre: 'Lorena Saiz',
  direccion: {
    calle: '1611 Oscar Walk',
    ciudad: 'The Pas',
    pais: 'Canada'
  },
  colores: [
    'orchid',
    'teal',
    'pink'
  ],
  ingresos: 10407
}
```

- Personas cuyo tercer color preferido sea el amarillo (yellow). Recuerda que los arrays comienzan por Cero. Deben salir 3
```json
db.personas.find({ "colores.2": "yellow" })

Resultado:
[
  {
    _id: ObjectId('67d8e0ec6e3084d2cfefacd5'),
    nombre: 'Laura Salcedo',
    direccion: {
      calle: '11046 Cayla Centers',
      ciudad: 'Rosso',
      pais: 'Mauritania'
    },
    colores: [ 'indigo', 'green', 'yellow', 'salmon' ],
    ingresos: 7435
  },
  {
    _id: ObjectId('67d8e0ec6e3084d2cfefacdc'),
    nombre: 'Anni Niño',
    direccion: {
      calle: '207 Taylor Cliffs',
      ciudad: 'Buenaventura',
      pais: 'Colombia'
    },
    colores: [ 'yellow', 'salmon', 'yellow' ],
    ingresos: 7387
  },
  {
    _id: ObjectId('67d8e0ec6e3084d2cfefacdd'),
    nombre: 'Andrea Paredes',
    direccion: {
      calle: '09438 Vandervort Drives',
      ciudad: 'San Antonio',
      pais: 'Puerto Rico'
    },
    colores: [ 'green', 'indigo', 'yellow', 'salmon', 'grey', 'violet' ],
    ingresos: 6340
  }
]
```

- Personas cuyo tercer color preferido sea el amarillo (yellow) y que vivan en Mauritania (debe salir uno)
```json
db.personas.find({ "colores.2": "yellow", 
                "direccion.pais": "Mauritania" })
  
  
Resultado:
[
  {
    _id: ObjectId('67d8e0ec6e3084d2cfefacd5'),
    nombre: 'Laura Salcedo',
    direccion: {
      calle: '11046 Cayla Centers',
      ciudad: 'Rosso',
      pais: 'Mauritania'
    },
    colores: [ 'indigo', 'green', 'yellow', 'salmon' ],
    ingresos: 7435
  }
]
```

- Usando el operador $all averiguar las personas que les gusta el plata (silver) y el salmon (salmon)
```json
db.personas.find({ colores: 
                    { $all: ["silver", "salmon"] } 
                })


Resultado:
[
  {
    _id: ObjectId('67d8e0ec6e3084d2cfefacd9'),
    nombre: 'Ángel Anguiano',
    direccion: {
      calle: '20780 McLaughlin Union',
      ciudad: 'Paramaribo',
      pais: 'Suriname'
    },
    colores: [ 'plum', 'silver', 'olive', 'salmon', 'silver' ],
    ingresos: 5712
  }
]
```

- Con el operador $elemMatch, averigua las personas que les guste el rosa (Pink) o el rojo (red)
```json
db.personas.find({ 
    colores: { 
        $elemMatch: { $in: ["pink", "red"] } 
        } 
    })


Resultado:
[
  {
    _id: ObjectId('67d8e0ec6e3084d2cfefacd4'),
    nombre: 'Laura Oquendo',
    direccion: {
      calle: '2800 Samir Trail',
      ciudad: 'Bijelo Polje',
      pais: 'Montenegro'
    },
    colores: [ 'pink', 'blue', 'teal' ],
    ingresos: 8707
  },
  {
    _id: ObjectId('67d8e0ec6e3084d2cfefacd8'),
    nombre: 'Sergi Cordero',
    direccion: {
      calle: '0605 Delfina Lodge',
      ciudad: 'Obiozara',
      pais: 'Nigeria'
    },
    colores: [ 'indigo', 'sky blue', 'red' ],
    ingresos: 7134
  },
  {
    _id: ObjectId('67d8e0ec6e3084d2cfeface1'),
    nombre: 'Lorena Saiz',
    direccion: { calle: '1611 Oscar Walk', ciudad: 'The Pas', pais: 'Canada' },
    colores: [ 'orchid', 'teal', 'pink' ],
    ingresos: 10407
  },
  {
    _id: ObjectId('67d8e0ec6e3084d2cfeface2'),
    nombre: 'Manuel Campos',
    direccion: {
      calle: '291 Waelchi Crest',
      ciudad: 'Holetown',
      pais: 'Barbados'
    },
    colores: [ 'maroon', 'red', 'cyan', 'blue', 'white', 'gold' ],
    ingresos: 11230
  },
  {
    _id: ObjectId('67d8e0ec6e3084d2cfeface3'),
    nombre: 'Salvador Meraz',
    direccion: {
      calle: '9327 Satterfield Alley',
      ciudad: 'Tuntange',
      pais: 'Luxembourg'
    },
    colores: [ 'red', 'yellow', 'orange', 'violet' ],
    ingresos: 4058
  },
  {
    _id: ObjectId('67d8e0ec6e3084d2cfeface6'),
    nombre: 'Ángel Zaragoza',
    direccion: {
      calle: "2758 O'Connell Ridge",
      ciudad: 'Barclayville',
      pais: 'Liberia'
    },
    colores: [ 'orange', 'maroon', 'white', 'tan', 'red', 'maroon' ],
    ingresos: 9869
  }
]
```
