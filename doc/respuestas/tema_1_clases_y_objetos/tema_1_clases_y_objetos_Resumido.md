# TEMA 1. Clases y objetos (Resumido)

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

* **Abstracción**: Su propósito es modelar únicamente las características esenciales de una entidad, ignorando los detalles internos de implementación. Permite manejar sistemas complejos centrándose en el *qué* hace y no en el *cómo*.
* **Encapsulamiento**: Agrupa datos (estado) y métodos (comportamiento) en una única unidad (clase). Protege la integridad del estado restringiendo el acceso directo (mediante `private` o `protected`) y forzando la interacción a través de una interfaz pública.
* **Herencia**: Establece jerarquías ("es-un") que permiten a una subclase heredar atributos y métodos de una superclase. Evita la duplicidad y fomenta la reutilización de código.
* **Polimorfismo**: Permite que una referencia de tipo genérico se comporte de múltiples formas según la instancia concreta que aloje en tiempo de ejecución (enlace dinámico), facilitando la extensibilidad.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Además de Java, destacan:
* **C++**: Multipradigma, compilado a nativo, gestión de memoria manual.
* **C#**: Principal ecosistema .NET, sintaxis similar a Java.
* **Python**: Tipado dinámico, interpretado, donde todo es un objeto.
* **Kotlin**: Moderno, tipado estático, corre sobre la JVM y es el estándar actual para Android.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

* **Código no estructurado (Ensamblador)**: Secuencias lineales y control de flujo mediante saltos incondicionales (`GOTO`).
* **Programación Estructurada**: Mejora la legibilidad restringiendo el flujo exclusivamente a tres estructuras de control: secuencia, selección (`if`) e iteración (`while`/`for`).
* **Programación Modular**: Descompone el sistema en unidades funcionales independientes (módulos) con interfaces definidas para agrupar procedimientos y datos relacionados (precursor del encapsulamiento).

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

* **Identidad**: Propiedad única que distingue a un objeto de cualquier otro (ej. dirección de memoria).
* **Estado**: Conjunto de valores concretos de sus atributos en un instante determinado.
* **Comportamiento**: Acciones que el objeto puede ejecutar, definidas por sus métodos.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

* **Clase**: Es la plantilla abstracta ("blueprint") que define la estructura y comportamiento comunes a un tipo de entidad.
* **Objeto / Instancia**: Es la concreción material de esa clase en la memoria durante la ejecución. No son lo mismo (la clase es el molde, el objeto es la pieza).
* **Universalidad**: No todos los lenguajes usan clases. Algunos (como JavaScript original) usan **prototipos**, creando objetos mediante la clonación de otros existentes.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**?

* **Memoria**: En Java, los objetos residen en el **Heap** (memoria dinámica) y sus referencias en el **Stack** (pila). En lenguajes como C++, también pueden crearse directamente en el Stack.
* **Recolección de basura (Garbage Collection)**: Mecanismo automático de la JVM que identifica y libera la memoria de los objetos inalcanzables, previniendo *memory leaks* y evitando la gestión manual (`free`/`delete`).

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**?

* **Método**: Es una función asociada a una clase/objeto que define su comportamiento y permite alterar su estado interno de forma controlada.
* **Sobrecarga (Overloading)**: Polimorfismo estático que permite definir múltiples métodos con el **mismo nombre** en una clase, diferenciados obligatoriamente por su **firma** (cantidad, tipo u orden de parámetros).

## 8. Ejemplo mínimo de clase en Java, que se llame Punto...

```java
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Main {
    public static void main(String[] args) {
        Punto miPunto = new Punto();
        miPunto.x = 3.0;
        miPunto.y = 4.0;
        System.out.println("La distancia es: " + miPunto.calculaDistanciaAOrigen()); 
    }
}
```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

* **Punto de entrada**: `public static void main(String[] args)`.
* **`static`**: Indica que un método o atributo pertenece a la clase en sí, no a sus instancias. Permite invocarlo sin hacer un `new`. No existe la referencia `this` dentro de ellos.
* **Combinado con `final`**: Crea una **constante** de clase (`static final double PI = 3.1416`), inmutable y compartida en un único espacio de memoria.

## 10. Intenta ejecutar un poco de Java... ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

* **Proceso Híbrido**: `javac` compila el código fuente (`.java`) a un formato intermedio llamado **byte-code** (`.class`). No compila a código máquina nativo directamente.
* **JVM (Máquina Virtual)**: Software que interpreta y ejecuta el byte-code (`java Archivo`). Actúa como capa de abstracción sobre el hardware, garantizando la portabilidad universal del código.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo...

El operador `new` realiza tres acciones:
1.  Reserva memoria en el Heap.
2.  Llama al **constructor** (bloque especial encargado de inicializar el estado del objeto).
3.  Devuelve la referencia al nuevo objeto.

```java
class Empleado {
    String dni, nombre, apellidos;

    // Constructor
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}
```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes?

* **`this`**: Es una referencia implícita al objeto actual que está ejecutando el método. 
* Se usa principalmente para desambiguar (diferenciar un atributo de un parámetro con el mismo nombre). En Python, el equivalente es `self`.

```java
class Punto {
    double x;
    void setX(double x) {
        this.x = x; // 'this.x' es el atributo, 'x' es el parámetro
    }
}
```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`...

```java
    double distanciaA(Punto otroPunto) {
        double dx = this.x - otroPunto.x;
        double dy = this.y - otroPunto.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
```

## 14. El paso de parámetros a un método, ¿es por copia o por referencia?

En Java, el paso de parámetros siempre es **por copia (por valor)**:
* **Primitivos**: Se copia el valor literal. Los cambios locales no afectan fuera.
* **Objetos**: Se copia el *valor de la referencia* (puntero). 
    * Si modificas los atributos internos del objeto pasado, **sí afecta** fuera, porque ambas referencias apuntan a la misma memoria.
    * Si reasignas la variable a un `new` dentro del método, no afecta a la variable original.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes?

Es un método heredado de la clase base `Object` que devuelve una representación en texto del estado del objeto. Se invoca automáticamente al imprimir la instancia. Existen equivalentes, como `__str__` en Python.

```java
    @Override
    public String toString() {
        return "Punto[x=" + this.x + ", y=" + this.y + "]";
    }
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase?

Estructuralmente se parecen al agrupar datos compuestos. Sin embargo, al `struct` clásico le falta:
* La capacidad de vincular **comportamiento** (métodos) directamente a la estructura.
* Mecanismos de **encapsulamiento** y control de acceso (`private`).
* Soporte nativo para jerarquías (herencia y polimorfismo).

## 17. ¿Como se podría “emular”, con `struct` en C, la clase `Punto`? ¿Qué ha pasado con `this`?

Se emula declarando funciones externas que reciben explícitamente un puntero al `struct`. Ese puntero actúa funcionalmente como el `this` implícito de Java.

```c
struct Punto {
    double x;
    double y;
};

// Función emulando un método. El puntero 'self' reemplaza a 'this'
double calculaDistancia(struct Punto* self) {
    return sqrt(self->x * self->x + self->y * self->y);
}
```