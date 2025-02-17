
# Practica 1: Bases de datos colecciones y inserts

1. Conectarnos con mongosh a MongoDb
1. Crar una base de datos llamada curso 
1. Comprobar que la base de datos no existe 
1. Crear una coleccion que se llame facturas y mostrarla 
```json
db.createCollection("facturas")
```json
 show collections
 ```
 1. Insertar un  documentos  con los siguientes datos 

 | Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 10 |
| Ciente | Frutas Ramirez |
| Total | 223 |

```json
db.facturas.insertOne(
... {
... cod_facturas: 10,
... cliente: 'Frutas Ramirez',
... total: 145
... }
... )
```

 | Codigo   | Valor   |
|-------------|-------------|
| Cod_Factura | 20 |
| Ciente | Fereteria Juan |
| Total | 140 |

```json
db.facturas.insertOne(
 {
 cod_facturas: 20,
 cliente: 'Fereteria Juan',
 total: 140
 }
 )
```

1. Crear una nueva coleccion pero usando directamente el insertOne.
    insertar un documento en la coleccion productos con los siguientes 
    datos> 
    | Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 1 |
| Nombre | Tornillo x 1 |
| Precio | 2 |

```json
db.productos.insertOne(
 {
 cod_producto: 20,
 nombre: 'Tornillo x 2',
 precio: 2 
 }
 )
```

1. Crear un nuevo documento que tenga un array con los siguientes datos:

| Codigo   | Valor   |
|-------------|-------------|
| Cod_Producto | 2 |
| Nombre | Martillo |
| Precio | 20 |
| Unidades | 50 |
| Fabricantes | fab1, fab2, fab3, fab4 | 

```json
db.productos.insertOne(
 {
 cod_producto: 2,
 nombre: 'Martillo',
 precio: 20, 
 unidades: 50,
 fabricantes: ['fab1', 'fab2', 'fab3', 'fab4']
 }
 )
```
1. Borrar la coleccion Facturas y comprobar que se borro

```json 
db.facturas.drop()
show 

9. Insertar un documento en una coleccion denominada **fabricantes**
    para comprobar los subducumentos y la clave _id personalizada 


| Codigo   | Valor   |
|-------------|-------------|
| id | 1 |
| Nombre | fab1 |
| Localidad | ciudad: buenos aires, pais: Argentina, calle: pez 27, cod_postal:2900  |

```json
db.fabricantes.insertOne(
 {
 _id: 1,
 nombre: 'Fab1',
 localidad: {
    ciudad: 'Buenos Aires', pais: 'Argentina', calle: 'Pez 27', cod_postal:2900
 } 
 }
 )
 ```

 10. Realizar una insercion de varios documentos en la coleccion productos 


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
| Nombre | Arrandela |
| Precio | 1 |
| Unidades | 1500 |
| Fabricantes | fab1, fab3, fab6 |


db.fabricantes.insertMany(
 [
   {
     
 cod_producto: 3,
 nombre: 'Alicates',
 precio: 10, 
 unidades: 25,
 fabricantes: ['fab1', 'fab2', 'fab5']
 },
  {
 cod_producto: 4,
 nombre: 'Arrandela',
 precio: 1, 
 unidades: 1500,
 fabricantes: ['fab1', 'fab3', 'fab6']
 }
 ]
)


# Practica1


## Cargar Datos
[lirbros.json](./data/libros.json)

## Búsquedas. Condiciones Simples de Igualdad. Método find()

1. Seleccionar todos los documentos de la colección "libros"

Json
db.libros.find({})


2. Mostrar todos los documentos que sean de la editorial "Biblio"

Json
db.libros.find({editorial:'Biblio'})


3. Mostrar todos los documentos que el precio sea 25

Json
db.libros.find({precio:25})


4. Seleccionar todos los documentos donde el titulo sea JSON para todos 

Json
db.libros.find({titulo:'JSON para todos'})


## Operadores de Compración

[Operadores de Comparación](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores de Comparación](image.png)


1. Mostrar todos los documentos donde el precio sea mayor a 25

Json
db.libros.find({
       precio: {$gt:25}
    }
)


2. Mostrar los documentos donde el precio sea 25

json
db.libros.find({ precio: { $eq: 25 } } )


3. Mostrar los documentos cuya cantidad sea menor a 5 

json
db.libros.find({ cantidad: { $lt: 5} } )


4. Mostar los documentos que pertenezcan a la editorial Biblio o Planeta

json
db.libros.find({ editorial: { $in: ['Biblio', 'Planeta']} } )


5. Mostrar todos los documentos de libros que cuesten 20 o 25

json
db.libros.find({ precio: { $in: [20, 25]} } )


6. Mostrar todos los documentos de libros que no cuesten 20 o 25

json
db.libros.find({ precio: { $nin: [20, 25]} } )


7. Mostrar el primer documentos de libros que cueste 20 o 25

json
db.libros.findOne({ precio: { $in: [20, 25]} } )


## Operadores Lógicos

[Operadores de Lógicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

![Operadores de Lógicos](image-1.png)

### Operador AND

Dos posibles opciones de AND

1. La simple, mediante condiciones separadas por comas

*Sintaxis*

json
db.collection.find({ condicion1, condicion2 } ) -> Con esto asume que es una and


2. Usando el operador $and

*Sintaxis*

json
db.collection.find({$and[{condicion1}, {condicion2}]} ) -> Con esto asume que es una and



#### Ejercicios

1. Mostrar todos aquellos documentos que cuesten más de 25 y cuya cantidad sea inferior a 15

*Forma Simple*

json
db.libros.find({ precio: { $gte: 25}, cantidad: {$lt: 15} } )


*Operador AND*

json
db.libros.find({ $and:[ {precio: { $gte: 25}}, {cantidad: {$lt: 15}}] } )


2. Mostrar todos aquellos libros que cuesten más de 25 y cuya 
  cantidad sea inferior a 15 y id igual a 4

*Forma Simple*

json
db.libros.find({ precio: { $gte: 25}, cantidad: {$lt: 15}, _id:4 } )


*Operador AND*

json
db.libros.find({ $and:[ {precio: { $gt: 25}}, {cantidad: {$lt: 15}}, {_id:{$eq: 4}}] } )





#### Expresion OR 


**Sintaxi**

{ $or: [ { <expression1> }, { <expression2> }, ... , { <expressionN> } ] }

#### Mostar  todos aquellos libros que cuesten mas de 25 o cuya cantidad sea inferior a 15

```json
db.libros.find( { 
  $or: [ { precio: { $gt: 25 } }, 
  { cantidad: { $lt: 15 } } ] 
  } )
```


#### AND y OR commbinados 

1. Mostrar los libros de la editorial biblio con precio mayor a 40 o libros de editorial Planeta 
    con precio mayor  a 10
```json
  db.libros.find( {
    $or: [ 
      { $and: [{editorial:'Biblio'},{precio:{ $gt:30}}]},
      { $and: [{editorial:{$eq:'Planeta'}},{precio:{$gt:20}}]}
    ]
  }
  )
  ```


    db.libros.find( 
      {
      $or: [ 
        {editorial:'Biblio',precio:{ $gt:30}},
        {editorial:{$eq:'Planeta'},precio:{$gt:20}}
      ]
    }
    )


   db.libros.find( 
    {
      editorial:'Biblio'},{precio:{ $gt:30}}, editorial:{$eq:'Planeta'},{precio:{$gt:20}
    }
  )


  ## Proyeccion de Columnas 
  *** Sintaxis ***

  db.coleccion.find(filtro, columnas)

  db.libros.find({},{tirulo:1})

  1. Seleccionar todos los documentos, mostrando el titulo y la editorial

```json
  db.libros.find({},{titulo:1,editorial:1, _id:0}) 
```

2. Seleccionar todos los documentos de la editorial planera, mostrando solamente el titulo y la editorial 

