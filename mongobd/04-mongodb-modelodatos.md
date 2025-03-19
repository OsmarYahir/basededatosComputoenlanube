
# Modelo de datos en mongoDB

- Modelos
  - Embebidos
  - Referenciados

- Modelos embebidos 

**Uno a Uno**
```json
db.departamentos.insertMany(
    [
        { 
        _id:1,
        nombre:"Tecnologias de Informacion",
        responsable:{
            nombre:"Monico",
            apellidos:"Perez Sanchez"
        },
        descripcion:'ejemplo de departamento'
        },
        {
        _id:2,
        nombre:"Contabilidad",
        responsable:{
            nombre:"Carlos",
            apellidos:"Tapia Sanchez"
        },
        descripcion:'ejemplo de departamento'
        }
    ]
)
```

- Modelos Refenrenciados

**Uno a uno**
 ``` json
db.local.insertMany(
[
    {
        _id:'BA',
        ciudad:'Buenos Aires',
        pais: 'Argentina',
        poblacion: '16 millones',
        turismo:["edificios","tango","gastronomia","museos","parques"],
        direccion:"Avenida Pololos",
        cod_departamentos:1
    },
    {
        _id:'SA',
        ciudad:'Santiago',
        pais: 'Chile',
        poblacion: '20 millones',
        turismo:["iglesias","vino","gastronimia","museos","parques"],
        direccion:"Avenida Chichis",
        cod_departamentos:2
    }
])
 ```


 db.departamentos.aggregate(
    [
        {
            $lookup:{
                from:"local",
                localField:"_id",
                foreignField:"cod_departamentos",
                as:"local"
            }
        }
    ]
 )



db.categoria.insertMany(
    [
        {
            _id:1,
            nombre:"Hogar"
        },
        {
            _id:2,
            nombre:"Dulces"
        }
    ]
)


db.producto.insertMany(
    [
        {
            _id:"CO",
            nombre:"Mesa",
            precio: 500,
            existencia: 10,
            cod_categoria:1
        },
         {
            _id:"SO",
            nombre:"Chupa chups",
            precio: 20,
            existencia: 2,
            cod_categoria:2
        }

    ]
)



 db.producto.aggregate(
    [
        {
            $lookup:{
                from:"producto",
                localField:"_id",
                foreignField:"cod_categoria",
                as:"producto"
            }
        }
    ]
 )