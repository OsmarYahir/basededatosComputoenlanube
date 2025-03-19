
# Busquedas en Arrays y Documentos Anidados 

## Buscar dentro de un documento anidado

```json
db.ciudades.find({
... "alcalde.nombre":"Crystal"
... }
... )
```

1. Seleccinar las ciudades en donde los alcaldes tengan mas de 70 años 

```json
db.ciudades.find( { "alcalde.edad": { $gt: 70 } })
```


2. Realizar la consulta anterior pero utilizando una proyeccion 

```json
db.ciudades.find( { "alcalde.edad": { $gt: 70 }},{_id:0,nombre:1,"alcalde.edad":1})
```
3. Seleccionar la ciudades donde el alcalde tenga 76 años o 41
```json
db.ciudades.find(  {
           $or: [
            { "alcalde.edad": 76 },
            { "alcalde.edad": 41 }
        ]})
```
## Consultas con Arrays

## Ejmplos con consultas con Arrays

1. Buscar los documentos que tengan usa como pais 
```json
 db.Cazas.find({ paises:"usa"})
```
2. Buscar los documentos donde cualquier elemento del array dimenciones sea mayor a 5
```json
 db.Cazas.find({ dimensiones:{$gte:5}})
```
3. Buscar los documentos donde las dimendiones del avion por ancho que es la posicion 1, sea superior a 15
   ```json
   db.Cazas.find({ "dimensiones.1":{$gt:14} })
    ```
```json
db.Cazas.find({ "dimensiones.1":{$gt:14} },{_id:0,modelo:1,dimensiones:1})
```


4. Buscar los aviones que estar sirviendo en egipto y en israel
```json
db.Cazas.find({paises:{$all:[ 'egipto','israel']}})
```
```json
db.Cazas.find({paises:{$all:[ 'egipto','israel']}},{_id:0,modelo:1,paises:1})
```
```json
db.Cazas.find({$and:[
    {paises:'egipto'},
    {paises:'israel'}
]})
```

5. Seleccionar las cazas que sirven en inglaterra y españa
```json
db.Cazas.find({$and:[
    {paises:'inglaterra'},
    {paises:'españa'}
]})
```
```json
db.Cazas.find({paises:{$all:[ 'inglaterra','españa']}})
```
## Arrays operador $elemMatch -> Permite hacer busquedas dentro de arrays, esta busca la lista de condiciones que estan dentro del operador 

```json
db.Cazas.find({paises:{$elemMatch:{$eq:'inglaterra'}}})
```

2. Buscar los aviones en donde las dimenciones sean menores a 20 o mayores a 15
```json
db.Cazas.find({dimensiones:{$elemMatch:{$lt:20,$gt:15}}})
```
### Buscar arrays en elementos anidados


1. Utilizar la collecion ciudades
2. Buscar los consejeros de nombre Jeri Flower de edad 78
```json
json
db.ciudades.find({
  consejeros:{identificador:1, nombre:'Jeri Flowers', edad:78}
})
```
3. Buscar en consejeros donde tengan una edad mayor a 70
```json
db.ciudades.find({"consejeros.edad":{$gt:70}})
```

4. Buscar en consejeros en la posicion 2 que esten entre 70 y 80 años 
```json
db.ciudades.find({
    "consejeros.2.edad":{$gt:70, $lt:80}
},{_id:0,"consejeros.nombre":1, "consejeros.edad":1})
```
5. Buscar los consejoros de la posicion 0 que sean mayores a 70 
```json
db.ciudades.find({
    "consejeros.0.edad":{$gt:70}
},{_id:0,"consejeros.nombre":1, "consejeros.edad":1})
```
6. Buscar los consejeros que sean mayores a 70 
```json
db.ciudades.find({"consejeros.edad":{$gt:70}})
```

disting