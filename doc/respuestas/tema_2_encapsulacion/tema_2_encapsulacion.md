
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La **encapsulación** busca agrupar en una misma entidad lógica (la clase) tanto los datos (estado) como las operaciones que los manipulan (comportamiento), estableciendo una frontera clara entre el componente y el exterior. Por su parte, la **ocultación de información** es el principio de diseño que restringe el acceso a los detalles internos de implementación, exponiendo únicamente aquello que es estrictamente necesario para el uso del objeto.

Entre las ventajas principales de la ocultación destacan:
1.  **Mantenibilidad**: Permite modificar la estructura interna o los algoritmos de una clase sin afectar al código de los clientes que la utilizan, siempre que se respete la interfaz pública.
2.  **Integridad**: Protege el estado del objeto de modificaciones arbitrarias o inconsistentes, asegurando que los datos siempre cumplan con las reglas de negocio definidas.
3.  **Abstracción simplificada**: Reduce la complejidad cognitiva para el desarrollador, quien sólo necesita conocer qué hace el objeto (su interfaz) y no cómo lo logra internamente.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** es el conjunto de métodos, constructores y constantes declarados con modificadores de acceso públicos (`public`) que una clase ofrece al exterior. Representa el "contrato" de uso que el objeto establece con el resto del sistema, definiendo los mensajes que es capaz de recibir y procesar.

La relación con la ocultación de información es directa y complementaria: mientras que la ocultación define qué partes del objeto son privadas e inaccesibles (la implementación), la interfaz pública define qué partes son visibles. Una buena encapsulación implica una interfaz pública mínima y estable, manteniendo ocultos la mayor cantidad posible de atributos y métodos auxiliares para evitar el acoplamiento excesivo.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Es crucial diseñar la interfaz pública con meticulosidad porque establece una dependencia fuerte entre la clase y sus consumidores. Cualquier método expuesto públicamente invita a otros programadores a utilizarlo, creando un acoplamiento que, una vez establecido en un sistema grande, es muy difícil de romper o refactorizar sin introducir errores en cascada.

Cambiar una interfaz pública no es fácil una vez que el código ha sido desplegado o compartido. Si se modifica la firma de un método público o se elimina, todo el código externo que dependía de él dejará de compilar o funcionar. Por el contrario, los cambios en la implementación interna (partes privadas) son transparentes y seguros, lo que refuerza la importancia de exponer lo mínimo indispensable.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son condiciones lógicas o restricciones sobre el estado de un objeto que deben ser siempre verdaderas durante toda su vida útil (excepto, momentáneamente, durante la ejecución de un método que esté actualizando dicho estado). Por ejemplo, en una clase `CuentaBancaria`, una invariante podría ser que "el saldo nunca puede ser negativo".

La ocultación de información es fundamental para preservar estas invariantes. Si los atributos fueran públicos (`public`), cualquier agente externo podría asignarles valores arbitrarios que violasen las reglas (ej. `cuenta.saldo = -500`), rompiendo la consistencia del objeto. Al hacer los atributos privados (`private`) y forzar el acceso a través de métodos, la clase puede validar cualquier intento de modificación y asegurar que las invariantes se mantengan intactas.

## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

```java
public class Punto {
    // Atributos ocultos (implementación)
    private double x;
    private double y;

    // Constructor (parte de la interfaz pública)
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    // Método (parte de la interfaz pública)
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
    
    // Getters (parte de la interfaz pública) para leer valores
    public double getX() { return x; }
    public double getY() { return y; }
}

```

La interfaz pública de esta clase `Punto` está compuesta por el constructor `Punto(double, double)`, el método `calcularDistanciaAOrigen()` y los métodos `getX()` e `getY()`. El modificador **`public`** indica que el miembro es accesible desde cualquier parte del programa. **`private`** significa que el miembro sólo es accesible desde el código que reside dentro de la propia clase `Punto`.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de acceso se pueden aplicar a diversos elementos, pero con restricciones según el contexto. Se pueden aplicar a **miembros de una clase**, que incluyen atributos (variables de instancia y de clase), métodos, constructores y clases anidadas (inner classes).

También se aplican a las **clases** en sí mismas (nivel superior), pero con limitaciones: una clase de nivel superior solo puede ser `public` o tener visibilidad de paquete (sin modificador). No puede ser declarada como `private` o `protected` (a menos que sea una clase interna). Las variables locales dentro de un método no admiten modificadores de acceso.

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Sí, existen niveles intermedios. En **Java**, además de `public` y `private`, existen dos visibilidades más: **`protected`** (visible para el mismo paquete y subclases en otros paquetes) y **paquete/default** (cuando no se escribe nada, visible solo para clases del mismo paquete).

En otros lenguajes varía. **C#** tiene `internal` (similar a paquete) y combinaciones como `protected internal`. **C++** utiliza `public`, `private` y `protected` (donde protected es solo para herencia y amigos, no paquetes). **Python** no tiene privacidad estricta, usando convenciones de nombres (guion bajo `_` para protegido, doble `__` para privado mediante *name mangling*). **Ruby** hace todos los atributos privados por defecto y los métodos públicos, salvo especificación contraria.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Están ocultos para **(a) otras clases**. En Java y la mayoría de lenguajes de POO, el control de acceso es a nivel de **clase**, no a nivel de objeto. Esto significa que una instancia de `Punto` puede acceder a los miembros privados de *otra* instancia de `Punto`.

```java
    // Método dentro de la clase Punto
    public double calcularDistanciaAPunto(Punto otro) {
        // Podemos acceder a 'otro.x' y 'otro.y' DIRECTAMENTE aunque sean private
        // porque estamos dentro de la definición de la clase Punto.
        double dx = this.x - otro.x; 
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }

```

Si la privacidad fuera por instancia, el código anterior fallaría al intentar leer `otro.x`. Como es por clase, es perfectamente válido.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Los métodos **"getter"** (accesores) y **"setter"** (mutadores) son métodos públicos estandarizados cuya función es leer y modificar, respectivamente, el valor de un atributo privado. Permiten controlar el acceso a los datos, sirviendo como guardianes entre el exterior y el estado interno.

El uso de estos métodos permite añadir lógica adicional en el futuro sin cambiar la interfaz pública. Por ejemplo, un *setter* puede incluir validaciones para asegurar invariantes (ej. impedir asignar una edad negativa), o notificar a otros objetos de un cambio. Un *getter* puede devolver una copia de un objeto mutable o un valor calculado en tiempo real, en lugar del atributo directo.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, en el contexto de la ingeniería de software, "seguridad" no se refiere a la invulnerabilidad contra ataques maliciosos o hacking (ya que la memoria siempre puede inspeccionarse con herramientas adecuadas o reflexión). Se refiere a la **seguridad lógica** o **robustez** del código.

La ocultación previene el uso incorrecto accidental de la clase por parte de otros desarrolladores. Evita que el programa entre en estados inconsistentes o inválidos debido a manipulaciones directas de variables que deberían estar controladas. Es una protección contra errores de programación y efectos colaterales no deseados, no contra ciberataques.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un **miembro de instancia** pertenece a un objeto concreto; cada objeto tiene su propia copia de los atributos y los métodos operan sobre ese estado específico. Un **miembro de clase** (marcado con `static` en Java) pertenece a la clase en sí y es compartido por todas las instancias; existe una única copia en memoria.

Sí, los miembros de clase también pueden y deben ocultarse si es necesario. Un atributo estático puede ser `private static` para almacenar datos compartidos de uso interno (como un contador, una caché o una constante de configuración interna) que no deben ser expuestos ni modificados desde fuera de la clase.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene mucho sentido en patrones de diseño específicos. Un constructor privado impide que otras clases instancien objetos de esa clase arbitrariamente mediante `new`.

Esto es fundamental para:

1. **Clases de utilidad**: Como `java.lang.Math`, que son solo colecciones de métodos estáticos y no deben instanciarse nunca.
2. **Patrón Singleton**: Donde se garantiza que solo exista una única instancia de la clase en todo el programa, controlando su creación mediante un método estático.
3. **Factory Methods**: Donde se obliga a usar métodos estáticos específicos para crear objetos, permitiendo mayor control sobre la creación (ej. validaciones complejas o retorno de subclases).

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Se indican con la palabra reservada **`static`**.

```java
public class Punto {
    private double x, y;
    
    // Miembros de clase (static) para rastrear máximos globales
    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
        // Actualizamos los valores estáticos si es necesario
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
    
    // Método estático para consultar la información de clase
    public static double getMaxXGlobal() { return maxX; }
    public static double getMaxYGlobal() { return maxY; }
}

```

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`?

```java
    public static Punto crearPuntoRedondeado(double x, double y) {
        double xRedondeada = Math.round(x);
        double yRedondeada = Math.round(y);
        return new Punto(xRedondeada, yRedondeada);
    }

```

Sí, es imprescindible usar **`static`**. Al ser un método factoría que crea una instancia nueva, debe poder llamarse sin tener un objeto previo (`Punto.crearPuntoRedondeado(...)`). Si no fuera estático, necesitaríamos una instancia de Punto para poder llamar al método que crea Puntos, lo cual sería paradójico.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

```java
public class Punto {
    // Cambio radical de implementación: ahora es un array
    private double[] coordenadas; 

    public Punto(double x, double y) {
        coordenadas = new double[2];
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    // La interfaz pública (getters) NO cambia su firma
    public double getX() { 
        return coordenadas[0]; 
    }

    public double getY() { 
        return coordenadas[1]; 
    }
    
    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coordenadas[0]*coordenadas[0] + coordenadas[1]*coordenadas[1]);
    }
}

```

Al haber usado encapsulamiento, el código externo que llama a `p.getX()` sigue funcionando exactamente igual, demostrando la potencia de separar interfaz de implementación.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No es mejor declararlo público. La convención casi universal en Java es que los atributos sean **privados** (o protegidos si es necesario por herencia). Aunque inicialmente el *getter* y *setter* solo lean y escriban, encapsularlos permite añadir lógica futura (validación, notificación de cambios, sincronización de hilos) sin romper la compatibilidad binaria ni de código.

Tiene todo que ver con las **invariantes de clase**. Si haces el atributo público, pierdes el control sobre los valores que se le asignan. Con un *setter*, mantienes el punto de control único para asegurar que nunca se viole una invariante (por ejemplo, asegurar que un denominador nunca sea cero).

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es **inmutable** si el estado de sus instancias no puede cambiar una vez han sido creadas. Para lograrlo, todos sus atributos suelen ser privados y finales, y no se ofrecen métodos que alteren dichos atributos. Un **método modificador** es aquel que cambia el estado interno del objeto.

No siempre es un "setter" (ej. `setNombre`). Un método como `avanzar()` en un coche o `depositar()` en una cuenta son modificadores porque alteran atributos internos. Las ventajas de la inmutabilidad son enormes: son automáticamente seguros en entornos concurrentes (thread-safe), son más sencillos de razonar, pueden ser cacheados sin riesgo y son ideales como claves en mapas hash.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No. Es una mala práctica generar automáticamente *getters* y *setters* para todos los atributos sin reflexión previa. Esto rompe el encapsulamiento al exponer detalles de implementación y a menudo conduce a "clases anémicas" (solo datos, sin comportamiento).

Se deben incluir *setters* solo si el objeto es mutable y tiene sentido semántico modificar esa propiedad tras la construcción. Si un dato no debe cambiar (como la fecha de nacimiento de una persona o el ID de una transacción), no debe tener *setter*. Se debe favorecer la inmutabilidad siempre que sea posible.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase `String` en Java es **inmutable**. Cuando se concatenan dos cadenas (`"a" + "b"`), no se modifica la original, sino que se crea un **nuevo objeto** `String` en memoria con el resultado, descartando los anteriores si no se usan.

Si se realizan muchas concatenaciones en bucle, se genera una gran cantidad de objetos temporales, afectando gravemente al rendimiento y la memoria. Para estos casos, se debe utilizar la clase **`StringBuilder`** (o `StringBuffer` en contextos concurrentes), que es mutable y permite construir la cadena modificando un buffer interno sin crear múltiples objetos intermedios.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java?

Depende del operador usado. El operador `==` compara la **identidad** (referencia de memoria): devuelve `true` solo si las dos variables apuntan exactamente al mismo objeto.

El método `equals(Object o)` se utiliza para comparar la igualdad semántica o de **contenido**. Por defecto, en la clase `Object`, `equals` se comporta igual que `==` (compara referencias). Sin embargo, las clases deben sobrescribirlo para definir cuándo dos objetos son "iguales" según sus atributos. Las cadenas en Java (`String`) **siempre** deben compararse con `.equals()`, ya que queremos saber si contienen el mismo texto, no si son el mismo objeto en memoria.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers?

Las clases **wrapper** (envoltorios) en Java (como `Integer`, `Double`, `Boolean`) son clases que encapsulan un tipo primitivo (`int`, `double`, `boolean`) dentro de un objeto. Esto es necesario porque en Java los tipos primitivos no son objetos y algunas estructuras (como las colecciones `ArrayList` o `HashMap`) solo trabajan con objetos.

El proceso de conversión es automático en Java moderno (desde versión 5) y se llama **Autoboxing** (de primitivo a wrapper) y **Unboxing** (de wrapper a primitivo). La ventaja es que permiten tratar datos simples como objetos. No todos los lenguajes los necesitan; lenguajes como Ruby o Python tratan todo como objetos desde el principio, por lo que no existe esa dicotomía entre primitivo y objeto ni la necesidad de wrappers.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Un **enumerado** es un tipo de dato especial que define un conjunto fijo y limitado de constantes con nombre. Sirve para representar variables que solo pueden tomar uno de entre varios valores predefinidos (ej. Días de la semana, Palos de la baraja).

En Java, un `enum` **es una clase** completa (hereda de `java.lang.Enum`). Esto ofrece ventajas enormes de encapsulación frente a usar constantes enteras (`public static final int`): proporcionan seguridad de tipos (no puedes pasar un valor inválido), pueden tener sus propios atributos, constructores y métodos, y pueden encapsular comportamiento específico para cada constante.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`).

```java
public enum Mes {
    ENERO(31), FEBRERO(28), MARZO(31), ABRIL(30), MAYO(31), JUNIO(30),
    JULIO(31), AGOSTO(31), SEPTIEMBRE(30), OCTUBRE(31), NOVIEMBRE(30), DICIEMBRE(31);

    // Atributo encapsulado e inmutable
    private final int dias;

    // Constructor privado (sólo invocado por las constantes del enum)
    private Mes(int dias) {
        this.dias = dias;
    }

    // Métodos de acceso
    public int getDias() {
        return dias;
    }

    public int getOrdinal1a12() {
        return this.ordinal() + 1; // ordinal() es nativo y empieza en 0
    }

    // Lógica de estaciones (simplificada: solapamiento de meses)
    // Invierno Norte: Dic, Ene, Feb, Mar (parte)
    public boolean esDeInvierno(boolean enHemisferioNorte) {
        return enHemisferioNorte 
             ? (this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO)
             : (this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE);
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        return enHemisferioNorte
             ? (this == MARZO || this == ABRIL || this == MAYO || this == JUNIO)
             : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE);
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        return enHemisferioNorte
             ? (this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE)
             : (this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO);
    }

    public boolean esDeOtono(boolean enHemisferioNorte) {
        return enHemisferioNorte
             ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE)
             : (this == MARZO || this == ABRIL || this == MAYO || this == JUNIO);
    }
}
```