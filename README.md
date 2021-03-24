# Creación de mapas con Binary Space Partitioning (BSP)

Antes de empezar, vamos a definir un poco este concepto. BSP es una función que sirve para dividir recursivamente un espacio. Podemos usar este algoritmo para varias finalidades, pero en este caso vamos a usarlo para generar mazmorras de forma aleatoria y procedural. Para más detalle podemos visitar la [wikipedia].

El reto de usar aleatoriedad en la creación de escenarios es que mantengan consistentes a pesar de ser aleatorios. De esta manera BSP nos permite tener ese punto de aleatoriedad y solidez. La lógica del algoritmo no es complicada a priori, se base en subdividir de forma recursiva un espacio en dos.

![alt text](https://cdn.kintoncloud.com/assets/img/1_HKJEoxPoma.png)

Luego solo tendriamos que usar el arbol (BST) para crear las conexiones de las salas.

![alt text](https://cdn.kintoncloud.com/assets/img/1_SWoGcYUaQS.png)

### Paso uno: Division

Para los ejemplos voy a usar pseudocódigo que es aplicable a cualquier lenguaje y/o para cualquier motor de videojuegos como pueden ser por ejemplo [Unity3D], [Unreal Engine] o [Godot Engine]

```csharp
Definimos una clase por ejemplo "sala".
Declaramos atributos numericos protegidos para guardar "izquierda , derecha , arriba , abajo".
Declaramos metodos para obtener los valores anteriores.
Declaramos metodos para la obtener la "anchura y altura":
Obtener anchura
    return  derecha  -  izquierda  +  1 ;

Definimos metodo para pintar/renderizar:
{
    Declaramos un objeto que usaremos de contenedor/padre de de la sala
    
    for loopcounterX = izquierda to derecha
        for loopcounterY = abajo to arriba
            Declaramos un objeto que usaremos de "tile"
            tile.position = vector(loopcounterX, loopcounterY)
            tile.parent = contenedor/padre
            loopcounter = loopcounter + 1
            
            tile.color = randomColor()
        endfor
        
        loopcounter = loopcounter + 1
    endfor    
    
    return contenedor/padre
}
```

El resultado en pantalla sera algo así:

![alt text](https://cdn.kintoncloud.com/assets/img/1_qbJWmQeIIv.png)

### Paso dos: agregar bordes

 - Write MORE Tests

### Paso dos: agregar conexiones
 - Write MORE Tests


License
----

MIT


**Free Software, Hell Yeah!**

**_Code & Love_**

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen. Thanks SO - http://stackoverflow.com/questions/4823468/store-comments-in-markdown-syntax)

   [wikipedia]: <https://es.wikipedia.org/wiki/Partici%C3%B3n_binaria_del_espacio>
   [Unity3d]: <https://unity.com/es>
   [Unreal Engine]: <https://www.unrealengine.com/>   
   [Godot Engine]: <https://godotengine.org/>      

