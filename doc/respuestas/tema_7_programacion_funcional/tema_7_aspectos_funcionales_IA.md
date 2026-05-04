# TEMA 7. Aspectos funcionales

## Deficion de programación funcional
1. Objetivo: Que las funciones sean "ciudadanos de primera clase", es decir, que    sean un tipo/valor más:
    1. Pueden ser asignadas a viariables 
    2. Pueden ser recibidas como parametros 
    3. Pueden ser devueltas en otras funciones

2. "Expresiones lambda": Expresa un valor de tipo función. No tienen nmbre, solo cabecera y cuerpo

3. Closures

4. En lenguajes con comprobación estatica de tipos (java, ts, c# ...): ¿Que tipo tienen?

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

Un puntero a función en C es una variable que almacena la dirección de memoria donde comienza el código ejecutable de una función, en lugar de almacenar un dato convencional. Esto permite invocar funciones indirectamente, pasarlas como argumentos a otras funciones (conocidas como *callbacks*) o almacenarlas en estructuras de datos, brindando un alto grado de flexibilidad al diseño de algoritmos, como las rutinas de ordenación (ej. `qsort`).

Para declarar un puntero a función, es necesario especificar exactamente la firma de la función a la que apuntará (su tipo de retorno y los tipos de sus parámetros). Al invocarla a través del puntero, el compilador utiliza esta firma para verificar y preparar los argumentos en la pila de llamadas y esperar el valor de retorno adecuado.
```c
#include <stdio.h>
#include <ctype.h>
#include <stdlib.h>
#include <string.h>

// Definición de la función
char* convertirAMayusculas(const char* cadena) {
    char* resultado = strdup(cadena);
    for (int i = 0; resultado[i] != '\0'; i++) {
        resultado[i] = toupper(resultado[i]);
    }
    return resultado;
}

int main() {
    // Declaración del puntero a función que coincide con la firma
    char* (*aMayusculas)(const char*) = convertirAMayusculas;

    // Invocación a través del puntero
    char* texto = aMayusculas("hola mundo");
    printf("%s\n", texto);
    
    free(texto); // Liberar la memoria dinámica
    return 0;
}
```

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.

Una función lambda (o función anónima) es una subrutina definida de forma compacta y sin identificador (sin nombre) directamente en el lugar donde se necesita utilizar. Estas expresiones permiten definir comportamiento "al vuelo", tratándolo como un valor de datos que puede ser asignado a una variable, pasado como argumento o retornado desde otro método, constituyendo la base de la programación funcional moderna.

A diferencia de las funciones tradicionales (que requieren una declaración formal previa en un ámbito de clase o archivo), las lambdas reducen drásticamente la verbosidad sintáctica, ya que el compilador infiere gran parte de la información de tipos basándose en el contexto donde se declaran.

**En JavaScript:**
```javascript
// La función se define anónimamente y se asigna a la constante
const aMayusculas = (cadena) => cadena.toUpperCase();

// Invocación
console.log(aMayusculas("hola mundo"));
```

**En Java:**
```java
import java.util.function.Function;

public class LambdaBasica {
    public static void main(String[] args) {
        // La lambda se asigna a una referencia de tipo interfaz funcional
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        // Invocación a través del método 'apply' definido en la interfaz
        System.out.println(aMayusculas.apply("hola mundo"));
    }
}
```

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El **paradigma funcional** es un estilo de programación declarativo que trata la computación como la evaluación de funciones matemáticas puras, evitando estrictamente mutar el estado de los datos o crear efectos secundarios (como modificar variables globales o imprimir en consola de forma impredecible). En este paradigma, el control de flujo no se basa en bucles iterativos (`for`, `while`), sino en la composición y encadenamiento de funciones de orden superior (como `map`, `filter`, `reduce`).

A lenguajes como Java (a partir de la versión 8) o C++ (a partir de C++11) se les denomina **multi-paradigma** porque, manteniendo intacta su estructura fundamental orientada a objetos (clases, herencia, encapsulación), incorporaron herramientas funcionales (lambdas, *Streams*) para ofrecer soluciones más expresivas y seguras en contextos de procesamiento de colecciones y concurrencia.

Que una función sea un **"ciudadano de primera clase"** (*first-class citizen*) significa que el lenguaje otorga a las funciones los mismos derechos y capacidades que a un dato primitivo o a un objeto normal. Esto incluye la capacidad de ser instanciadas sin nombre, ser asignadas a variables, ser pasadas como argumentos a otras funciones y ser retornadas como resultados, todo en tiempo de ejecución.

## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis de una expresión lambda en Java se compone de tres partes fundamentales separadas por el operador flecha (`->`). La estructura general se lee como "parámetros que producen un resultado".

A la izquierda de la flecha se sitúa la **lista de parámetros** entre paréntesis. Si el compilador puede inferir el tipo de dato, este puede omitirse `(a, b)`. Si hay un único parámetro y su tipo es inferido, los paréntesis también pueden suprimirse `a ->`. Si no hay parámetros, se utilizan paréntesis vacíos `() ->`.

A la derecha de la flecha se encuentra el **cuerpo de la expresión**. Si el cuerpo consta de una única instrucción, no requiere el uso de llaves `{}` ni de la palabra clave `return`; el resultado de esa instrucción se evalúa y retorna automáticamente (cuerpo de expresión). Si el bloque requiere múltiples sentencias lógicas, se encierra entre llaves `{}` y es obligatorio utilizar la palabra clave `return` explícitamente si la interfaz funcional espera un valor de vuelta.

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

El concepto de recibir funciones como parámetros es el núcleo de las **funciones de orden superior** (*Higher-Order Functions*). Al abstraer el comportamiento (el "qué hacer") y pasarlo como argumento a un método estructural (el "cómo aplicarlo"), se logra un diseño sumamente flexible. El método `transformar` se convierte en una plantilla genérica que delega la lógica específica de transformación a la función inyectada.

**En JavaScript:**
```javascript
// Función de orden superior que recibe una función callback
function transformar(texto, operacion) {
    return operacion(texto);
}

const aMayusculas = (cadena) => cadena.toUpperCase();

// Se pasa la función como dato
console.log(transformar("hola mundo", aMayusculas));
```

**En Java:**
```java
import java.util.function.Function;

public class OrdenSuperior {
    
    // Método que recibe el comportamiento como una Function
    public static String transformar(String texto, Function<String, String> operacion) {
        // Se ejecuta la lógica delegada mediante .apply()
        return operacion.apply(texto);
    }

    public static void main(String[] args) {
        Function<String, String> aMayusculas = c -> c.toUpperCase();
        
        System.out.println(transformar("hola mundo", aMayusculas));
    }
}
```

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

Pasar la expresión lambda directamente (conocido como *inline*) en el punto de invocación de la función de orden superior maximiza la legibilidad y la concisión del código. Esta práctica elimina la necesidad de declarar variables temporales para comportamientos efímeros que solo se utilizarán una vez, acercando la definición de la lógica al lugar exacto donde se ejecuta.

En este ejemplo, la lógica de inversión (utilizando un `StringBuilder` por eficiencia de mutación interna) se define anónimamente dentro de los propios paréntesis de la llamada al método `transformar`.
```java
import java.util.function.Function;

public class InversionInline {

    public static String transformar(String texto, Function<String, String> operacion) {
        return operacion.apply(texto);
    }

    public static void main(String[] args) {
        // Se define el comportamiento de inversión "al vuelo" directamente en la llamada
        String resultado = transformar("hola mundo", 
            cadena -> new StringBuilder(cadena).reverse().toString()
        );
        
        System.out.println(resultado); // Imprime: "odnum aloh"
    }
}
```

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

Un cierre o *closure* es una característica que permite a una función lambda "capturar" y recordar el contexto léxico en el que fue creada. Esto significa que la lambda puede acceder a variables locales que fueron definidas fuera de su cuerpo (en el ámbito circundante), y mantener acceso a esos valores incluso si la función lambda es ejecutada posteriormente, en un contexto completamente distinto o en otro hilo de ejecución.

En Java, existe una restricción de seguridad estricta respecto a las variables locales capturadas en un *closure*: deben ser "efectivamente finales" (*effectively final*). Esto significa que, aunque no posean la palabra clave `final`, su valor no debe ser reasignado nunca después de su inicialización; de lo contrario, el compilador generará un error. Esto evita problemas complejos de sincronización de memoria y efectos secundarios impredecibles.
```java
import java.util.function.Function;

public class EjemploClosure {

    public static String transformar(String texto, Function<String, String> operacion) {
        return operacion.apply(texto);
    }

    public static void main(String[] args) {
        // Variable local en el ámbito circundante (debe ser efectivamente final)
        String sufijo = " [Procesado]"; 

        // La lambda forma un "closure" capturando la variable 'sufijo' del exterior
        String resultado = transformar("Documento", 
            cadena -> cadena + sufijo
        );
        
        // sufijo = " [Modificado]"; // Descomentar esto causaría un error de compilación
        
        System.out.println(resultado); // Imprime: "Documento [Procesado]"
    }
}
```

## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

Aunque a simple vista ambas herramientas sirven para pasar comportamiento como argumento, su naturaleza conceptual y técnica es fundamentalmente distinta. Un puntero a función en C es una simple dirección de memoria que apunta a una subrutina global y estática en el segmento de código. Este puntero carece de contexto u estado; no puede recordar el entorno donde fue creado ni capturar variables locales externas.

Por el contrario, una función lambda (especialmente aquellas que forman un *closure*) posee **estado y comportamiento**. En lenguajes orientados a objetos como Java, una lambda es convertida internamente por el compilador en una instancia de una clase anónima que implementa una interfaz funcional. Las variables externas que la lambda captura se guardan físicamente como atributos ocultos dentro de este objeto invisible en el *Heap*. Por lo tanto, una lambda no es solo código (como en C), sino un objeto completo que transporta consigo su propio contexto de ejecución.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

El retorno de funciones es otra manifestación potente de las funciones de orden superior, permitiendo crear "fábricas de comportamiento" o funciones especializadas dinámicamente configuradas (*Currying* o aplicación parcial). El método devuelve una expresión lambda que, al momento de generarse, encierra la lógica específica configurada por el parámetro de entrada.

La *closure* generada en este caso captura el argumento `porcentaje` (pasado al método de fábrica) dentro del cuerpo de la lambda retornada. Aunque el método `crearDescuento` termine su ejecución y su contexto local sea teóricamente destruido en el Stack, las funciones generadas (`desc10` y `desc50`) mantienen retenido el valor específico del porcentaje en la memoria dinámica. Cada función retornada posee su propio estado aislado encapsulado en su *closure*.
```java
import java.util.function.Function;

public class FabricaDescuentos {

    // Método que actúa como fábrica y retorna una función lambda
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda devuelta captura el valor de 'porcentaje' en su closure
        return (precioOriginal) -> precioOriginal - (precioOriginal * (porcentaje / 100.0));
    }

    public static void main(String[] args) {
        // Se instancian dos comportamientos diferentes generados dinámicamente
        Function<Double, Double> desc10 = crearDescuento(10.0);
        Function<Double, Double> desc50 = crearDescuento(50.0);

        double precio = 100.0;
        
        System.out.println("Precio con 10% off: " + desc10.apply(precio)); // 90.0
        System.out.println("Precio con 50% off: " + desc50.apply(precio)); // 50.0
    }
}
```

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una **interfaz funcional** es una interfaz en Java diseñada específicamente para actuar como el tipo de dato que el compilador asignará a una expresión lambda (o a una referencia de método). Al existir una comprobación estática de tipos estricta, Java no permite "funciones puras" huérfanas en el vacío; toda función lambda debe ser interpretada como la implementación concreta de una interfaz existente.

El requisito absoluto e indispensable de una interfaz funcional es que contenga **exactamente un único método abstracto** (sin implementar). Puede contener cualquier cantidad de métodos predeterminados (`default`), métodos estáticos o métodos sobrescritos de la clase `Object` (como `equals`), pero el contrato que exige implementación debe ser uno solo. Esto es crucial porque el compilador necesita saber inequívocamente a qué método abstracto asociar el código de la lambda proporcionada por el desarrollador. Generalmente, se decoran con la anotación `@FunctionalInterface` para forzar al compilador a verificar este requisito.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).
```java
// La anotación es opcional pero altamente recomendada por seguridad
@FunctionalInterface
public interface Transformador {
    
    // Exactamente un método abstracto. Define la firma (String -> String)
    String ejecutar(String origen);
    
}

// Ejemplo de uso contextual:
class UsoTransformador {
    public static void main(String[] args) {
        // La lambda coincide con la firma de 'ejecutar'
        Transformador pasarAMinusculas = texto -> texto.toLowerCase(); 
        
        System.out.println(pasarAMinusculas.ejecutar("TEXTO PRUEBA"));
    }
}
```

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

Al introducir genericidad (`<T, R>`), la interfaz funcional se convierte en una plantilla universal. La letra `T` suele representar el tipo de la entrada (*Type*) y `R` el tipo del retorno (*Result*). Esto elimina la necesidad de crear interfaces dedicadas para cada posible combinación de conversiones en el sistema (ej. una interfaz para `DoubleToInt`, otra para `StringToInt`, etc.).

Al instanciar la lambda, el compilador infiere los tipos basándose en los genéricos declarados en el lado izquierdo de la asignación, garantizando la seguridad en la conversión (en este caso, de `Double` a `Integer`).
```java
@FunctionalInterface
public interface TransformadorGenerico<T, R> {
    
    R transformar(T entrada);
    
}

class UsoGenerico {
    public static void main(String[] args) {
        // Define la transformación concreta: Double -> Integer
        TransformadorGenerico<Double, Integer> redondeador = (Double valor) -> (int) Math.round(valor);
        
        Integer resultado = redondeador.transformar(5.7);
        System.out.println("Redondeado: " + resultado); // Imprime 6
    }
}
```

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Efectivamente, para evitar que cada programador reinvente interfaces funcionales genéricas comunes, Java introdujo en el paquete `java.util.function` un conjunto estándar de interfaces preparadas para casi cualquier escenario.

Las familias principales son:
*   **`Function<T, R>`**: Recibe un argumento de tipo `T` y devuelve un resultado de tipo `R`. Su método es `apply()`. (Útil para mapeo y transformaciones).
*   **`Predicate<T>`**: Recibe un argumento de tipo `T` y devuelve un valor primitivo booleano (`boolean`). Su método es `test()`. (Útil para filtrado y condiciones).
*   **`Consumer<T>`**: Recibe un argumento de tipo `T` y no devuelve nada (`void`). Su método es `accept()`. (Útil para ejecutar acciones finales, como imprimir o escribir en base de datos).
*   **`Supplier<T>`**: No recibe ningún argumento y devuelve un objeto de tipo `T`. Su método es `get()`. (Útil para la creación diferida de objetos o generación de valores).

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método `forEach` integrado en las colecciones de Java, introduce lo que se conoce como "iteración interna". A diferencia del bucle `for` tradicional donde el programador gestiona manualmente el estado del índice o el iterador (iteración externa), con `forEach` se delega la mecánica del recorrido a la propia colección. Se inyecta exclusivamente el comportamiento (un `Consumer`) que la colección debe aplicar individualmente a cada elemento.
```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-2, 5, -8, 10, 0);

        // Uso expresivo: la iteración es gestionada internamente
        numeros.forEach(num -> {
            if (num > 0) {
                System.out.println("El número " + num + " es positivo.");
            }
        });
    }
}
```

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

La firma de `forEach` emplea un *wildcard* de límite inferior (`? super T`) para habilitar la **contravarianza**, lo que maximiza la flexibilidad del código. Significa que, si se tiene una lista de tipos específicos (ej. `List<Perro>`), el método `forEach` aceptará un consumidor especializado en `Perro` (`Consumer<Perro>`), pero también, gracias al *wildcard*, aceptará de forma segura un consumidor genérico diseñado para la superclase (`Consumer<Animal>` o `Consumer<Object>`). Si fuera solo `Consumer<T>`, rechazaría los consumidores genéricos estáticamente.

**PECS** es el acrónimo introducido por Joshua Bloch que rige el uso de *wildcards*: **"Producer Extends, Consumer Super"**. 
1.  **Producer Extends (`? extends T`)**: Si un parámetro, estructura o función actúa como productor o proveedor de datos (sólo emite valores para ser leídos), debe usar covarianzas con límite superior. Esto asegura que todo lo leído es de tipo `T` o un derivado.
2.  **Consumer Super (`? super T`)**: Si actúa como consumidor (recibe datos y opera con ellos sin devolver nada), debe usar contravarianza. Permite usar funciones creadas para operar en tipos más altos de la jerarquía que saben tratar con el objeto recibido.

Para mejorar el método genérico `transformar(T entrada, Function<T, R> operacion)`, aplicando PECS la firma ideal sería: `<T, R> R transformar(T entrada, Function<? super T, ? extends R> operacion)`. 
La función actúa como consumidor de la entrada (por lo que es seguro que la función espere operar sobre superclases de `T`) y actúa como productor del retorno (por lo que es seguro que la función retorne clases más específicas que `R` que puedan ser asignadas de vuelta de forma segura).

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las referencias a métodos son una sintaxis abreviada ("azúcar sintáctico") que permite apuntar directamente a un método ya existente que posee la firma exacta que requiere una interfaz funcional, evitando tener que escribir una función lambda repetitiva cuyo único cuerpo sería la invocación de ese mismo método.

**En JavaScript:**
```javascript
class Persona {
    constructor(nombre) { this.nombre = nombre; }
    
    saludar() {
        return "Hola, soy " + this.nombre;
    }
}

const personaJS = new Persona("Ana");
// En JS, al extraer el método, se pierde el contexto 'this'. 
// Es imprescindible usar '.bind()' para atar el método a la instancia original.
const referenciaSaludarJS = personaJS.saludar.bind(personaJS);

console.log(referenciaSaludarJS());
```

**En Java:**
```java
import java.util.function.Supplier;

class Persona {
    private String nombre;
    public Persona(String nombre) { this.nombre = nombre; }
    
    public String saludar() {
        return "Hola, soy " + this.nombre;
    }
}

public class ReferenciaInstancia {
    public static void main(String[] args) {
        Persona personaJava = new Persona("Luis");

        // Referencia a un método de una instancia concreta (operador ::)
        // Coincide con la firma de Supplier (no parámetros, retorna algo)
        Supplier<String> referenciaSaludarJava = personaJava::saludar;

        System.out.println(referenciaSaludarJava.get());
    }
}
```

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

Existen cuatro categorías fundamentales para referenciar métodos utilizando el operador de doble dos puntos (`::`), cada una adecuada para diferentes necesidades de la API funcional.

1.  **Referencia a método estático:** `Clase::metodoEstatico`. Es la más sencilla, simplemente invoca una función global. (Ej. `Math::max` para apuntar a `Math.max(a, b)`).
2.  **Referencia a un constructor:** `Clase::new`. Genera un proveedor que instancia nuevos objetos llamando al constructor apropiado. (Ej. `ArrayList::new` como fábrica para crear nuevas listas vacías).
3.  **Referencia a método de una instancia concreta:** `instancia::metodo`. Se ata el método a un objeto específico ya creado, como se demostró en el ejemplo de `personaJava::saludar`.
4.  **Referencia a método de instancia de un tipo arbitrario:** `Clase::metodoInstancia`. Es la más compleja. Se usa cuando la instancia sobre la que operará el método se pasará como el primer argumento en el momento de la ejecución. (Ej. `String::toUpperCase`. La interfaz funcional esperará que el primer parámetro sea el `String` específico que se pondrá en mayúsculas).

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

El paso de funciones de comparación ha sido históricamente la razón de ser de interfaces como `Comparator` (que exigía instanciar clases anónimas verbosas). Con lambdas, la ordenación personalizada de colecciones de objetos complejos se reduce a una sola línea léxica.

La versión "manual" elabora las sentencias condicionales explícitas dentro del bloque de la lambda comparando atributo por atributo. La segunda versión utiliza la API funcional avanzada de `Comparator`, que aprovecha las referencias de métodos estáticos y el encadenamiento fluido (`thenComparing`) para describir la estrategia de ordenación de forma declarativa, sin escribir la mecánica de comparación de enteros o cadenas subyacente.
```java
import java.util.*;

class Persona {
    String nombre;
    int edad;
    public Persona(String nombre, int edad) { this.nombre = nombre; this.edad = edad; }
    public String getNombre() { return nombre; }
    public int getEdad() { return edad; }
    @Override public String toString() { return nombre + ":" + edad; }
}

public class OrdenacionFuncional {
    public static void main(String[] args) {
        List<Persona> gente = Arrays.asList(
            new Persona("Zoe", 30), new Persona("Ana", 25), new Persona("Luis", 30)
        );

        // VERSIÓN 1: Comparación explícita manual con lambda de bloque
        Collections.sort(gente, (p1, p2) -> {
            if (p1.getEdad() != p2.getEdad()) {
                return Integer.compare(p1.getEdad(), p2.getEdad()); // Orden principal: Edad
            } else {
                return p1.getNombre().compareTo(p2.getNombre());    // Empate: Orden alfabético
            }
        });
        System.out.println("Orden Manual: " + gente);

        
        // VERSIÓN 2: Uso de la API fluida de Comparator + Referencias de método (Sintaxis declarativa)
        Collections.sort(gente, 
            Comparator.comparingInt(Persona::getEdad)
                      .thenComparing(Persona::getNombre)
        );
        System.out.println("Orden Comparator API: " + gente);
    }
}
```

```</Persona></String></Integer></Double,></T,></Double,></Double,></Double,></String,></String,></String,></String,></String,>