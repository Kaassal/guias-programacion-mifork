# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El polimorfismo es una de las características fundamentales de la Programación Orientada a Objetos que permite tratar objetos de distintas clases derivadas como si fueran objetos de una misma clase genérica base. Su propósito principal es desacoplar el código cliente de las implementaciones específicas, permitiendo escribir sistemas más flexibles y extensibles, ya que una única instrucción puede provocar comportamientos distintos dependiendo del objeto real al que se apunte.

La **sobreescritura** de métodos (o *overriding*) es el mecanismo principal para lograr este polimorfismo en tiempo de ejecución. Consiste en que una subclase proporciona una nueva implementación específica para un método que ya ha sido definido en su superclase. Cuando se invoca ese método a través de una referencia genérica, se ejecuta la versión sobreescrita perteneciente a la instancia real alojada en memoria.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La **ligadura dinámica** (o enlace tardío) es el mecanismo interno que hace posible el polimorfismo. Consiste en diferir la resolución de la llamada a un método hasta el momento en que el programa se está ejecutando (*runtime*). En lugar de que el compilador decida estáticamente qué código ejecutar basándose en el tipo de la referencia, el sistema busca el tipo real del objeto instanciado en memoria y ejecuta su método correspondiente.

La necesidad de indicar esta característica de forma explícita depende enteramente de las decisiones de diseño de cada lenguaje. En **C++**, el enlace de los métodos es temprano (estático) por defecto para maximizar el rendimiento. Para habilitar el enlace tardío y el polimorfismo, el desarrollador debe marcar explícitamente el método de la clase base con la palabra clave `virtual`. 

En **Java**, el enfoque es el opuesto: todos los métodos de instancia (que no sean privados ni finales) emplean ligadura dinámica por defecto, lo que fomenta y simplifica el diseño polimórfico sin necesidad de añadir palabras clave adicionales. En lenguajes de tipado dinámico como **Python**, la resolución de métodos ocurre siempre mediante enlace tardío de forma natural, dado que las variables no poseen un tipo rígido (*duck typing*) y la existencia del método se evalúa en el mismo instante de la ejecución.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

La aplicación del polimorfismo se ilustra claramente al operar sobre colecciones de objetos que comparten un mismo tipo base. Esto posibilita tratar referencias genéricas de forma uniforme, delegando en la máquina virtual la tarea de enrutar cada invocación hacia la implementación correcta de manera automática.

En el siguiente ejemplo, un único bucle interacciona con referencias de la clase genérica para solicitar un saludo. Gracias a la sobreescritura de métodos, aunque la instrucción léxica es idéntica para todos los elementos, cada subtipo en la memoria ejecuta su comportamiento especializado.

```java
class Soldado {
    public void saludar() {
        System.out.println("¡Señor, sí señor!");
    }
}
```

```java
class Zapador extends Soldado {
    @Override 
    public void saludar() {
        System.out.println("¡Zapador preparado para detonar!");
    }
}
```

```java
class Artillero extends Soldado {
    @Override
    public void saludar() {
        System.out.println("¡Artillero listo con la munición pesada!");
    }
}
```
```java
public class Main {
    public static void main(String[] args) {
        // Array de referencias de la clase base genérica
        Soldado[] peloton = new Soldado[3];
        peloton[0] = new Soldado();
        peloton[1] = new Zapador();
        peloton[2] = new Artillero();

        // Resolución polimórfica: la misma llamada produce distintos resultados
        for (Soldado s : peloton) {
            s.saludar(); 
        }
    }
}
```

> Con `@Override` indicamos que no se va a ejecutar el metodo del mismo nombre de la clase padre 

## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Es completamente posible y habitual invocar la implementación original de un método definido en la clase base desde el método que lo está sobreescribiendo en la subclase. Esto resulta sumamente útil para extender el comportamiento original en lugar de reemplazarlo por completo, añadiendo instrucciones específicas tras ejecutar la lógica preexistente.

Para lograr este cometido se utiliza la palabra clave `super`. Esta palabra reservada actúa como una referencia explícita a la porción del objeto correspondiente a la superclase, indicando al compilador que ignore la sobreescritura actual y ejecute directamente el código del nivel inmediatamente superior de la jerarquía.

```java
class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Se invoca el comportamiento base original
        super.saludar();
        // Se añade el comportamiento específico
        System.out.println("¡ZAPADOR A SUS ORDENES!");
    }
}
```

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobreescribir un método en Java, la firma del método en la subclase debe ser exactamente igual a la de la superclase: el mismo nombre, así como el mismo número, tipo y orden de los parámetros. El tipo de retorno debe coincidir exactamente o ser un subtipo del tipo de retorno original (tipos de retorno covariantes). Adicionalmente, el método sobreescrito no puede reducir el nivel de visibilidad (por ejemplo, pasar de `public` a `protected`), aunque sí se permite aumentarlo.

La diferencia conceptual respecto a la sobrecarga es crucial. La sobrecarga (*overloading*) consiste en tener múltiples métodos con el mismo nombre en la misma clase, distinguiéndose exclusivamente por tener parámetros distintos (diferente firma); es un polimorfismo estático resuelto en compilación. La sobreescritura (*overriding*), por el contrario, requiere de herencia, mantiene firmas idénticas, y su invocación se resuelve en tiempo de ejecución mediante ligadura dinámica.

La anotación `@Override` se coloca sobre el método que redefine el comportamiento base. Su propósito es ordenar al compilador que verifique de forma estricta que existe un método idéntico en la clase padre listo para ser sobreescrito. Resulta altamente recomendable porque previene errores invisibles: si se comete un error tipográfico en el nombre del método o en sus parámetros, el compilador emitirá un fallo, evitando que se cree accidentalmente un método sobrecargado nuevo en lugar de sobreescribir el deseado.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, en la arquitectura de Java el polimorfismo se aplica de manera ubicua desde los primeros desarrollos. Dado que todas las clases escritas en el lenguaje heredan de forma implícita de la superclase raíz `java.lang.Object`, todos los objetos creados participan automáticamente en una gran jerarquía polimórfica, compartiendo una interfaz común.

Al redefinir comportamientos estándar como `toString()` o `equals(Object)`, se hace uso directo de la sobreescritura y de la ligadura dinámica. Componentes de la API estándar de Java, como el método de impresión en consola (`System.out.println`), esperan una referencia genérica de tipo `Object`. Al recibir un objeto personalizado y llamar a su método `.toString()`, el polimorfismo asegura que se ejecute la versión específica codificada para esa clase, sin que el sistema original necesite ser modificado.

## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una **clase abstracta** es una plantilla base diseñada explícitamente para quedar incompleta, actuando únicamente como un molde conceptual para organizar jerarquías de herencia. Se prohíbe categóricamente la creación de instancias de esta clase mediante el operador `new`; su única utilidad es proveer atributos y comportamientos comunes a otras clases que heredarán de ella.

Un **método abstracto** es un método que carece de implementación o cuerpo, terminando en un punto y coma. Su función es establecer un contrato estricto, obligando a todas las subclases derivadas concretas a proveer la lógica faltante. Para utilizar esta característica, se debe colocar la palabra clave `abstract` tanto en la declaración del método como en la declaración de la propia clase.

```java
// La clase completa se marca como abstract porque alberga métodos incompletos
abstract class Soldado {
    public void saludar() {
        System.out.println("¡Señor, sí señor!");
    }
    
    // Método abstracto: impone la firma pero no la lógica
    public abstract void atacar();
}

class Artillero extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Lanzando cohete explosivo...");
    }
}

class Zapador extends Soldado {
    @Override
    public void atacar() {
        System.out.println("Cavando y armando mina terrestre...");
    }
}
```

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave `final` en Java actúa como un mecanismo restrictivo para detener el flujo de la herencia y la redefinición. Al aplicarse sobre un método, se prohíbe explícitamente que cualquier subclase pueda sobreescribirlo (*override*). Si se aplica sobre la declaración de una clase entera, se vuelve completamente imposible heredar de ella, finalizando el árbol de jerarquía en ese punto.

Esta restricción está íntimamente ligada al diseño de sistemas polimórficos, ya que permite al desarrollador cerrar ciertas vías de flexibilidad para proteger la integridad lógica del sistema. Establecer un método como final elimina la incertidumbre de la ligadura dinámica, garantizando que dicho comportamiento se ejecutará idénticamente sin importar el subtipo del objeto invocado.

Un ejemplo emblemático de esto en la API estándar de Java es la clase `java.lang.String`. Esta clase se diseñó como `final` para garantizar la absoluta inmutabilidad de las cadenas de texto, previniendo así vulnerabilidades de seguridad donde una subclase maliciosa pudiera alterar el comportamiento predecible del manejo de textos en toda la máquina virtual.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Las **interfaces** en Java son herramientas de diseño utilizadas para definir contratos de comportamiento puramente abstractos. Estructuralmente, se pueden comprender como el extremo más radical de una clase abstracta: tradicionalmente solo contienen métodos públicos que carecen de cuerpo (completamente abstractos) y atributos que son siempre constantes (`public static final`). No se les permite albergar estado de instancia mutables ni métodos lógicos, definiendo exclusivamente qué acciones se deben ejecutar, pero no cómo.

A diferencia de la herencia tradicional de clases en Java, que restringe a una subclase a derivar de un único padre (`extends`), las interfaces aportan enorme flexibilidad al permitir la implementación múltiple. Una única clase puede adherirse o implementar (`implements`) tantas interfaces como necesite de forma simultánea, adquiriendo varios contratos polimórficos sin entrar en ambigüedades estructurales de estado.

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

Para materializar este diseño, la clase base actúa como una abstracción pura que promete una funcionalidad de cálculo de distancia. Cada subclase implementa la fórmula matemática correspondiente, salvaguardando la integridad de los datos empleando `instanceof` para asegurarse de extraer y castear adecuadamente los ejes dimensionales necesarios que el objeto abstracto no posee explícitamente.

En la arquitectura resultante, la entidad encargada de representar la línea queda completamente abstraída y desacoplada de la dimensionalidad de las coordenadas. Al requerir la longitud, el mecanismo de enlace dinámico intercepta la solicitud y la dirige hacia la función geométrica específica del espacio 2D o 3D pertinente, todo ello sin utilizar sentencias condicionales en la clase contenedora.

```java
abstract class Punto {
    public abstract double calcularDistanciaA(Punto otro);
}
```

```java
class Punto2D extends Punto {
    private double x, y;
    public Punto2D(double x, double y) { this.x = x; this.y = y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro; // Downcasting seguro
            return Math.sqrt(Math.pow(p2.x - this.x, 2) + Math.pow(p2.y - this.y, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 2D para este cálculo");
    }
}
```

```java
class Punto3D extends Punto {
    private double x, y, z;
    public Punto3D(double x, double y, double z) { this.x = x; this.y = y; this.z = z; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        if (otro instanceof Punto3D) {
            Punto3D p3 = (Punto3D) otro; // Downcasting seguro
            return Math.sqrt(Math.pow(p3.x - this.x, 2) + 
                             Math.pow(p3.y - this.y, 2) + 
                             Math.pow(p3.z - this.z, 2));
        }
        throw new IllegalArgumentException("El punto debe ser 3D para este cálculo");
    }
}
```

```java
class Linea {
    private Punto origen;
    private Punto destino;

    public Linea(Punto origen, Punto destino) {
        this.origen = origen;
        this.destino = destino;
    }

    public double calcularLongitud() {
        // El polimorfismo se encarga de determinar qué método utilizar
        return origen.calcularDistanciaA(destino);
    }
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La **herencia de interfaces** es un mecanismo jerárquico mediante el cual una interfaz adopta y amplía el conjunto de reglas dictado por una interfaz base. Para formalizar esta relación se utiliza la palabra clave `extends`, lo que implica que cualquier clase concreta que se comprometa con la interfaz hija estará lógicamente obligada a proveer la implementación de todos los métodos declarados en la totalidad del árbol de la interfaz.

Dado que las interfaces operan como puros compromisos abstractos libres del almacenamiento de estado u otras implementaciones problemáticas, en Java **sí se soporta la herencia múltiple de interfaces**. De este modo, una interfaz avanzada puede unificar y extender simultáneamente los contratos de múltiples interfaces base, separándolas mediante comas.

```java
// Interfaz base o genérica
interface Fichero {
    String leerContenido();
}

// Interfaz extendida que hereda el contrato anterior
interface FicheroEscribible extends Fichero {
    void escribirContenido(String datos);
    void eliminar();
}

// Una clase concreta debe satisfacer todo el árbol de contratos
class DocumentoDeTexto implements FicheroEscribible {
    @Override
    public String leerContenido() { 
        return "Contenido actual del documento..."; 
    }
    
    @Override
    public void escribirContenido(String datos) { 
        System.out.println("Guardando información: " + datos); 
    }
    
    @Override
    public void eliminar() { 
        System.out.println("Fichero trasladado a la papelera."); 
    }
}
```