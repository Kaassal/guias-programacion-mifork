# Tema 4.1. Composición

## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En lenguajes estructurados como C, la composición se logra anidando tipos de datos compuestos. Un `struct` puede contener como miembros (campos) a otros `struct` previamente definidos. Esto establece una relación jerárquica de tipo "tiene-un" (*has-a*), permitiendo modelar entidades complejas a partir de bloques más simples. 

Al carecer de orientación a objetos, el comportamiento (las funciones) está separado de los datos. Para operar sobre estas estructuras compuestas, es necesario definir funciones independientes que reciban las estructuras como parámetros (ya sea por valor o por referencia mediante punteros) para acceder a sus miembros internos y realizar los cálculos correspondientes.

```c
#include <stdio.h>
#include <math.h>

// Definición de las estructuras compuestas
struct Punto {
    double x;
    double y;
};

struct Linea {
    struct Punto p1;
    struct Punto p2;
};

// Funciones que operan sobre las estructuras
double calcularDistancia(struct Punto a, struct Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

double calcularLongitudLinea(struct Linea l) {
    // Reutiliza la función base pasando los componentes internos
    return calcularDistancia(l.p1, l.p2);
}

```

## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.

Al transicionar a la orientación a objetos en Java, la composición se implementa declarando referencias a objetos como atributos dentro de otra clase. A diferencia de C, las funciones se convierten en métodos encapsulados dentro de las propias clases, lo que permite que los objetos operen sobre su propio estado interno.

Para garantizar la inmutabilidad solicitada, se utiliza el modificador de acceso `private` junto con la palabra reservada `final`. Esto asegura que las referencias y los valores primitivos solo puedan asignarse una vez durante la construcción del objeto, protegiendo la integridad estructural de la línea y sus puntos a lo largo de todo su ciclo de vida, sin necesidad de omitir la funcionalidad principal.

```java
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistancia(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    // Composición: La clase Linea "tiene-dos" Puntos
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        if (p1 == null || p2 == null) {
            throw new IllegalArgumentException("Los puntos no pueden ser nulos");
        }
        this.p1 = p1;
        this.p2 = p2;
    }

    public double calcularLongitud() {
        // Se delega el cálculo al método de la clase Punto
        return p1.calcularDistancia(p2);
    }
}

```

## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La multiplicidad en el diseño orientado a objetos define cuántas instancias de una clase pueden o deben estar asociadas con una única instancia de otra clase. Es una restricción estructural que delimita las relaciones, especificando si son uno-a-uno, uno-a-muchos, muchos-a-muchos o números exactos y obligatorios.

En el ejemplo proporcionado, la multiplicidad dirigida desde `Linea` hacia `Punto` es exactamente **2** (una instancia de `Linea` contiene obligatoriamente dos instancias de `Punto`). Por el contrario, la multiplicidad desde `Punto` hacia `Linea` es de **0 a muchos (0..*)**, ya que un objeto `Punto`, en este diseño unidireccional, no tiene conocimiento interno de las líneas a las que pertenece, y conceptualmente podría formar parte de ninguna, de una, o de múltiples líneas simultáneamente.

## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La distinción entre composición fuerte y débil radica en la independencia y la gestión del ciclo de vida de los objetos involucrados. En una relación **fuerte**, el objeto contenido no tiene sentido lógico ni existencia funcional fuera del objeto contenedor; su ciclo de vida está estrictamente subordinado. Si el contenedor se destruye, las partes también deben destruirse. En una relación **débil**, los objetos contenidos pueden existir de forma independiente y ser compartidos por varios contenedores.

En la terminología estándar (como UML), a la relación fuerte se le denomina estrictamente **"Composición"** (representada por un rombo relleno). A la relación débil se le denomina **"Agregación"** o **"Asociación"** (representada por un rombo vacío o una línea simple), indicando que el contenedor agrupa elementos que tienen un ciclo de vida autónomo e independiente de la existencia del contenedor.

## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

En esos casos, se habla de una relación de **dependencia** (conocida coloquialmente como relación "usa-un" o *uses-a*). Ocurre cuando una clase requiere de otra para llevar a cabo una operación específica, pero no la retiene como parte de su estado estructural a largo plazo.

La diferencia fundamental con la composición (o agregación) es la persistencia de la relación. En la composición, la referencia al objeto colaborativo se almacena en los atributos de la clase, manteniendo el vínculo durante la vida de la instancia. En la dependencia, el vínculo es transitorio y efímero; nace y muere dentro de la ejecución de un método concreto o el paso de un parámetro.

## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

Para implementar una composición **fuerte**, el contenedor (`Linea`) debe ser el responsable exclusivo de instanciar a sus partes (`Punto`) internamente, ocultando su existencia al exterior y evitando que las referencias originales sean compartidas.

Para una composición **débil** (agregación), el contenedor recibe las referencias de los objetos (`Punto`) desde el exterior a través de su constructor o métodos *setter* (inyección de dependencias), lo que implica que los puntos fueron creados previamente y sobrevivirán si la línea deja de utilizarse.

```java
// Variante 1: Composición Fuerte (La línea crea y posee los puntos)
class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    public LineaFuerte(double x1, double y1, double x2, double y2) {
        // Los puntos nacen con la línea, no existen fuera de ella
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }
}

// Variante 2: Composición Débil / Agregación (La línea recibe los puntos)
class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    public LineaDebil(Punto p1, Punto p2) {
        // Los puntos son provistos desde fuera, su ciclo de vida es independiente
        this.p1 = p1;
        this.p2 = p2;
    }
}

```

## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?

A diferencia de lenguajes como C o C++ donde la memoria dinámica debe ser liberada manualmente (mediante `free` o `delete` en los destructores), en Java el programador no interviene directamente en la destrucción de los objetos en memoria.

En Java, esto está gestionado automáticamente por el **Recolector de Basura** (*Garbage Collector* o GC). En una composición fuerte, las únicas referencias hacia las instancias de `Punto` residen exclusivamente dentro de la instancia de `Linea`. Cuando el objeto `Linea` deja de ser referenciado por el resto del programa, se vuelve inalcanzable. Consecuentemente, sus atributos internos (`p1` y `p2`) también pierden su única ruta de acceso, permitiendo que el GC libere la memoria de los tres objetos de manera conjunta, simulando perfectamente la destrucción en cascada sin escribir código explícito.

## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.

```java
class Profesor {
    private final String nombre;
    public Profesor(String nombre) { this.nombre = nombre; }
    public String getNombre() { return nombre; }
}

class Departamento {
    private Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento requiere un director.");
        }
        this.profesores = new Profesor[50];
        this.numProfesores = 0;
        this.director = directorInicial;
        addProfesor(directorInicial); // Garantiza que el director está en la lista
    }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor no válido.");
        if (numProfesores >= 50) throw new IllegalStateException("Departamento lleno.");
        profesores[numProfesores] = p;
        numProfesores++;
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        if (profesores[pos] == director) {
            throw new IllegalStateException("No se puede eliminar al director actual.");
        }
        // Desplazar elementos para no dejar huecos en el array primitivo
        for (int i = pos; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }
        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public void cambiarDirector(int posProfesor) {
        if (posProfesor < 0 || posProfesor >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición inválida.");
        }
        this.director = profesores[posProfesor];
    }

    public int getCantidadProfesores() { return numProfesores; }
    
    public Profesor getProfesor(int pos) {
        if (pos < 0 || pos >= numProfesores) throw new IndexOutOfBoundsException("Posición inválida");
        return profesores[pos];
    }
    
    public Profesor getDirector() { return director; }
}

```

## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Al sustituir el array primitivo por una colección como `ArrayList`, se delega la gestión de memoria y el redimensionamiento a la API nativa de Java. El código se simplifica drásticamente: se elimina la necesidad de mantener un contador manual de elementos (`numProfesores`), se omite la lógica manual de desplazamiento de elementos al eliminar (el bucle `for` desaparece por completo), y no es necesario imponer un límite estricto de 50 posiciones iniciales de forma manual.

Si existiera un método que devolviese la lista completa y se retornase la referencia directa (`return this.profesoresList`), se generaría una fuga de encapsulamiento crítica. El código llamador externo podría usar métodos como `.clear()` o `.remove()` sobre esa lista, alterando el estado interno del departamento, violando las invariantes (por ejemplo, eliminando al director) sin pasar por las validaciones de la clase.

Para resolver este problema, la clase debe devolver una "copia defensiva" o, preferiblemente, una vista inmutable de la lista original. Esto se logra retornando `new ArrayList<>(this.profesoresList)` o utilizando utilidades del lenguaje como `Collections.unmodifiableList()`.

```java
import java.util.ArrayList;
import java.util.List;
import java.util.Collections;

class DepartamentoConLista {
    private List<Profesor> profesoresList;
    private Profesor director;

    public DepartamentoConLista(Profesor directorInicial) {
        if (directorInicial == null) throw new IllegalArgumentException("Requiere director.");
        this.profesoresList = new ArrayList<>();
        this.director = directorInicial;
        this.profesoresList.add(directorInicial);
    }

    public void addProfesor(Profesor p) {
        if (p == null) throw new IllegalArgumentException("Profesor nulo.");
        profesoresList.add(p);
    }

    public void removeProfesor(int pos) {
        if (pos < 0 || pos >= profesoresList.size()) throw new IndexOutOfBoundsException();
        if (profesoresList.get(pos) == director) {
            throw new IllegalStateException("No se puede eliminar al director.");
        }
        profesoresList.remove(pos); // El desplazamiento es automático
    }

    // Resolución al problema de devolver la estructura completa
    public List<Profesor> getTodosLosProfesores() {
        // Devuelve una vista de solo lectura para proteger la encapsulación
        return Collections.unmodifiableList(profesoresList); 
    }
}

```

## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.

La composición recursiva ocurre cuando una clase define atributos cuyo tipo de dato es la propia clase que se está definiendo. Esto permite modelar estructuras jerárquicas o en cadena de profundidad arbitraria. Para asegurar la inmutabilidad, el atributo recursivo debe ser definido como `final` y asignado en el momento de la construcción.

Otros ejemplos clásicos de estructuras que utilizan este patrón de composición recursiva son las listas enlazadas (donde un `Nodo` contiene una referencia al siguiente `Nodo`), las estructuras de datos en forma de árbol (un `NodoArbol` contiene una lista de hijos de tipo `NodoArbol`), y los sistemas de archivos (donde un `Directorio` puede contener subdirectorios que son objetos de la misma clase `Directorio`).

```java
class Persona {
    private final String nombre;
    private final Persona madre; // Composición recursiva

    // Constructor base (persona sin madre registrada)
    public Persona(String nombre) {
        this.nombre = nombre;
        this.madre = null;
    }

    // Constructor que enlaza con otra Persona
    public Persona(String nombre, Persona madre) {
        this.nombre = nombre;
        this.madre = madre;
    }

    public String getNombre() { return nombre; }
    public Persona getMadre() { return madre; }
}

public class ArbolGenealogico {
    public static void main(String[] args) {
        // Se construyen de "abajo hacia arriba" para inyectar las dependencias
        Persona abuela = new Persona("Carmen");
        Persona madre = new Persona("Ana", abuela);
        Persona nieto = new Persona("Luis", madre);

        System.out.println("Nieto: " + nieto.getNombre());
        System.out.println("Madre: " + nieto.getMadre().getNombre());
        System.out.println("Abuela: " + nieto.getMadre().getMadre().getNombre());
    }
}

```

## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?

Una relación de composición (o asociación) bidireccional se establece cuando dos clases mantienen referencias mutuas en sus estructuras internas. Esto permite que ambas partes puedan navegar la relación hacia la otra: el contenedor conoce a sus componentes, y simultáneamente, cada componente sabe a qué contenedor pertenece de forma directa.

Para implementar esto en el ejemplo de `Profesor` y `Departamento`, sería necesario añadir un atributo de tipo `Departamento` dentro de la clase `Profesor`. La complejidad técnica radica en mantener la consistencia entre ambas referencias. Cuando se añade un `Profesor` a un `Departamento`, no basta con incluirlo en la lista interna; también se debe actualizar el atributo del `Departamento` dentro de la instancia de `Profesor`. Esto requiere una lógica meticulosa de actualización cruzada en los métodos modificadores (o en los constructores) para evitar bucles infinitos o estados desincronizados.
