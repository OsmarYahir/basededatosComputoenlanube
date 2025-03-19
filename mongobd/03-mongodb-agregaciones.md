# Agregaciones en MongoDB (Framework)

## Metodos para realizar agregaciones simples 

- distinct(): Devuelve valores no duplicados
- countDocuments(): Cuenta los documentos en una coleccion
- estimatedDocumentCount(): Cuenta de manera aproximanda durante un periodo de tiempo

## Una Aggregation pipeline consta 

1. Cada estapa realiza una operacion en los documentos de entrada. Por ejemplo, una fase puede fultar 

2. Los documentos que se generan en una fase pasan a la siguiente fase. 
3. Puede revolver resultados para grupos de documentos como totales, maximos, minimo, etc

### Se utiliza la clausula "aggregate"

- Existen una serie de operadores que se pueden utilizar para realizar operaciones. Se tienen distintos tipos de etapa, de comprobacion, booleanos, aritmeticos, de cadena, etc.

## Metodos simples: countDocuments() y distinc()

1. 

```json
 db.libros.countDocuments()
 ```

2. Contar los documentos de la editorial Terra
   ```json
     db.libros.countDocuments({editorial:{$eq:"Terra"}})

     db.libros.countDocuments({editorial:"Terra"})
      ```
3. Mostrar todos los libros mostrando solamente la editorial 
```json
     db.libros.countDocuments({},{_id:0,editorial:1})
```

4. Mostar todos los libros distintos a editorial
```json
     db.libros.distinct('editorial')
```
[Documentacion de Agregaciones](https://www.mongodb.com/docs/manual/aggregation/)

## $match. Una pipeline basica 
## tienen funciones de etapa
```json
db.libros.aggregate(
    [ 
    {
        $match:{editorial:"Terra"}
    }
    ]
)
```


## $project. Iniciar y renombrar capas 
```json
db.libros.aggregate(
    [
        {
        $match:{editorial:"Terra"}
        },
        {
            $project:{
                _id:0,
                titulo:1,
                precio:1,
                NombreEditorial:"$editorial",
                editorial:1
            }
        }
    ]
)
```

# Cargar 
```json
db.getCollection('libros').aggregate(
  [
    {
      $project: {
        _id: 0,
        precio: 1,
        cantidad: 1,
        NombreEditorial: '$editorial',
        editorial: 1
      }
    },
    {
      $project: {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        'Nombre Editorial': '$editorial',
        'Total de ganacias': {
          $multiply: ['$precio', '$cantidad']
        }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```

## Sort ordenaciones 

db.getCollection('libros').aggregate(
  [
    {
      $project: {
        _id: 0,
        precio: 1,
        cantidad: 1,
        NombreEditorial: '$editorial',
        editorial: 1
      }
    },
    {
      $project: {
        _id: 0,
        titulo: 1,
        precio: 1,
        cantidad: 1,
        'Nombre Editorial': '$editorial',
        'Total de ganacias': {
          $multiply: ['$precio', '$cantidad']
        }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);

## Crud agregaciones

[Agrupaciones](https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/)


1. Cuantos documentos hay por cada una de las editoriales
```json
db.getCollection('libros').aggregate(
  [
    {
      $group: {
        _id: '$editorial',
        'Numero Documento': { $count: {} }
      }
    }
  ],
  { maxTimeMS: 60000, allowDiskUse: true }
);
```


- Cuantos documentos hay por cada una de las editoriales por numero de documentos de manera descendente 

json
[
  {
    $group:
      {
        _id: "$editorial",
        "Numero Documento": {
          $count: {}
        }
      }
  },
  {
    $sort:
      {
        editorial: -1
      }
  }
]


- Utilizando Atlas Base de Datos sample_airbnb
- Agrupar por tipo de propiedad, mostrando el numero de propiedades y el promedio de sus precios

json
[
  {
    $group: {
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



- Operador $set

json
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
  },
  {
    $set:
      {
        Media_Total: {
          $trunc: "$Media"
        }
      }
  }
]


- Operador $unsec

json
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
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset: ["Media", "Media_Total"]
  }
]



- Quitando un campo

json
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
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset: "Media"
  }
]


- Creando nuevas colecciones utilizando el operador $out 
- Nota: debe ser el el ultimo en la agregacion

json
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
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset: "Media"
  },
  {
    $out: {
      db: "sample_airbnb",
      coll: "media_airbnb"
    }
  }
]

- Ejemplos con operadores de comparacion y logicos 

- Creando nuevas colecciones utilizando el operador $out 
- Nota: debe ser el el ultimo en la agregacion

json
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
  },
  {
    $set: {
      Media_Total: {
        $trunc: "$Media"
      }
    }
  },
  {
    $unset: "Media"
  },
  {
    $out: {
      db: "sample_airbnb",
      coll: "media_airbnb"
    }
  }
]


- Ejemplos con operadores de comparación y lógicos

json
[
  {
    $project:
      {
        _id: 0,
        name: 1,
        price: 1,
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
        baratito: {
          $lt: ["$price", 100]
        }
      }
  }
]



- $cond - Devuelve valores segun una condicion (parecido a un operador ternario de un lenguaje de programación )


json
[
  {
    $project: {
      _id: 0,
      name: 1,
      price: 1,
      room_type: 1,
      caro: {
        $cond: [
          {
            $gt: ["$price", 300]
          },
          "Si",
          "No"
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
      baratito: {
        $cond: [
          {
            $lt: ["$price", 100]
          },
          "Si",
          "No"
        ]
      }
    }
  }
]



- Views

json
db.createView("ganancias_libros", "libros",
[
  {
    $match: {
      editorial: "Biblio"
    }
  },
  {
    $project: {
      _id: 0,
      titulo: 1,
      precio: 1,
      cantidad: 1,
      "Nombre editorial": "$editorial",
      "Total Ganancias": {
        $multiply: ["$precio", "$cantidad"]
      }
    }
  },
  {
    $sort: {
      "Total Ganacias": -1
    }
  }
]
)


db.ganancias_Libros.find({'Total ganacias':{$lte:240}}, {titulo:1, 'Total ganancias':1}).sort({titulo:-1})