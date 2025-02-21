
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



# Modificando documentos

## Comandos importantes

1. UpdateOne -> Modificar un solo documento
1. UpdateMany -> Modificar multriples documentos
1. replaceOne -> Sustituir el contenido completo de un documento


``` json
db.collection.updateOne(
    {filtro},
    {operador: }
    )
```

[Operadores Update](https://www.mongodb.com/docs/manual/reference/operator/update/)

### Operador set

1. Modificar un documento
```json
db.libros.updateOne({titulo:'Python para todos'},{$set:{titulo:'Java para todos'}})
```

2. Actualizar el precion a 100 y la cantidad a 50 para el _id:10

db.libros.updateOne({_id:10},{$set:{precio:100, cantidad:50}})


3. Operador $inc y $mul 
- Actualizar con un incremento de 5 todos los elementos 
```json
  db.libros.updateMany({},{$inc:{precio:5}})
```

- Actualizar con multriplicacion de 2 a todos los documentos donde la cantidad que sean mayores a 20
```json
db.libros.updateMany({cantidad:{$gt:20}},{$mul:{cantidad:2}})
```

-- Actualiza todos los documentos donde el precio sea mayor a 20 y se multriplique por 2 la cantidad y el precio

db.libros.updateMany(
  { precio: { $gt: 20 } }, 
  {
    $mul: {
      cantidad: 2, 
      precio: 2
    }
  }
);


3. Remplazar documentos (replaceOne)

db.libros.replaceOne({_id:2}, {titulo:'Chupaelperro', autor:'Hola soy german',precio:500})


# Borrar documentos 

1. deleteOne -> Elimina un solo documento
2. deleteMany -> Elimina multriples documentos


1. Eliminar el documento con el id: 2

db.libros.deleteOne({_id:2})

2. Eliminar los documentos en donde la cantidad sea mayor o igual a 150

db.libros.deleteMany(
  { cantidad: {$gte:150}}
)


# Expreciones regulaes 

1. Buscar los libros que contengan la letra T en el tirulo

db.libros.find({titulo:/t/})

2. Buscar los libros que en titulo tengan la palabra Json

db.libros.find({titulo:/Json/})

3. Buscar todos los documentos que en el tirulo terminen en tos

db.libros.find({titulo:/tos$/})

4. Todos los documentos que en titulo cominecen con J 

db.libros.find({titulo:/J/})

db.libros.find(
  { titulo: /^J/ }
);

# Operador $regex
[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

-- Seleccionar los libros que contengan la palabra para en titulo

  db.libros.find({titulo: {$regex: 'para'}})

  db.libros.find({titulo: {$regex:'JSON'}})

    db.libros.find({titulo: {$regex:/JSON/}})

    - Distinguir entre mayusculas y minusculas 

        db.libros.find({titulo: {$regex:/json/}}) -> No distingue entre minusculas y masyusculas

            db.libros.find({titulo: {$regex:/JSON/,$options: "i"}})

             db.libros.find({titulo: {$regex:/JSON/,i}})

  - Seleccionar todos los libros que comiencen con J

    db.libros.find({titulo: {$regex:/^j/, $options: "i"}})

- Seleccionar todos los que terminen con s

 db.libros.find({titulo: {$regex:/es$/i/}})

  db.libros.find({titulo: {$regex:"es$", $options: "i"}})


  # Metodo sort (Ordenar Documentos)

  1. Ordenar los libros de manera ascendente por el precio

db.libros.find({},{titulo:1,precio:1,_id:0}).sort({precio:1})

  2. Ordenar los libros de manera desendete por le precio

db.libros.find({},{titulo:1,precio:1,_id:0}).sort({precio:-1})

  3. Ordenar los libros de manera ascendete por la editorial y de manera desendente por el precio, mostrando el titulo, el precio y la editorial 
db.libros.find({},{titulo:1,precio:1,editorial:1,_id:0}).sort({editorial:1, precio:-1})

  # Otros metodos skip, limit, size

  db.libros.find({},{titulo:1,precio:1,_id:0,editorial:1}).size()

    db.libros.find({titulo: {$regex:/java/i}}).size()

  -- Buscar todos los libros pero mostrando los dos primeros 

  db.libros.find({},{titulo:1,editorial:1, precio:1, _id:0}).limit(2)

  -- Mostrar  los 3 ultimos libros 

  db.libros.find({},{titulo:1,precio:1,editorial:1,_id:0}).sort({precio:-1}).limit(3)

  -- Seleccionar todos los libros ordenados por titulo de forma decentente saltando los 2 primeros documentos y mostrando el tamallo 

 db.libros.find({}).sort({titulo:-1}).skip(2).size()

 # Borrar colecciones y base de datos


  db.createCollection("Ejemplo")

  show collections

  db.Ejemplo.insertOne({
... nombre: "Juanito"
... }
... )
{



  db.Ejemplo.find({})


  db.Ejemplo.drop() 


   db.dropDatabase()