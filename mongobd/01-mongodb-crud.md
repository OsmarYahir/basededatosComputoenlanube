
# Crud y consultas en MongoBD

## Crear una base de datos
solo se crea si contiene por lo menos una coleccion 

**use bd1**

## Como crear una coleccion
use bd1
db.createCollection("Empleado")

## Mostrar las colecciones
show collection

## Mostar base de datos 
show dbs

## Eliminar base de datos 
db.dropDatabase()

## Seleccion  de datos 
db.alumnos.find({})

## Insertar un documento
``` Json
db.alumnos.insertOne(
{
     nombre: 'Huitzulopoztli',
     apellido1: 'Vaca',
     edad: 35,
     ciudad: 'San Miguel de las Piedras'
 }
)
```

## Insercion de un documento mas complejo con array

```json
db.alumnos.insertOne(
 {
  nombre: 'Juan',
  apellido1: 'Sanchez',
  apellido2: 'Vazquez',
  edad: 15,
  aficiones: [
            'Estudiar', 'Femboy', 'Agua'
              ]
 }
 )
 ```

 ## Insercion de documentos mas complejos con elementos anidados 
```json
 db.alumnos.insertOne(
{
 nombre: 'Osmi',
 apellido1: 'Resendiz',
 apellido2: 'Vazquez',
 edad: 19,
 estudios: [
       'Kinder',
       'Primaria',
       'Ing en desarrolo de software multiplataforma',
        ],
 experiencia: {
              lenguaje: 'Java',
              sbd: 'SQL server',
              aniosExp: 46
              }
 }
)
```

```json
db.alumnos.insertOne(
    {
        _id: 3,
        nombre: 'Sergio',
        apellido: 'Ramos',
        equipo: 'Monterrey',
        aficiones: [ 'Dinero', 'Hombres', 'Fiesta'],
        talentos: {
            futbol: true,
            bañarse: false,
        }
    }
)
```


## Insertar multriples documentos 
```json
db.alumnos.insertMany(
 [
   {
     _id: 12,
     nombre: 'Roberto',
     apellido: 'Gomez',
     edad: '46',
     descripcion: 'Es un sonso',
 },
 {
     nombre: 'Luis',
     apellido: 'Perez',
     edad: 49,
     habilidades: [
                   'Correr', 'dormir', 'tragar'
                  ],
     dirreciones: {
                calle: 'Del bienestar',
                numero: 666
                },
     esposas: [
             {
                nombre: 'Joana',
                edad: 20,
                pension: 791,
                hijos: ['si por favor', 'osmijn']
             },
            {
                nombre: 'Isofi',
                edad: 46,
                pension: 655451.23,
                complaciente: true
            }
           ]
        }
 ]
 )
```


## Practica 

1. Bases de datos, colecciones e insert

  * Nos conectamos con mongosh al mongodb
  * Crear una base de datos denominada curso

  ```json   
  use curso
  ```
  * Verificar que no exista la base de datos
  ```json
  show dbs
  ```

  * Crear una coleccion denominada 'facturas' con el comando createCollection y comprobar que aparece la coleccion como la base de datos 


  * Practica

  ## Cargar datos
  [libros.json](./Data/libros.json)


  ## Busquedas. Condiciones Simples de Igualdad: Metodo find()
  
  1. Seleccionar todos los documentos de la coleccion libros 
  ```json 
  db.libros.find({})
  ```
  2. Mostrar todos los documentos que sean de la editorial biblio
  ```json 
   db.libros.find({editorial:'Biblio'})
   ```
   3. Mostrar todos los documentos que el precio sea 25
    ```json
    db.libros.find({precio:25})
    ```

   4. Seleccionar todos los documentos en donde el tirulo sea json para todos   
    ```json 
    db.libros.find({titulo:'JSON para todos'})
    ```

## Operadores de Comparacion

[Operadores de comparacion](https://www.mongodb.com/docs/manual/reference/operator/query/)

![alt text](/img/1.0.png)

1. Mostrar todos los documento en donde el precio sea mayor a 25 
```json
db.libros.find({
     precio:{$gte: 25
    }
    }
 )
```
2. Mostrar los documentos donde el precio sea 25
```json
db.libros.find({ precio: { $eq: 25 } } )
```
3. Mostrar los ducumentos cuya cantidad sea menor a 5
```json
db.libros.find({ cantidad: { $lt: 5 } } )
```
4. Mostar los documentos que le pertenezcan a la editorial biblio o planeta 
```json 
db.libros.find({editorial:{$in:['Biblio', 'Planeta'] }})
``
5. Mostar todos los documentos de los que cuenten 20 o 25
```json
db.libros.find({precio:{$in:[20, 25] }})
```

6. Moatrar todos los documentos que no cuesten 20 o 25
```json
db.libros.find({precio:{$nin:[20, 25] }})
```

7. Mostar el primer documento de libros que cueste 20 a 25
```json 
db.libros.findOne({ precio:{$in:[20,25]}})
```

## Operadores logicos

[Operadores logicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

![alt text](/img/image-1.png)

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
db.libros.find({ $and:[ {precio: { $gt: 25}}, {cantidad: {$lt: 15}}, {_id:{$eq: 4}}] } )


### Operadores OR

1. Mostrar todos aquellos libros que cuesten mas de 25 o cuya cantidad sea inferior a 15

json
db.libros.find( { 
  $or: [ 
    { precio: { $gt: 25 } }, 
    { cantidad: { $lt: 15 } } 
    ] 
  })


### AND y OR Combinados

1. Mostrar los libros de la editorial Biblio con precio mayor a 30 o libros de la editorial
  Planeta con precio mayor a 20

json
db.libros.find( { 
  $or: [ 
    { $and:[{editorial: 'Biblio'}, {precio: {$gt:30}}] }, 
    { $and: [{editorial: {$eq:'Planeta'}}, {precio: {$gt:20}}] } 
    ] 
  })


### Forma Simple

json
db.libros.find( 
      {
      $or: [ 
        {editorial:'Biblio',precio:{ $gt:30}},
        {editorial:{$eq:'Planeta'},precio:{$gt:20}}
      ]
    })


## Proyección de Columnas

*** Sintaxis ***

db.collection.find(filtro, columnas)

db.libros.find({},{titulo:1})

1. Seleccionar todos los documentos, mostrando el titulo y la editorial

json
db.libros.find({}, {titulo:1, editorial:1})

db.libros.find({}, {titulo:1, editorial:1, _id:0})


2. Seleccionar todos los documentos de la editorial Planeta, 
  mostrando solamente el titulo y la editorial

json
db.libros.find({editorial: 'Planeta'}, {titulo:1, editorial:1, _id:0})



## Operador Exists (Permite saber si un campo se encuentra o no en un documento)
```json
db.libros.insertOne({
  _id:10,
  titulo: 'Mongo en entornos graficos',
  editorial: 'Terra',
  precio: 125
  }
)
```
1. Mostrar todos los documentos que no contengan el campo cantidad 
```json
db.libros.find({
    cantidad:{$exists : true}
}
)
```


### Operador Type (Permite preguntar si un determinado campo corresponde con un tipo)

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)

1. Mostrar todos los documentos donde el precio sean dobles
```json
db.libros.find(
    {precio:{$type:1}}
)
```
```json
db.libros.find(
    {precio:{$type:16}}
)



db.libros.insertOne({
    _id:11,
    titulo:'IA',
    editorial:'Tierra',
    precio:125.6,
    cantidad:45
})


db.libros.insertMany([
 {
    _id: 12,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125, 
	cantidad: 20
  },
  {
    _id: 13,
    titulo: 'Python para todos',
    editorial: 2001,
    precio: 200, 
	cantidad: 30
  }]
  )
```


1. Mostar los documentos donde el precio sea de tipo entero
```json
db.libros.find( { editorial:{$type:16}} )
```
2. Selecionar las editorial en la cual sea un tipo string

```json
db.libros.find( { editorial:{$type:2}} )
db.libros.find( { editorial:{$type:'string'}} )
```
## Practica de consultas
1. Instalar los tools de mongo
[DatabaseTools](https://fastdl.mongodb.org/tools/db/mongodb-database-tools-windows-x86_64-100.11.0.zip)

2. Cargar el json empleados (Debemos de estar ubicados en la carpeta donde se encuentra el JSon empleado)

* En local:
    comando:    
        mongoimport --db curso --collection empleados --file empleados.json