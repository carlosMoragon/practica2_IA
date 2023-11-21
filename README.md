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

<pre>
 <code>
 package aplicacion;

 public class Main {
     public static void main(String[] args) {
         System.out.println("Esto es una prueba");
     }
 }
 </code>
 </pre>
 
Es muy importante que no olvide la líneapackage aplicacion;Para poderlo compilar y ejecutar sin necesidad de crear ningún proyectoen un IDE (p.ej. Netbeans), modifique el fichero build.xml de la siguienteforma1:1Para entender el papel de los fichero build.xml en Java y el uso de ant se recomiendaque consulte el manual de Apache accesible a través de la siguiente dirección: https://ant.apache.org/manual/tutorial-HelloWorldWithAnt.html
CAPÍTULO 1. PRÁCTICAS 5En la zona indicada por <!– set global properties for this build –>, sedebe añadir la siguiente línea de código:<property name="main-class" value="aplicacion.Main"/>El target de dist también se debe modificar para que quede de la siguientemanera:<target name="dist" depends="build" description="generate thedistribution"><jar jarfile="${dist}/java-algorithms-implementation-${DSTAMP}.jar"basedir="${build}"><manifest><attribute name="Main-Class" value="${main-class}"/></manifest></jar></target>Luego, se añade:<target name="run_main" depends="dist"><java jar="${dist}/java-algorithms-implementation-${DSTAMP}.jar"fork="true"/></target>A continuación se comprueba que el programa funciona:ant run_mainDespués, tomando como referencia los tests, se crea un programa princi-pal que genere un camino aplicando A*.A continuación, responda a las siguientes preguntas:1. ¿Qué variable representa la lista ABIERTA?2. ¿Qué variable representa la función g?3. ¿Qué variable representa la función f ?4. ¿Qué método habría que modificar para que la heurística representarala distancia aérea entre vértices?5. ¿Realiza este método reevaluación de nudos cuando se encuentra unanueva ruta a un determinado vértice? Justifique la respuesta.1.3.2. Material a entregarDeberá ser el siguiente:1. El proyecto completo, es decir, lo que se ha bajado inicialmente deGithub más el código implementado.2. README.md en formato Markdown respondiendo a las preguntas que seplantean en el presente enunciado.

