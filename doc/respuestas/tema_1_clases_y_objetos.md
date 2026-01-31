
# TEMA 1. Clases y objetos

  

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

  

**Abstracción**: se centra en el diseño conceptual, aislando la complejidad al modelar únicamente las características esenciales de una entidad para el contexto del problema. En Java, este mecanismo se articula mediante `interface` y clases `abstract` que definen contratos de comportamiento, permitiendo que el desarrollo se enfoque en qué funciones realiza un objeto, ignorando los detalles de cómo se implementan internamente.

  

**Encapsulamiento**: asegura la integridad del objeto al agrupar datos y comportamientos en una única unidad lógica, restringiendo el acceso directo al estado interno. Mediante el uso de modificadores de acceso (`private`, `protected`), se obliga a interactuar con los datos a través de métodos públicos controlados. Esto reduce el acoplamiento y permite modificar la implementación interna sin afectar a los clientes que consumen la clase.

  

**Herencia**: promueve la reutilización y organización del código estableciendo relaciones jerárquicas de tipo "es-un" entre clases. Al permitir que una subclase adquiera propiedades y métodos de una superclase (mediante `extends`), se facilita la especialización del comportamiento y se evita la redundancia, situando la lógica común en los niveles superiores de la jerarquía.

  

El **polimorfismo** otorga flexibilidad al sistema permitiendo que una referencia de tipo genérico se comporte de diferentes maneras según la instancia concreta que aloje en tiempo de ejecución. Gracias a la sobrescritura de métodos (`@Override`) y al enlace dinámico (*dynamic binding*), un mismo mensaje puede desencadenar distintas implementaciones, desacoplando el código cliente de las clases específicas.

  

Finalmente, estos cuatro conceptos operan de manera interdependiente para gestionar la complejidad del software. La abstracción dicta el modelo y el encapsulamiento protege su implementación; la herencia propaga dicha estructura evitando duplicidades, y el polimorfismo aprovecha la jerarquía resultante para permitir la extensibilidad y el tratamiento uniforme de objetos heterogéneos.

  
  

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Cuatro lenguajes representativos, además de Java, que implementan este paradigma son:

* **C++**: Evolución directa de C que introdujo el concepto de clases. Es un lenguaje multiparadigma que permite una gestión de memoria manual y un alto rendimiento.
* **C#**: Lenguaje principal del ecosistema .NET de Microsoft, sintácticamente similar a Java pero con características modernas como propiedades, eventos y LINQ.
* **Python**: Lenguaje interpretado de tipado dinámico donde "todo es un objeto". Destaca por su legibilidad y simplicidad sintáctica.
* **Kotlin**: Lenguaje moderno estáticamente tipado que se ejecuta sobre la JVM y es totalmente interoperable con Java. Es el estándar actual para desarrollo Android y mejora la POO tradicional reduciendo el código repetitivo (_boilerplate_) mediante el uso de _data classes_, propiedades y extensiones, integrando además seguridad contra nulos (_null-safety_) en su diseño..

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La **programación estructurada** surge como respuesta al "código espagueti" generado por el uso indiscriminado de saltos incondicionales (`GOTO`). Este paradigma propone mejorar la claridad y calidad del software mediante el uso exclusivo de subrutinas y tres estructuras de control básicas: secuencia, selección (`if`/`switch`) e iteración (`for`/`while`). Se centra en el flujo lógico del algoritmo más que en los datos.

La **programación modular** es un paso evolutivo superior que introduce el concepto de dividir el sistema en partes más pequeñas e independientes llamadas módulos. Cada módulo agrupa procedimientos y datos relacionados, exponiendo una interfaz clara y ocultando su complejidad interna. Es el precursor directo del encapsulamiento en POO, ya que permite trabajar en componentes aislados, facilitando la compilación separada y el mantenimiento, aunque carece del vínculo fuerte entre estado y comportamiento que caracteriza a los objetos.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

Un objeto se define por la conjunción de **identidad**, **estado** y **comportamiento**. La **identidad** es la propiedad que distingue a un objeto de todos los demás, independientemente de sus valores internos (en Java, comparable a la dirección de memoria).

El **estado** representa el conjunto de valores de los atributos del objeto en un instante dado, definiendo sus características concretas. El **comportamiento** está determinado por los métodos que el objeto puede ejecutar, definiendo cómo reacciona ante mensajes externos y cómo modifica su propio estado.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una **clase** es la abstracción o plantilla que define la estructura y el comportamiento común de un conjunto de entidades. No es lo mismo que un **objeto**; la relación es análoga a la de un molde con las piezas producidas. El objeto es la concreción material en memoria de dicha clase. El término **instancia** es técnico y sinónimo de objeto, refiriéndose al acto de manifestar una clase específica en tiempo de ejecución.

No todos los lenguajes orientados a objetos utilizan clases. Existen lenguajes basados en **prototipos** (como JavaScript original o Lua), donde no existen las clases como moldes estáticos. En ellos, la creación de objetos se realiza mediante la clonación de objetos existentes (prototipos) y la extensión de sus funcionalidades de manera dinámica.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**?

En Java, los objetos se almacenan exclusivamente en el espacio de memoria conocido como **Heap** (montículo), mientras que las referencias a dichos objetos se ubican en el **Stack** (pila) de ejecución. Esto no es igual en todos los lenguajes; en C++, por ejemplo, es posible instanciar objetos directamente en el Stack, lo que implica que se destruyen automáticamente al salir del ámbito de la función, sin necesidad de gestión dinámica.

La **recolección de basura** (*Garbage Collection*) es un mecanismo automático de gestión de memoria presente en entornos como la JVM o PVM. Su función es identificar y liberar la memoria ocupada por objetos que ya no son alcanzables por ninguna referencia activa en el programa, evitando fugas de memoria (*memory leaks*) y liberando al programador de la responsabilidad de destruir objetos manualmente (`free` o `delete`).

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**?

Un **método** es una subrutina o función que está asociada a una clase o a un objeto. Representa el comportamiento y es el único medio permitido (bajo un encapsulamiento estricto) para alterar el estado interno del objeto.

La **sobrecarga de métodos** (*overloading*) es una forma de polimorfismo estático (en tiempo de compilación). Consiste en definir múltiples métodos con el mismo nombre dentro de la misma clase, pero diferenciándolos por su **firma** (número, tipo u orden de los parámetros). Esto permite realizar operaciones conceptualmente similares sobre diferentes tipos de datos.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

```java
class Punto {
    // Atributos con visibilidad por defecto (package-private)
    double x;
    double y;

    // Método para calcular la distancia al origen (0,0)
    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

// Ejemplo de uso
public class Main {
    public static void main(String[] args) {
        // Instancia del objeto de la clase Punto
        Punto miPunto = new Punto();
        
        // Asignación de estado
        miPunto.x = 3.0;
        miPunto.y = 4.0;
        
        // Uso del método (Comportamiento)
        double distancia = miPunto.calculaDistanciaAOrigen();
        System.out.println("La distancia es: " + distancia); // Devuelve: 5.0
    }
}
```

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El punto de entrada estándar es el método público `public static void main(String[] args)`. El modificador **`static`** indica que el miembro (método o atributo) pertenece a la clase en sí misma y no a una instancia particular. Esto permite invocarlo sin necesidad de crear un objeto (`new`), lo cual es imprescindible para el `main` ya que al iniciar el programa aún no existen objetos.

`static` no se usa solo para el `main`; se emplea frecuentemente para métodos utilitarios (como `Math.sqrt`), contadores compartidos entre instancias o el patrón Singleton. Cuando se combina con **`final`** en una variable (`static final`), se define una **constante** a nivel de clase, inmutable y compartida por todas las instancias, optimizando memoria y asegurando integridad de valores fijos.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para compilar se utiliza el comando `javac Archivo.java`, que genera un fichero `Archivo.class`. Para ejecutarlo, se usa `java Archivo` (sin extensión). Java se considera un lenguaje híbrido o semicompilado: el código fuente se compila a un formato intermedio (*byte-code*), no a código máquina nativo directamente.

La **Máquina Virtual de Java (JVM)** es el software encargado de interpretar y ejecutar ese código intermedio. Actúa como una capa de abstracción sobre el hardware, garantizando la portabilidad ("Write Once, Run Anywhere"). Ese código intermedio es el **byte-code** (contenido en los ficheros `.class`), un conjunto de instrucciones optimizadas para la JVM, que luego son interpretadas o recompiladas a código nativo en tiempo de ejecución por el compilador JIT (Just-In-Time) para mejorar el rendimiento.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

La palabra clave **`new`** es un operador que realiza tres acciones: reserva espacio en memoria Heap para el nuevo objeto, inicializa los atributos a sus valores por defecto y llama al constructor de la clase. Un **constructor** es un bloque de código especial, con el mismo nombre que la clase, cuyo propósito exclusivo es inicializar el estado del objeto recién creado.

```java
class Empleado {
    String dni;
    String nombre;
    String apellidos;

    // Constructor
    public Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

// Uso: Empleado e = new Empleado("12345678A", "Juan", "Pérez");
```

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

**`this`** es una referencia implícita disponible dentro de los métodos de instancia que apunta al propio objeto que está ejecutando el código. Se utiliza principalmente para desambiguar entre atributos de clase y parámetros locales con el mismo nombre, o para invocar otros constructores de la misma clase. No se llama igual en todos los lenguajes; en Python se usa `self` (por convención, aunque explícito) .

```java
class Punto {
    double x, y;
    
    // Uso de 'this' para diferenciar el atributo 'x' del parámetro 'x'
    void setX(double x) {
        this.x = x; 
    }
}
```

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

```java
    // Método dentro de la clase Punto
    double distanciaA(Punto otroPunto) {
        // 'this' es el punto actual, 'otroPunto' es el parámetro
        double dx = this.x - otroPunto.x;
        double dy = this.y - otroPunto.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
```

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función?

En Java, el paso de parámetros es **estrictamente por valor (por copia)**. Sin embargo, hay un matiz crucial:
1.  Cuando se pasa un objeto (como `Punto`), lo que se copia es el **valor de la referencia** (la dirección de memoria). Por tanto, si dentro del método se modifican los atributos del objeto (`p.x = 5`), los cambios **sí afectan** al objeto original fuera del método, porque ambas copias de la referencia apuntan al mismo sitio en el Heap.
2.  Si se reasigna la referencia completa (`p = new Punto()`), el cambio **no afecta** fuera, ya que solo se modifica la copia local del puntero.

Si se pasa un tipo primitivo como `int`, se copia el valor literal. Cualquier modificación al entero dentro del método es puramente local y **no afecta** a la variable original, ya que no hay referencias de memoria involucradas, solo valores en el Stack.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método `toString()` es un método definido en la clase raíz `java.lang.Object`, heredado por todas las clases en Java. Su propósito es devolver una representación en formato texto (`String`) del estado del objeto. Es invocado automáticamente cuando se concatena un objeto con una cadena o se imprime en consola. Existen equivalentes en otros lenguajes, como el método mágico `__str__` en Python o `ToString()` en C#.

```java
    // Dentro de la clase Punto
    @Override
    public String toString() {
        return "Punto[x=" + this.x + ", y=" + this.y + "]";
    }
```

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Estructuralmente, un `struct` en C se asemeja a una clase en que agrupa diferentes tipos de datos bajo un mismo nombre. Sin embargo, un `struct` clásico es puramente un contenedor de datos pasivo.

Para que un `struct` sea funcionalmente equivalente a una clase, le falta la capacidad de **vincular comportamiento (métodos)** directamente a los datos, y mecanismos de **control de acceso** (encapsulamiento real con `private`/`protected`). En C, las funciones están separadas de los datos. Además, carece de soporte nativo para herencia y polimorfismo. Un `struct` define un tipo de dato compuesto, pero no instaura el paradigma de "objeto" como entidad autónoma con responsabilidad propia.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

Para emular esto en C, se define el `struct` para los datos y funciones separadas que reciben un puntero a dicho `struct` como primer argumento.

```c
struct Punto {
    double x;
    double y;
};

// Función que emula el método
double calculaDistancia(struct Punto* self) {
    return sqrt(self->x * self->x + self->y * self->y);
}
```

Lo que en Java es la referencia implícita **`this`**, en esta emulación se convierte en el parámetro explícito **`struct Punto* self`**. En realidad, esto es lo que hacen internamente los compiladores de lenguajes orientados a objetos: pasan el puntero del objeto actual como un argumento oculto en cada llamada a método de instancia.