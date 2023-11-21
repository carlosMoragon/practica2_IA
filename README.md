Práctica 2: Búsqueda sin Adversarios
==========

## Realizada por Carlos Moragón Corella

### Código base: <a href="http://github.com/phishman3579/java-algorithms-implementation"> Justin Wetherell </a>

### Enunciado:

El propósito de esta práctica es que el alumno reutilice código que implementa el algoritmo A*. Se bajará código de Github, y lo utilizará para ejecutar este algoritmo para un grafo concreto.

#### Pasos a realizar
En primer lugar, debe bajarse de Github el código de algoritmos y estructurasde datos en Java de Justin Wetherell:
> git clone https://github.com/phishman3579/java-\algorithms-implementation.git

A continuación, examine el código de la clase AStar.java, más concreta-mente, el método aStar. Luego, examine los tests del A* que hay en:
> java-algorithms-implementation/test/com/jwetherell/algorithms/graph/test/Graphs.java

Después, de forma gradual, desarrolle la clase principal donde estará elprograma de la práctica. Comience con un código sencillo:
```java
 package aplicacion;

 public class Main {
     public static void main(String[] args) {
         System.out.println("Esto es una prueba");
     }
 }
```

 
Es muy importante que no olvide la línea
> package aplicacion;

Para poderlo compilar y ejecutar sin necesidad de crear ningún proyectoen un IDE (p.ej. Netbeans), modifique el fichero 
<span title="Para entender el papel de los fichero build.xml en Java y el uso de ant se recomiendaque consulte el manual de Apache accesible a través de la siguiente dirección"><a href="https://ant.apache.org/manual/tutorial-HelloWorldWithAnt.html">build.xml</a></span>
 de la siguiente forma.

En la zona indicada por 
> <!– set global properties for this build –>

, sedebe añadir la siguiente línea de código:

```xml
<property name="main-class" value="aplicacion.Main"/>
```
El target de dist también se debe modificar para que quede de la siguiente manera:
```xml
<target name="dist" depends="build" description="generate the distribution">
   <jar jarfile="${dist}/java-algorithms-implementation-${DSTAMP}.jar"basedir="${build}">
       <manifest>
           <attribute name="Main-Class" value="${main-class}"/>
       </manifest>
   </jar>
</target>
```
Luego, se añade:
```xml
<target name="run_main" depends="dist">
   <java jar="${dist}/java-algorithms-implementation-${DSTAMP}.jar"fork="true"/>
</target>
```
A continuación se comprueba que el programa funciona:
> ant run_main

Después, tomando como referencia los tests, se crea un programa principal que genere un camino aplicando A*.

A continuación, responda a las siguientes preguntas:
1. ¿Qué variable representa la lista ABIERTA?
2. ¿Qué variable representa la función g?
3. ¿Qué variable representa la función f ?
4. ¿Qué método habría que modificar para que la heurística representarala distancia aérea entre vértices?
5. ¿Realiza este método reevaluación de nudos cuando se encuentra una nueva ruta a un determinado vértice? Justifique la respuesta.

#### Material a entregar

Deberá ser el siguiente:
1. El proyecto completo, es decir, lo que se ha bajado inicialmente de Github más el código implementado.
2. README.md en formato Markdown respondiendo a las preguntas que seplantean en el presente enunciado.

### Resolución de la práctica:

#### Código implementado:

Se ha implementado una llamada al algoritmo **aStar** de la clase AStar, desde el método **main** de la clase Main.

```java
public static void main(String[] args) {
    AStar<Integer> aStar = new AStar<Integer>();
    UndirectedGraph undirectedGraph = new UndirectedGraph();
    List<Graph.Edge<Integer>> path = aStar.aStar(undirectedGraph.graph, undirectedGraph.v1, undirectedGraph.v5);
    System.out.println(path);
}
```

Para implementar dicha llamada hemos copiado la clase UndirectedGraph, la cual **implementa un grafo sin dirección**.

```java
// Undirected
private static class UndirectedGraph {
    final List<Vertex<Integer>> verticies = new ArrayList<Vertex<Integer>>();
    final Graph.Vertex<Integer> v1 = new Graph.Vertex<Integer>(1);
    final Graph.Vertex<Integer> v2 = new Graph.Vertex<Integer>(2);
    final Graph.Vertex<Integer> v3 = new Graph.Vertex<Integer>(3);
    final Graph.Vertex<Integer> v4 = new Graph.Vertex<Integer>(4);
    final Graph.Vertex<Integer> v5 = new Graph.Vertex<Integer>(5);
    final Graph.Vertex<Integer> v6 = new Graph.Vertex<Integer>(6);
    final Graph.Vertex<Integer> v7 = new Graph.Vertex<Integer>(7);
    final Graph.Vertex<Integer> v8 = new Graph.Vertex<Integer>(8);
    {
        verticies.add(v1);
        verticies.add(v2);
        verticies.add(v3);
        verticies.add(v4);
        verticies.add(v5);
        verticies.add(v6);
        verticies.add(v7);
        verticies.add(v8);
    }

    final List<Edge<Integer>> edges = new ArrayList<Edge<Integer>>();
    final Graph.Edge<Integer> e1_2 = new Graph.Edge<Integer>(7, v1, v2);
    final Graph.Edge<Integer> e1_3 = new Graph.Edge<Integer>(9, v1, v3);
    final Graph.Edge<Integer> e1_6 = new Graph.Edge<Integer>(14, v1, v6);
    final Graph.Edge<Integer> e2_3 = new Graph.Edge<Integer>(10, v2, v3);
    final Graph.Edge<Integer> e2_4 = new Graph.Edge<Integer>(15, v2, v4);
    final Graph.Edge<Integer> e3_4 = new Graph.Edge<Integer>(11, v3, v4);
    final Graph.Edge<Integer> e3_6 = new Graph.Edge<Integer>(2, v3, v6);
    final Graph.Edge<Integer> e5_6 = new Graph.Edge<Integer>(9, v5, v6);
    final Graph.Edge<Integer> e4_5 = new Graph.Edge<Integer>(6, v4, v5);
    final Graph.Edge<Integer> e1_7 = new Graph.Edge<Integer>(1, v1, v7);
    final Graph.Edge<Integer> e1_8 = new Graph.Edge<Integer>(1, v1, v8);
    {
        edges.add(e1_2);
        edges.add(e1_3);
        edges.add(e1_6);
        edges.add(e2_3);
        edges.add(e2_4);
        edges.add(e3_4);
        edges.add(e3_6);
        edges.add(e5_6);
        edges.add(e4_5);
        edges.add(e1_7);
        edges.add(e1_8);
    }

    final Graph<Integer> graph = new Graph<Integer>(verticies, edges);
}
```

Representación del grafo:

![imgs/WhatsApp Image 2023-11-21 at 21.30.27.jpeg](https://github.com/carlosMoragon/practica2_IA/blob/main/imgs/WhatsApp%20Image%202023-11-21%20at%2021.30.27.jpeg)

#### Ejecución del código:

Para realizar la ejecución del código se puede apoyar en un IDE o hacer uso de Framework **ant**.

Para instalar **ant** en linux puede ejecutar el comando:
> sudo apt install ant

Tras instalar ant debe ejecutar el comando:
> ant run_main

Tras su ejecución obtendrá como salida:
```txt
[[ 1(0) ] -> [ 3(0) ] = 9
, [ 3(0) ] -> [ 6(0) ] = 2
, [ 6(0) ] -> [ 5(0) ] = 9
]
```
Que es la ruta óptima.

Representación de la ruta óptima:

![imgs/WhatsApp Image 2023-11-21 at 21.30.26.jpeg](https://github.com/carlosMoragon/practica2_IA/blob/main/imgs/WhatsApp%20Image%202023-11-21%20at%2021.30.26.jpeg)

#### Preguntas a resolver:

1. ¿Qué variable representa la lista ABIERTA?
> La lista ABIERTA se representa con la variable **OperSet**. </br>
> Esta variable está en la **clase AStar**, dentro del **método aStar**.

```java
final List<Graph.Vertex<T>> openSet = new ArrayList<Graph.Vertex<T>>(size);
```
> Inicializada como:
```java
// The set of tentative nodes to be evaluated, initially containing the start node
final List<Graph.Vertex<T>> openSet = new ArrayList<Graph.Vertex<T>>(size); 
openSet.add(start);
```
</br>

2. ¿Qué variable representa la función g?
> La **función g** se representa con la variable **gScore**. </br>
> Esta variable está en la **clase AStar**, dentro del **método aStar**.

```java
final Map<Graph.Vertex<T>,Integer> gScore = new HashMap<Graph.Vertex<T>,Integer>();
```
> Inicializada como:
```java
// Cost from start along best known path.
final Map<Graph.Vertex<T>,Integer> gScore = new HashMap<Graph.Vertex<T>,Integer>();
gScore.put(start, 0);
```
</br>

3. ¿Qué variable representa la función f ?
> La **función f** se representa con la variable **fScore**. </br>
> Esta variable está en la **clase AStar**, dentro del **método aStar**.

```java
final Map<Graph.Vertex<T>,Integer> fScore = new HashMap<Graph.Vertex<T>,Integer>();
```
> Inicializada como:
```java
// Estimated total cost from start to goal through y.
final Map<Graph.Vertex<T>,Integer> fScore = new HashMap<Graph.Vertex<T>,Integer>();
for (Graph.Vertex<T> v : graph.getVertices())
     fScore.put(v, Integer.MAX_VALUE);
fScore.put(start, heuristicCostEstimate(start,goal));
```
</br>

4. ¿Qué método habría que modificar para que la heurística representara la distancia aérea entre vértices?
> El método para representar la distancia aérea entre vértices sería: **heuristicCostEstimate**. </br>
> Este método está en la **clase AStar**.

```java
/**
* Default heuristic: cost to each vertex is 1.
*/
@SuppressWarnings("unused") 
protected int heuristicCostEstimate(Graph.Vertex<T> start, Graph.Vertex<T> goal) {
     return 1;
}
```
> Llamado desde 2 sitios en el método **aStar**:
```java
// This path is the best until now. Record it!
cameFrom.put(neighbor, current);
gScore.put(neighbor, tenativeGScore);
final int estimatedFScore = gScore.get(neighbor) + heuristicCostEstimate(neighbor, goal);
fScore.put(neighbor, estimatedFScore);
```

```java
// Estimated total cost from start to goal through y.
final Map<Graph.Vertex<T>,Integer> fScore = new HashMap<Graph.Vertex<T>,Integer>();
for (Graph.Vertex<T> v : graph.getVertices())
     fScore.put(v, Integer.MAX_VALUE);
fScore.put(start, heuristicCostEstimate(start,goal));
```
</br>

5. ¿Realiza este método reevaluación de nudos cuando se encuentra una nueva ruta a un determinado vértice? Justifique la respuesta.
> El método que se usa para la reevaluación es **reconstructPath**. </br>
> Este método está en la **clase AStar**.

```java
private List<Graph.Edge<T>> reconstructPath(Map<Graph.Vertex<T>,Graph.Vertex<T>> cameFrom, Graph.Vertex<T> current) {
    final List<Graph.Edge<T>> totalPath = new ArrayList<Graph.Edge<T>>();

    while (current != null) {
        final Graph.Vertex<T> previous = current;
        current = cameFrom.get(current);
        if (current != null) {
            final Graph.Edge<T> edge = current.getEdge(previous);
            totalPath.add(edge);
        }
    }
    Collections.reverse(totalPath);
    return totalPath;
}
```
> Llamado desde el método **aStar**:
```java
while (!openSet.isEmpty()) {
    final Graph.Vertex<T> current = openSet.get(0);
    if (current.equals(goal))
        return reconstructPath(cameFrom, goal);

// CONTINUA EL CÓDIGO
```

