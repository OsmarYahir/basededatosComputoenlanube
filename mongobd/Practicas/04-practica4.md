# Arrays y documentos anidados

1. Para hacer esta práctica vamos a cargar unos datos ficticios de empresas.
2. Tienes un fichero denominado “personas.json”
3. Debes poner el resultado de las consultas en cada apartado
- Buscar las personas que vivan en Colombia

```json
db.personas.find({"direccion.pais":"Colombia"})
```

![alt text](/img/image-1.png)

```json
db.personas.find({"direccion.pais":"Colombia"}).size()
```

![alt text](/img/image.png)

- Personas a las que le guste el color rosa
```json
db.personas.find(
    { colores: { $in: ["pink"] } }
)
```

![alt text](/img/image-2.png)

- Personas cuyo tercer color preferido sea el amarillo (yellow). Recuerda que los arrays comienzan por Cero. Deben salir 3
```json
db.personas.find(
    { "colores.2": "yellow" }
)
```
![alt text](/img/image-3.png)

- Personas cuyo tercer color preferido sea el amarillo (yellow) y que vivan en Mauritania (debe salir uno)
```json
db.personas.find(
    { 
        $and: [
            { "colores.2": "yellow" },
            { "direccion.pais": "Mauritania" }
        ]
    }
)
```

![alt text](/img/image-4.png)

- Usando el operador $all averiguar las personas que les gusta el plata (silver) y el salmon (salmon)

```json
db.personas.find(
    {
        "colores":{$all:[ 'silver','salmon']}
    }
)
```

![alt text](/img/image-5.png)


- Con el operador $elemMatch, averigua las personas que les guste el rosa (Pink) o el rojo (red)

```json
db.personas.find(
    { 
        "colores": { 
            $elemMatch:  { $in: ["Pink", "red"]  } 
        } 
    }
)
```

![alt text](/img/image-6.png)