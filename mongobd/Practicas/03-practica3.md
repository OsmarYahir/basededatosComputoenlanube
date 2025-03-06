

1. Cambiar el salario del empleado Imogene Nolan. se le asigna 8000

    db.empleados.updateOne({nombre: 'Imogene'},{$set:{salario:8000}})


    db.empleados.updateOne(
    {
        $and: [
            { nombre: "Imogene" },
            { apellidos: "Nolan" }
        ]
    },
    { $set: { salario: 8000 } }
)
|
2. Cambiar "Belgium" por "Belgica" en los empleados (debe haber dos)

    db.empleados.updateMany({pais:'Belgium'},{$set:{pais:'Belgica'}})

3. Incrementar el salario de todos los empleados de google en 1000

    db.empleados.updateMany({empresa:'Google'},{$inc:{salario:1000}})


4. Remplasar el empleado Omar Gentry por el siguiente documento :
```json
{
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
}
```

    db.empleados.replaceOne({nombre:'Omar'},{
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
})

5. Con un find comprobar que el empleado ha sido modificado

     db.empleados.find({nombre:'Omar'})
[
  {
    _id: ObjectId('67ae3f0678b7e7a78c29d880'),
    nombre: 'Omar',
    apellidos: 'Gentry',
    correo: 'sin correo',
    direccion: 'Sin calle',
    region: 'Sin region',
    pais: 'Sin pais',
    empresa: 'Sin empresa',
    ventas: 0,
    salario: 0,
    departamentos: 'Este empleado ha sido anulado'
  }

6. Borrar todos los empleados que ganan mas de 8500
   1. Nota: Deben de ser borrados 3 documentos
   
   db.empleados.deleteMany({salario:{ $gt:8500}})

7. Visualizar con una expresion regular todos los empleados con apellidos qie comiencen con "R"

    db.empleados.find({apellidos:/R/})

8. Buscar todad las regiones que contengan una "V"
   1. Hacerlo con el operador $regex y que distinga mayusculas y minusculas. Deben de salir 2

     db.empleados.find({region: {$regex:/v/i}})

9. Visualizar los apellidos de los empleados ordenados por el propio apellido

   db.empleados.find({},{apellidos:1,_id:0}).sort({apellidos:-1})

10.  Indicar el numero de mepleados que tarbajan en Google
 
    db.empleados.find({empresa:'Google'}).size()

12.  Borrar la coleccion de la base de datos 
  
    db.empleados.drop()
