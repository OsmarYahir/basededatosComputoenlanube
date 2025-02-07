
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


  