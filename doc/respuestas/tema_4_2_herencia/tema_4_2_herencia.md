# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

La herencia es un pilar fundamental de la orientación a objetos que permite crear nuevas clases (subclases o clases derivadas) a partir de clases existentes (superclases o clases base). Establece una relación semántica estricta del tipo "es-un" (*is-a*); por ejemplo, se puede afirmar lógicamente que un artillero "es un" soldado. Esta relación organiza el código en jerarquías que promueven la especialización y evitan la redundancia.

La primera implicación principal es la **herencia de estado y comportamiento**: la subclase adquiere automáticamente todos los atributos y métodos de la superclase, evitando tener que reescribir la lógica común. La segunda implicación es la **compatibilidad de tipos**: una instancia de una subclase es plenamente compatible y puede ser tratada en cualquier lugar del código donde se espere una instancia de su superclase, lo que permite un alto grado de abstracción y polimorfismo.

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Señor, sí señor. Soy el soldado " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() { return cohetes; }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() { return minas; }
}

public class Main {
    public static void main(String[] args) {
        // Uso de la compatibilidad de tipos: Array del supertipo
        Soldado[] peloton = new Soldado[3];
        peloton[0] = new Soldado("Pérez");
        peloton[1] = new Artillero("Gómez", 5);
        peloton[2] = new Zapador("López", 10);

        // Se recorre sin importar el subtipo concreto
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}
```

## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

Al instanciar un objeto de una subclase, se produce un encadenamiento de constructores. Se ejecuta primero el constructor de la superclase más alta en la jerarquía y el proceso desciende hasta ejecutar finalmente el constructor de la subclase concreta que se está instanciando. Por lo tanto, al crear un `Artillero`, se ejecutan dos constructores: primero el de `Soldado` para inicializar el estado base, y luego el de `Artillero` para su estado específico.

La palabra reservada `super` dentro de un constructor se utiliza para invocar explícitamente a un constructor específico de la clase padre. Debe ser, obligatoriamente, la primera instrucción dentro del bloque del constructor de la subclase.

Si la clase base no posee un constructor público o protegido sin parámetros (es decir, el constructor por defecto no está disponible, como ocurre en la clase `Soldado` que obliga a pasar un `String`), es estrictamente obligatorio llamar a `super(...)` pasando los argumentos requeridos. Si no se hace, el compilador emitirá un error, ya que no sabría cómo inicializar el estado de la parte del objeto correspondiente a la superclase.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

Físicamente, los atributos privados de la superclase sí forman parte integral de la instancia de la subclase en la memoria dinámica (Heap). Cuando se hace un `new Artillero(...)`, el sistema reserva el espacio necesario tanto para el atributo `cohetes` como para el atributo `nombre` heredado de la clase `Soldado`. El objeto en memoria es una entidad completa que alberga todo el estado de su jerarquía.

Sin embargo, a nivel lógico y de compilación, la existencia física no implica accesibilidad. El modificador `private` impone un encapsulamiento estricto: el código fuente dentro de la clase `Artillero` no puede acceder directamente a la variable `nombre`. Para manipular o consultar ese dato desde la subclase, se depende exclusivamente de los métodos que la clase `Soldado` haya decidido exponer en su interfaz pública o protegida (como el método `saludar()` o si existiese un `getNombre()`).

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

La compatibilidad de tipos (polimorfismo) otorga al software una extensibilidad extrema, cumpliendo con el principio de diseño "Abierto/Cerrado": el código está abierto a la extensión pero cerrado a la modificación. Permite que el código cliente opere sobre abstracciones (el supertipo) ignorando las implementaciones específicas. Cuando el sistema evoluciona, se pueden integrar nuevas funcionalidades y clases sin necesidad de alterar ni una sola línea de la lógica preexistente que orquesta a dichos objetos.

En el ejemplo del pelotón, la lógica de pedir el saludo funciona mediante el supertipo `Soldado`. Si en el futuro el modelo requiere la incorporación de personal médico, basta con crear la nueva clase. El bucle original que solicita el saludo continuará funcionando perfectamente con las nuevas instancias.

```java
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }
}

// En el código cliente (Main):
// Se puede añadir el médico al pelotón.
// El siguiente bucle NO SE MODIFICA EN ABSOLUTO, demostrando la extensibilidad.
/*
for (Soldado s : peloton) {
    s.saludar(); 
}
*/
```

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

Sí, es perfectamente legal y común tener una referencia del supertipo apuntando a una instancia real del subtipo (ej. `Soldado s = new Artillero(...)`). Sin embargo, a través de esa referencia de supertipo, el compilador solo permite invocar los métodos definidos explícitamente en la clase del supertipo, impidiendo el acceso directo a los métodos exclusivos del subtipo (como `getCohetes()`), porque la referencia actúa como un filtro de visibilidad.

El **"upcasting"** es la conversión hacia arriba en la jerarquía (tratar un `Artillero` como `Soldado`); es implícita y siempre segura. El **"downcasting"** es la conversión hacia abajo (tratar una referencia `Soldado` como `Artillero`); es explícita y puede causar errores en tiempo de ejecución si el objeto real no coincide con el tipo forzado. Para evitar fallos, se emplea el operador `instanceof`, el cual evalúa en tiempo de ejecución si un objeto real es una instancia de una clase concreta antes de proceder al cast.

```java
        for (Soldado s : peloton) {
            // Se usa instanceof para asegurar la viabilidad del downcasting
            if (s instanceof Artillero) {
                // Downcasting: forzamos la referencia a Artillero
                Artillero artilleroReal = (Artillero) s;
                System.out.println("Tengo " + artilleroReal.getCohetes() + " cohetes listos.");
            }
        }
```

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

El acceso **"protegido"** es un nivel intermedio de encapsulamiento diseñado específicamente para flexibilizar la herencia. Un miembro (atributo o método) declarado como protegido permanece oculto para el público general, operando como si fuera privado para el mundo exterior. Sin embargo, permite el acceso directo a dicho miembro desde cualquier subclase de la jerarquía (independientemente del paquete donde resida) y desde cualquier otra clase situada dentro del mismo paquete.

En Java, se implementa sustituyendo la palabra reservada `private` por `protected`. Esto rompe parcialmente la encapsulación en favor de la conveniencia, permitiendo que las clases derivadas manipulen directamente el estado base sin tener que recurrir a la interfaz pública del padre.

```java
class Soldado {
    // Cambio a visibilidad protegida
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMina() {
        // Acceso directo y legal al atributo 'nombre' de la superclase
        System.out.println("El zapador " + this.nombre + " ha colocado una mina.");
        this.minas--;
    }
}
```

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

La existencia de una clase base o clase raíz cósmica de la que deriven implícitamente todas las demás no es una regla universal en el paradigma de la orientación a objetos. Lenguajes como C++ no poseen una única clase base suprema; los programadores pueden definir jerarquías de herencia completamente aisladas y desconectadas entre sí dentro de un mismo programa.

En Java, por el contrario, sí existe una clase raíz absoluta. Todas las clases, sin excepción, heredan directa o indirectamente de la clase `java.lang.Object`. Si al declarar una clase no se especifica la palabra reservada `extends`, el compilador inyecta implícitamente una derivación de `Object`. Esto asegura que todos los objetos compartan un comportamiento mínimo estandarizado (como los métodos `toString()`, `equals()`, `hashCode()`) y permite la creación de colecciones genéricas que pueden almacenar cualquier tipo de instancia.

## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

La **herencia múltiple** de clases es una característica mediante la cual una subclase puede tener más de una superclase directa de la cual heredar simultáneamente tanto estado (atributos) como implementación (métodos). Esta capacidad suele introducir problemas de ambigüedad severos, conocidos como el "problema del diamante", donde el compilador no puede determinar qué versión de un método o atributo heredar si ambas clases base poseen miembros con la misma firma.

En Java, **no existe la herencia múltiple de clases** para evitar esta ambigüedad lógica. Una clase únicamente puede utilizar la palabra clave `extends` una vez, derivando de un único padre. Sin embargo, para no perder flexibilidad, Java permite la "herencia múltiple de tipos" mediante el uso de interfaces. Una clase puede implementar múltiples interfaces (`implements`), adquiriendo múltiples "contratos" de comportamiento, pero sin heredar atributos ni estados en conflicto.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

Para crear una excepción no controlada en Java, la nueva clase debe heredar de `RuntimeException`. Al ser un objeto más, la excepción puede usar composición para almacenar referencias a otros objetos relevantes (como la entidad `Usuario` implicada en el error), proporcionando un contexto muy rico para la depuración en lugar de un simple mensaje de texto.

La sobrecarga de constructores permite inicializar la excepción de distintas formas. Una versión básica puede recibir solo el mensaje y el objeto implicado, mientras que otra puede recibir, además, un objeto `Throwable` para preservar la cadena de excepciones (la causa subyacente original).

```java
class Usuario {
    private String id;
    public Usuario(String id) { this.id = id; }
    public String getId() { return id; }
}

// Excepción no controlada (hereda de RuntimeException)
public class UsuarioNoEncontradoException extends RuntimeException {
    
    // Composición: la excepción almacena el contexto del error
    private final Usuario usuarioImplicado;

    // Constructor básico
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario) {
        super(mensaje);
        this.usuarioImplicado = usuario;
    }

    // Constructor sobrecargado para soportar encadenamiento de excepciones (causa)
    public UsuarioNoEncontradoException(String mensaje, Usuario usuario, Throwable causa) {
        super(mensaje, causa);
        this.usuarioImplicado = usuario;
    }

    public Usuario getUsuarioImplicado() {
        return usuarioImplicado;
    }
}
```

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

Utilizar la herencia exclusivamente por conveniencia para no duplicar código suele conducir a jerarquías ilógicas y sistemas frágiles. La herencia establece una relación semántica muy rígida del tipo "es-un". Si una clase hereda de otra solo para aprovechar un par de métodos, pero lógicamente no representa una especialización verdadera del padre, se está violando el principio de diseño, lo que a la larga generará confusión y problemas de mantenimiento.

Por ejemplo, si se necesita una clase `Coche` y existe una clase `Motor` con métodos útiles para la ignición, hacer que `Coche` herede de `Motor` solo para reutilizar ese código es conceptualmente incorrecto (un coche no "es un" motor). La opción correcta para el simple propósito de reutilización de funcionalidades aisladas suele ser incluir al objeto que provee la funcionalidad como un atributo (composición).

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?

¿Por qué favorecer la composición frente a la herencia?

**Rigidez vs. Flexibilidad:** La herencia es estática; se define al compilar y no se puede cambiar la clase padre de un objeto mientras el programa se ejecuta. La composición es dinámica; permite cambiar el comportamiento en tiempo de ejecución simplemente sustituyendo el objeto interno por otro.

**Acoplamiento:** La herencia crea un acoplamiento fuerte (una clase hija depende profundamente de la implementación de la clase padre). La composición genera un acoplamiento débil (la clase solo usa métodos a través de una interfaz).

**Mantenimiento:** La composición fomenta clases más pequeñas, cohesivas y fáciles de probar, evitando arrastrar todo el código y la complejidad de una jerarquía de herencia profunda.



Imagina un videojuego donde un personaje ataca.

1. El problema con la Herencia (Rígido)
Si usamos herencia, el comportamiento queda fijado para siempre al crear el objeto. Un Guerrero no puede decidir usar magia de repente porque su clase ya está definida.
```Java

abstract class Personaje {
    public abstract void atacar();
}

class Guerrero extends Personaje {
    @Override
    public void atacar() {
        System.out.println("Ataca con una espada.");
    }
}

// Uso:
Personaje p = new Guerrero();
p.atacar(); 
// Si ahora quiero que ataque con un arco, no puedo. 
// Tendría que crear un nuevo objeto de clase Arquero.
```

2. La solución con Composición (Flexible)
Al usar composición ("tiene-un" arma en lugar de "es-un" tipo específico de atacante), el comportamiento se puede intercambiar en plena ejecución.

```Java

// Interfaz que define el comportamiento
interface Arma {
    void usar();
}

class Espada implements Arma {
    public void usar() { System.out.println("Ataca con una espada."); }
}

class Arco implements Arma {
    public void usar() { System.out.println("Dispara una flecha."); }
}

// La clase usa la composición ("tiene-un" Arma)
class Personaje {
    private Arma arma;

    // Se inyecta el comportamiento inicial
    public Personaje(Arma arma) {
        this.arma = arma;
    }

    // ¡La magia de la composición! Se puede cambiar en tiempo de ejecución
    public void cambiarArma(Arma nuevaArma) {
        this.arma = nuevaArma;
    }

    public void atacar() {
        arma.usar(); // Delega la acción al objeto compuesto
    }
}

// Uso:
public class Main {
    public static void main(String[] args) {
        // Nace con una espada
        Personaje p = new Personaje(new Espada());
        p.atacar(); // "Ataca con una espada."

        // Cambia el comportamiento en tiempo de ejecución dinámicamente
        p.cambiarArma(new Arco());
        p.atacar(); // "Dispara una flecha."
    }
}

```

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

Esta afirmación apunta al problema conocido como "la clase base frágil". A diferencia del código cliente externo, que interactúa únicamente con una interfaz pública estricta, una subclase a menudo depende íntimamente de la implementación interna específica de su superclase (especialmente si utiliza miembros protegidos o asume que ciertos métodos padres se comportan de una manera exacta).

Si en el futuro el creador de la superclase modifica un detalle de implementación de un método base (incluso sin cambiar su firma pública), puede provocar fallos inesperados y en cascada en todas las subclases que heredaban de ella y confiaban en esa lógica oculta. Por lo tanto, la frontera de encapsulamiento entre una clase base y sus derivadas es difusa, creando un acoplamiento donde el funcionamiento de la hija está peligrosamente atado a las tripas de la madre.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

La primera alternativa utiliza la herencia para extraer las propiedades comunes a un nivel superior, forzando una jerarquía estática donde las entidades se definen ontológicamente como personas.

```java
// Alternativa 1: Herencia
class Persona {
    protected String dni;
    protected String nombre;
    
    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class EstudianteHeredado extends Persona {
    private String matricula;
    
    public EstudianteHeredado(String dni, String nombre, String matricula) {
        super(dni, nombre);
        this.matricula = matricula;
    }
}

class TrabajadorHeredado extends Persona {
    private double salario;
    
    public TrabajadorHeredado(String dni, String nombre, double salario) {
        super(dni, nombre);
        this.salario = salario;
    }
}
```

La segunda alternativa prefiere la composición, delegando la información demográfica a una entidad separada que se inyecta como dependencia. Esto permite mayor flexibilidad (por ejemplo, si un trabajador cambia legalmente de nombre, se puede actualizar un único objeto `DatosPersonales` que podría estar referenciado desde varias partes del sistema simultáneamente).

```java
// Alternativa 2: Composición
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
    public String getNombre() { return nombre; }
}

class EstudianteCompuesto {
    private DatosPersonales datos; // Composición
    private String matricula;

    public EstudianteCompuesto(DatosPersonales datos, String matricula) {
        this.datos = datos;
        this.matricula = matricula;
    }
}

class TrabajadorCompuesto {
    private DatosPersonales datos; // Composición
    private double salario;

    public TrabajadorCompuesto(DatosPersonales datos, double salario) {
        this.datos = datos;
        this.salario = salario;
    }
}
```