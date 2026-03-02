# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

La encapsulación es como un escudo que añade proteccion a la clase, la cual es un artefacto con estado y comportamiento, que puede ocultar a mimbros de la clase para:

- Garantizar que el estaod interno es siempre válido
- Evitar que otro codigo dependa o acceda a partes del codigo que no quiero
- Facilita poder cambar partes del codigo sin afectar a otras

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

La interfaz publica son los miembros que ser que ser ven desde otras clases, es decir, lo que no esta oculto


## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz publica, si se cambia, tiene mas consecuencias que si cambian partes ocultas

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Son reglas/condiciones que se cumplen durante toda la vida de las instancias de esa clase. Nos referimos habitualmente al estado interno del objeto.

Ejemplos invariantes de clase

- Saldo de cuenta bancaria >= 0
- Edad de persona >= 0
- Usuario debe tener contraseña de longitud minima 5

La ocultación del estado interno garantiza que desde el exterior, no se modifique el estado a valores que incumplen la invariable de clase


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

**Private**: Miembros solo accesibles desde el codigo de la propia clase
**Public**: Miembors accesibles desde cualquier codigo de otras clases 

```java
class Punto {
    private double x;
    private double y;

    public Punto (double x, double y){
        this.x = x;
        this.y = y;
    }

    public double CalcularDistanciaOrigen (){
        return Math.sqrt(x * y  +  x * y)
    }

    //Getter 
    public double getX() { return x; }
    public double getY() { return y; }
}
```


```java
class EjercicioEcapsulacion {
    public static void main String (args[]){
        Punto punto = new Punto (4.3, 6.7 )
    }

    sysout(punto.CalcularDistanciaOrigen)
}
```


## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

**Public**: Clases y miembros
**Private**: Clases internal y miembros (clases internas no se ven)

## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

**Protected**: Miembros accesibles desde subclases (se ve en herencia)
**Sin modificar (package-private)**: Mimebros accesibles desde otras clases del mismo paquete

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

**Opción a)**: Oculto a otras clases porque es un mecanimo de seguridad al programador

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

No usarlos porque sí

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

No, nos referimos a intentar reducir errores de programación


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

**Miembro de clase:** Asociados a la clase y compartidos por todas las instancia. En los metodos de clase no existe `this` 

Los miembros de clase se pueden ocultar.

**Miembros asociados a las instancias completas**: Asociados siempre y unicamente a una instancia

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, en ciertas ocasiones se pueden usar construcctores privados.

- Quiero crear objetos solo a traves de **metodos factoría**
- La clase solo tiene miembros de clase
- Controlar el numero de instancias que se crean


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

Se indica con `static` 

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

Vale para crear nuevas instacias y es un metodo `static`, logicamente porque no tiene instancias antes.

Esto es un caso en el que se puede tener un contrucctor privado para obligar a usar el metodo factoría.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

No, porque se pierde el control sobre las invariables de clase y se comlica mucho un cambio a futuro

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Inmutable implica que el estado de la instacia de la clase (inmutable) no cambia

Un metodo modificador es un metodo que cambia el estado interno del objeto, como por ejemplo un "setter", aunque podemos crear metodos mutables como en la clase coche el metodo `avanzar()`

Ventajas:
- Más faciles de entender lo que resulta en menos errores
- Mejor en concurrencias

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No, porque te cargas la inmutabilidad de la clase


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

String es inmutable


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

Con estes objetos como ejemplo 

```java
Libro L1 = new Libro  ("Quijote")
Libro L2 = new Libro  ("Quijote"
```

Por identidad, si tienen la misma direccion de memoria son iguales. 
En java  `if (L1 == L2)` 

Por contenido, si tienen el mismo estado son iguales.
En java usamos el metodo "equals", siempre y cuando esté implementado para comparar por contenido

`if (L1.equals(L2))`

Equals:
- Está en todas la clases
- Por defecto hace comparacion por **identidad**
- En las clases que así lo implementen, hace comparación por contenido, por ejemplo `String` 


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Se suelen ver en los lenguages Orientados a Objetos, que tienen primitivos

Clases para envolver los valores primitivos y darle ventajas de la OO

- int <=> Integer
- float <=> Float
- char <=> Character
- bool <=> Boolean

Ventajas
- Tienen métodos utiles => Integer.parseInt(String)
- Pueden ser usado en contextos donde se necesiten Objetos: Ej: `List<Integer>

Ejemplos 


```java
Integer i = 7; //Autoboxing
Integer i = new Integer(7) //Sin autoboxing

int j = i; //Autounboxing
int j = i.int.Value(); 
```


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

Es un tipo cuyos valores son finitos y conocidos de antemano 

En java un enumerado **es una clase**, pero:

- Las instancias que tiene son finitas y conocidas de antemano
- Pero tambien en una clase 



## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado.

### Respuesta


## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
