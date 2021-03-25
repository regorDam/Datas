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
Definimos un costructor para inizializar "izquierda , derecha , arriba , abajo".
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

El resultado en pantalla será algo así:

![alt text](https://cdn.kintoncloud.com/assets/img/1_qbJWmQeIIv.png)

En este punto realizamos alguno cambios en la clase anterior haciéndola abstracta y haciendo virtual la función de pintar para luego sobreescribirla.

Ahora vamos a dividir ese espacio en dos con un corte horizontal o vertical al azar.
Para hacer eso crearemos una nueva clase hija de "sala" por ejemplo "binarySala".
Necesitamos definir los máximos y mínimos (minAltura, maxAltura, minAnchura, maxAnchura) de las salas, podemos tenerlos donde mejor nos convenga por ejemplo un script de config's.


```csharp
Definimos una nueva clase hija de la clase anterior.
Declaramos atributos para minAltura, maxAltura, minAnchura, maxAnchura.
Declaramos dos booleanos corteHorizontal y corteVertical que usaremos para saber si el nodo (sala) es un nodo hoja o no.
Declaramos dos variables del tipo BinarySala (salaDerecha y salaIzquierda)
Modificamos el constructor de la clase madre para inizializar el resto de variables a false y null

Creamos un nuevo método "esHoja" que retorne un booleano:
{
    return (salaDerecha is null && salaIzquierda is  null)
}

Creamos un método "separar":
{
    if random() > 0.5 && obtenerAnchura() >= 2 * minAnchura:
        separarVertical()
    else if obtenerAltura() >= 2 * minAltura:
        separarHorizontal()
}

Sobreescribimos la función de la clase madre "pintar":
{
    if esHoja:
        return super.pintar() //Si es hoja llamamos la función de la clase madre
    else:
        salaIzquierda.pintar()
        salaDerecha.pintar()
        return 
}
```
El grafo siguiente podría ser un ejemplo de BST donde podemos ver los diferentes tipos de nodos.
![alt text](https://cdn.kintoncloud.com/assets/img/BST.png)

Ahora viene la parte interesante, la función separarVertical y separarHorizontal.
```csharp
Denifnimos la función separarHorizontal:
{
    int corte = randomRange(minAltura - obtenerAltura() - minAltura)
    
    salaIzquierda = new binarySala(izquierda, derecha, arriba-corte, abajo);
    salaDerecha = new binarySala(izquierda, derecha, arriba, arriba+1-corte);
}
```

Ahora deberíamos poder obtener una imagen parecida a esta:
![alt text]()

### Paso dos: agregar bordes
Si queremos que las salas no ocupen todo el espacio tendremos que añadir bordes. Añadimos una nueva variable a nuestra clase para poder parametrizar el espacio que queremos dejar entre salas "recortarSala". En este caso usaremos siempre el mismo valor, pero podemos añadirle aleatoriedad.
```csharp
Para ello crearemos una nueva función "recortar":
{
        izquierda += recortarSala
        derecha -= recortarSala
        arriba -= recortarSala
        abajo += recortarSala

        if (salaIzquierda != null)
        {
            salaIzquierda.recortar();
        }
        if (salaDerecha != null)
        {
            salaDerecha.recortar();
        }
}
```
Si llamamos la función "recortar" antes de pintar obtendremos un resultado parecido a esto:
![alt text]()

### Paso tres: agregar conexiones
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
