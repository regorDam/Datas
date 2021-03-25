# Creación de mapas con Binary Space Partitioning (BSP)

Antes de empezar, vamos a definir un poco este concepto. BSP es una función que sirve para dividir recursivamente un espacio. Podemos usar este algoritmo para varias finalidades, pero en este caso vamos a usarlo para generar mazmorras de forma aleatoria y procedural. Para más detalle podemos visitar la [wikipedia].

El reto de usar la aleatoriedad en la creación de escenarios es que los escenarios se mantengan consistentes a pesar de ser aleatorios. BSP nos permite mantener ambas características, aleatoriedad y solidez. La lógica del algoritmo no es complicada a priori, se basa en subdividir de forma recursiva un espacio en dos sub espacios.

![alt text](https://cdn.kintoncloud.com/assets/img/1_HKJEoxPoma.png)

Después, solo debemos usar el árbol (BST) para crear las conexiones de las salas.

![alt text](https://cdn.kintoncloud.com/assets/img/1_SWoGcYUaQS.png)

### Paso uno: Division

Para los ejemplos vamos a usar pseudocódigo que es aplicable a cualquier lenguaje y/o en cualquier motor de videojuegos, como por ejemplo [Unity3D], [Unreal Engine] o [Godot Engine]

```csharp
1. Definimos una clase, por ejemplo "sala".
2. Declaramos atributos numéricos protegidos para guardar "izquierda" , "derecha" , "arriba" , "abajo".
3. Declaramos métodos para obtener los valores anteriores.
3. Declaramos métodos para la obtener la "anchura" y "altura":
4. Obtenemos la anchura:
    return  derecha  -  izquierda  +  1 ;
5. Definimos un constructor para inicializar "izquierda , derecha , arriba , abajo".
6. Definimos método para pintar/renderizar:
{
    7. Declaramos un objeto que usaremos como contenedor/padre de la sala
    
    for loopcounterX = izquierda to derecha
        for loopcounterY = abajo to arriba
            8. Declaramos un objeto que usaremos de "tile"
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

//Ahora, deberemos realizar algunos cambios en la clase anterior. Primero deberemos de cambiarla a abstracta, después cambiar la función de pintar a virtual para por último sobreescribirla (mejorar o ampliar explicación)//

Dividiremos ahora ese espacio en dos con un corte horizontal, o vertical, al azar.

Para poder hacerlo, crearemos una nueva clase hija de "sala" por ejemplo "binarySala".

Necesitamos definir los máximos y mínimos (minAltura, maxAltura, minAnchura, maxAnchura) de las salas, podemos tenerlos donde mejor nos convenga por ejemplo un script de config's. 


```csharp
1. Definimos una nueva clase hija de la clase anterior.
2. Declaramos atributos para minAltura, maxAltura, minAnchura, maxAnchura.
3. Declaramos dos booleanos corteHorizontal y corteVertical que usaremos para saber si el nodo (sala) es un nodo hoja o no.
4. Declaramos dos variables del tipo BinarySala (salaDerecha y salaIzquierda)
5. Modificamos el constructor de la clase madre para inizializar el resto de variables a false y null

6. Creamos un nuevo método "esHoja" que retorne un booleano:
{
    return (salaDerecha is null && salaIzquierda is  null)
}

7. Creamos un método "separar":
{
    if random() > 0.5 && obtenerAnchura() >= 2 * minAnchura:
        separarVertical()
    else if obtenerAltura() >= 2 * minAltura:
        separarHorizontal()
}

8. Sobreescribimos la función de la clase madre "pintar":
{
    if esHoja:
        return super.pintar() //Si es hoja, llamamos la función de la clase madre
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
1. Definimos la función separarHorizontal:
{
    int corte = randomRange(minAltura - obtenerAltura() - minAltura)
    
    salaIzquierda = new binarySala(izquierda, derecha, arriba-corte, abajo);
    salaDerecha = new binarySala(izquierda, derecha, arriba, arriba+1-corte);
}
```

Deberíamos obtener una imagen parecida a la siguiente:
![alt text](https://cdn.kintoncloud.com/assets/img/1_dRghjuiNnkla.png)

### Paso dos: agregar bordes
Si queremos que las salas no ocupen todo el espacio tendremos que añadir bordes. Añadimos una nueva variable a nuestra clase para poder parametrizar el espacio que queremos dejar entre salas "recortarSala". En este caso usaremos siempre el mismo valor, así que no podremos añadirle aleatoriedad.
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
![alt text](https://cdn.kintoncloud.com/assets/img/1_pHvFjaiYbdl.png)

### Paso tres: agregar conexiones
Ahora solo nos falta añadir las conexiones que conectarán las salas. Para hacerlo vamos a conectar de forma recursiva cada nodo con su hermano.
Crearemos métodos para encontrar todas las posiciones donde podamos agregar pasillos.
También crearemos una nueva clase pasillo para poder definir algunos atributos y su método pintar.
```csharp
TODO binarySala methods
```

```csharp
TODO pasillo class
```

Al crear los pasillos el resultado del mapa final debería ser algo así:

![alt text](https://cdn.kintoncloud.com/assets/img/1_ResljGaybakg.png)

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
