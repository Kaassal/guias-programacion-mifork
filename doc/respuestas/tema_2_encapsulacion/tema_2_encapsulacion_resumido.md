# TEMA 2. Encapsulación (Resumido)

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La **encapsulación** agrupa en una misma entidad lógica (la clase) los datos (estado) y las operaciones que los manipulan (comportamiento). La **ocultación de información** complementa esto restringiendo el acceso a los detalles internos de implementación.

Sus principales ventajas son:
* **Mantenibilidad**: Permite modificar la lógica interna sin romper el código externo que usa la clase.
* **Integridad**: Protege el estado del objeto de cambios inválidos, garantizando las reglas de negocio.
* **Abstracción**: Simplifica el uso de la clase, exponiendo solo lo que el desarrollador necesita saber.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La **interfaz pública** es el conjunto de miembros (métodos, constructores) declarados como `public` que la clase ofrece al exterior. Es el "contrato" de uso del objeto. Se relaciona con la ocultación porque define la única vía de comunicación permitida; una buena encapsulación maximiza la ocultación (privado) y minimiza la interfaz pública para reducir dependencias.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

Es crucial porque la interfaz pública genera un fuerte acoplamiento con el código cliente. Cambiarla (alterar firmas o eliminar métodos) no es fácil una vez el código está en uso, ya que provocará errores en cascada en todas las partes del sistema que dependan de ella. Los cambios internos (privados), en cambio, son seguros.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las **invariantes de clase** son reglas o condiciones lógicas sobre el estado del objeto que deben ser siempre verdaderas (ej. un saldo bancario nunca puede ser negativo). La ocultación ayuda porque, al hacer los atributos privados, se evita que código externo asigne valores directamente que violen estas reglas, obligando a pasar por métodos que incluyan validaciones.

## 5. Pon un ejemplo de una clase `Punto` en `Java`... ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

* **`private`**: El miembro solo es accesible desde el código de la propia clase (ocultación).
* **`public`**: El miembro es accesible desde cualquier parte del programa (interfaz).

```java
public class Punto {
    // Implementación oculta
    private double x;
    private double y;

    // Interfaz pública
    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    public double getX() { return x; }
    public double getY() { return y; }
}
```

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

Se aplican principalmente a los **miembros de la clase** (atributos, métodos, constructores, clases internas). También se aplican a **clases de nivel superior**, pero estas solo pueden ser `public` o tener visibilidad de paquete (no pueden ser `private` ni `protected`).

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

Sí. En **Java** existen `protected` (accesible para subclases y clases del mismo paquete) y la visibilidad por defecto o *package-private* (accesible solo dentro del mismo paquete). Otros lenguajes varían: C# usa `internal`, y lenguajes dinámicos como Python usan convenciones de nomenclatura (como prefijos de guion bajo `_`) en lugar de restricciones estrictas del compilador.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo...

Están ocultos para **(a) otras clases**. En Java, el control de acceso opera a nivel de clase, no de objeto. Por tanto, una instancia puede acceder directamente a los atributos privados de otra instancia siempre que ambas sean de la misma clase.

```java
    public double calcularDistanciaAPunto(Punto otro) {
        // Acceso directo y legal a atributos privados de la instancia 'otro'
        double dx = this.x - otro.x; 
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
```

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

Son métodos públicos estándar usados para leer (accesores / *getters*) y modificar (mutadores / *setters*) el valor de un atributo privado. Actúan como una barrera protectora, permitiendo añadir lógica de validación o transformación en el futuro sin alterar la forma en que otras clases interactúan con el dato.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No. En este contexto, "seguridad" hace referencia a la **robustez lógica** del software. Significa prevenir que otros desarrolladores usen la clase de forma incorrecta por accidente, evitando corrupciones del estado interno o errores de ejecución (bugs), no previniendo ataques maliciosos externos.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

* **Miembro de instancia**: Pertenece exclusivamente a un objeto concreto. Cada objeto tiene su propia copia del dato.
* **Miembro de clase (`static`)**: Pertenece a la clase en general y es compartido por todas sus instancias (solo hay una copia en memoria).
Sí, los miembros de clase pueden y deben ocultarse (`private static`) si almacenan datos de uso puramente interno para la clase.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, es útil en patrones de diseño. Sirve para evitar la instanciación libre mediante `new`. Se usa en **clases de utilidad** (solo contienen métodos estáticos), en el patrón **Singleton** (garantizar una única instancia), o para obligar al uso de **métodos factoría**.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo...

Se indican con el modificador **`static`**.

```java
public class Punto {
    private double x, y;
    
    // Miembros de clase para rastrear máximos globales
    private static double maxX = Double.MIN_VALUE;
    private static double maxY = Double.MIN_VALUE;

    public Punto(double x, double y) {
        this.x = x; this.y = y;
        if (x > maxX) maxX = x;
        if (y > maxY) maxY = y;
    }
    
    public static double getMaxXGlobal() { return maxX; }
    public static double getMaxYGlobal() { return maxY; }
}
```

## 14. Como sería un método factoría dentro de la clase `Punto`... ¿Has usado `static`?

```java
    public static Punto crearPuntoRedondeado(double x, double y) {
        return new Punto(Math.round(x), Math.round(y));
    }
```
Sí, el uso de **`static`** es obligatorio. Al ser un método encargado de crear un objeto, debe poder invocarse sin necesidad de tener una instancia previa creada.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

```java
public class Punto {
    // La implementación interna cambia a un array
    private double[] coordenadas = new double[2]; 

    public Punto(double x, double y) {
        coordenadas[0] = x;
        coordenadas[1] = y;
    }

    // La interfaz pública se mantiene intacta
    public double getX() { return coordenadas[0]; }
    public double getY() { return coordenadas[1]; }
}
```

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Tiene esto algo que ver con las "invariantes de clase"?

No es mejor. Declararlo público hace perder el control absoluto sobre las modificaciones. Mantenerlo privado y usar un *setter* conserva un punto de control centralizado donde se puede validar la entrada, garantizando así que no se rompan las **invariantes de clase**.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

* **Inmutable**: El estado de sus instancias no puede cambiar una vez construidas.
* **Método modificador**: Cualquier método que altere el estado interno. No siempre es un "setter" tradicional (ej. `avanzar()` o `depositar()`).
* **Ventajas**: Son seguras en entornos de concurrencia (hilos), más fáciles de predecir, reducen bugs y pueden ser usadas como claves seguras en mapas.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No. Generar *setters* por defecto rompe la encapsulación al exponer todo el estado interno. Solo deben incluirse si el objeto necesita ser semánticamente mutable tras su creación. Se debe favorecer la inmutabilidad siempre que sea posible.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas?

La clase `String` es estrictamente **inmutable**. Al concatenar dos cadenas, no se altera la original, sino que se reserva memoria para un nuevo objeto `String` con el resultado. Para concatenaciones intensivas en bucles, se debe usar **`StringBuilder`**, que sí es mutable y optimiza el uso de memoria.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java?

* **Identidad (`==`)**: Compara si dos variables apuntan a la misma dirección de memoria.
* **Contenido (`.equals()`)**: Compara si los estados internos de los objetos son semánticamente equivalentes. Por defecto, `equals` en la clase base `Object` actúa igual que `==`. Las clases deben sobrescribirlo para comparar por atributos. Para cadenas de texto (`String`), se debe usar siempre `.equals()`.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático?

Son clases (como `Integer`, `Double`, `Boolean`) que "envuelven" tipos primitivos (`int`, `double`) para tratarlos como objetos reales. Esto permite usarlos en estructuras que solo admiten objetos, como las colecciones (`List<Integer>`). En Java, el proceso de conversión entre primitivo y objeto es automático y se denomina **Autoboxing** y **Unboxing**.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Es un tipo que representa un conjunto fijo, finito y conocido de valores. En Java, un `enum` **es una clase completa**. Su gran ventaja de encapsulación es que ofrece seguridad de tipos (el compilador impide usar valores inválidos), y al ser una clase, puede encapsular sus propios atributos, constructores y métodos específicos para cada constante.

## 23 y 24. Crea un tipo enumerado `Mes` con días, ordinales, y métodos para las estaciones del año.

```java
public enum Mes {
    ENERO(31), FEBRERO(28), MARZO(31), ABRIL(30), MAYO(31), JUNIO(30),
    JULIO(31), AGOSTO(31), SEPTIEMBRE(30), OCTUBRE(31), NOVIEMBRE(30), DICIEMBRE(31);

    private final int dias;

    private Mes(int dias) {
        this.dias = dias;
    }

    public int getDias() { return dias; }
    public int getOrdinal1a12() { return this.ordinal() + 1; }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
        } else {
            return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
        }
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
        } else {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
        }
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
        } else {
            return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
        }
    }

    public boolean esDeOtono(boolean enHemisferioNorte) {
        if (enHemisferioNorte) {
            return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
        } else {
            return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
        }
    }
}
```