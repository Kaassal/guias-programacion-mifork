# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

En lenguajes sin un sistema de genericidad fuerte o en sus versiones más antiguas, la forma de almacenar múltiples tipos de datos en una misma estructura consistía en utilizar el tipo más abstracto o genérico disponible. En Java, como todas las clases heredan implícitamente de la clase `Object`, un array de tipo `Object[]` tiene la capacidad de almacenar referencias a cualquier instancia, independientemente de su clase específica. 

En el caso de C, al carecer de orientación a objetos y de una raíz de herencia común, se recurría a punteros genéricos, concretamente `void*`. Un array de `void*` permite almacenar direcciones de memoria de cualquier tipo de dato, delegando en el programador la responsabilidad de recordar qué hay almacenado en cada posición y cómo interpretar esos bytes.

El siguiente ejemplo en Java ilustra una estructura básica que aloja elementos de cualquier tipo:

```java
public class ContenedorBasico {
    private Object[] elementos;
    private int tamaño;

    public ContenedorBasico(int capacidad) {
        this.elementos = new Object[capacidad];
        this.tamaño = 0;
    }

    public void añadir(Object elemento) {
        if (tamaño < elementos.length) {
            elementos[tamaño] = elemento;
            tamaño++;
        }
    }

    public Object obtener(int indice) {
        return elementos[indice];
    }
}
```

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La programación genérica es un paradigma que permite escribir algoritmos y estructuras de datos especificando los tipos exactos de datos sobre los que operan en un momento posterior (en la instanciación), en lugar de fijarlos durante la definición de la clase o método. El objetivo es maximizar la reutilización del código manteniendo un control de tipos estricto y seguro durante la fase de compilación.

El ejemplo anterior que utiliza `Object` (o `void*` en C) **no se considera verdadera programación genérica**. Se trata simplemente de un uso del polimorfismo de herencia (subtipado) para evadir la restricción de tipos. Al aceptar `Object`, la estructura acepta cualquier cosa, pero pierde por completo la información sobre el tipo original de los elementos, lo cual es contrario al propósito de seguridad de tipos que persigue la programación genérica.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El problema fundamental de emplear tipos abstractos universales como `Object` o `void*` es la pérdida total de la seguridad de tipos (*type safety*). Cuando se extrae un elemento de dicha estructura, el compilador solo sabe que es un `Object` (o una dirección de memoria sin forma en C). Por lo tanto, no se puede invocar ningún método específico del objeto original sin antes forzar una conversión de tipo explícita (*downcasting*).

Esto traslada la responsabilidad de la verificación de tipos desde el tiempo de compilación (donde los errores son evidentes y fáciles de corregir) al tiempo de ejecución. Si se introduce un `String` en la estructura, pero más tarde se comete el error de intentar extraerlo y convertirlo a `Integer`, el compilador no mostrará ninguna advertencia. 

En Java, esto provocará una excepción `ClassCastException` que detendrá el programa abruptamente. En C, extraer un puntero y castearlo a un tipo incorrecto provocará la lectura de memoria de forma errónea, resultando a menudo en un fallo de segmentación (*segmentation fault*) o, peor aún, en comportamientos indefinidos y corrupción silenciosa de datos.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los parámetros de tipo son identificadores (generalmente letras mayúsculas como `T`, `E`, `K`, `V`) que actúan como "variables para tipos". Al igual que un método recibe variables que contienen valores de datos, una clase o método genérico recibe parámetros de tipo que serán sustituidos por tipos de datos reales (como `String`, `Integer`, o una clase personalizada) cuando se utilicen.

La introducción de estos parámetros permite al compilador conocer exactamente qué tipo de objeto se está manejando dentro de una instancia particular de una clase. Gracias a esto, el compilador puede verificar que solo se inserten objetos compatibles y permite extraer los datos sin necesidad de realizar conversiones manuales (*downcasting*), garantizando la seguridad en tiempo de compilación.

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

En ambos lenguajes, la programación genérica se manifiesta especificando el tipo concreto entre corchetes angulares `< >` al momento de crear la estructura. Esto configura la colección para que rechace cualquier tipo que no coincida.

A continuación se muestra el ejemplo en Java utilizando la clase `ArrayList`, y en C++ utilizando `std::vector`. En ambos casos, no es necesario hacer un *cast* al extraer los datos, ya que el sistema asegura desde la compilación que todos los elementos son cadenas de texto.

**En Java:**
```java
import java.util.ArrayList;
import java.util.List;

public class EjemploJava {
    public static void main(String[] args) {
        // Se instancia una lista genérica parametrizada con String
        List<String> lista = new ArrayList<>();
        lista.add("Hola");
        lista.add("Mundo");
        // lista.add(10); // Error de compilación: no admite enteros

        for (String elemento : lista) {
            // Se invoca un método exclusivo de String (toUpperCase) con total seguridad
            System.out.println(elemento.toUpperCase());
        }
    }
}
```

**En C++:**
```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Se instancia un vector de la Standard Template Library para std::string
    std::vector<std::string> lista;
    lista.push_back("Hola");
    lista.push_back("Mundo");
    // lista.push_back(10); // Error de compilación

    for (const std::string& elemento : lista) {
        // Se puede operar con seguridad asumiendo que son strings
        std::cout << elemento.length() << std::endl;
    }
    return 0;
}
```

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

El tratamiento que dan los compiladores a los parámetros de tipo es radicalmente distinto entre C++ y Java, originado por las diferentes filosofías de diseño de cada lenguaje. 

En C++, el mecanismo se denomina **instanciación de plantillas** (*template instantiation* o monomorfización). Cuando se usa un `std::vector<int>` y un `std::vector<std::string>`, el compilador de C++ genera y compila dos copias de código máquina completamente separadas y optimizadas para cada tipo exacto. Esto produce un rendimiento muy alto, pero puede incrementar drásticamente el tamaño del archivo ejecutable, un efecto conocido como "hinchazón de código" (*code bloat*).

En Java, se utiliza un mecanismo llamado **borrado de tipos** (*type erasure*). Para asegurar la compatibilidad con versiones antiguas de Java que no tenían genericidad, el compilador verifica la corrección de los tipos estáticamente, pero al generar el *bytecode* (`.class`), elimina (borra) todos los parámetros de tipo `<T>`. Estos son sustituidos internamente por `Object` (o por el tipo límite superior), y el compilador inserta automáticamente los *casts* (conversiones) necesarios. Por tanto, en Java solo existe una única clase compilada en memoria, independientemente de cuántos tipos distintos se instancien, previniendo el aumento del tamaño del código pero limitando cierta información de tipos durante la ejecución.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

La declaración de múltiples parámetros de tipo se realiza separándolos por comas dentro de los corchetes angulares. Esta estructura es sumamente útil cuando se requiere devolver más de un valor en un método de forma fuertemente tipada, solucionando la limitación de Java que solo permite un único valor de retorno.

En el siguiente ejemplo, la clase `Par` utiliza dos tipos genéricos, `T` y `U`. Posteriormente, en la función, se instancia concretamente como `Par<Double, Double>` para representar los dos estadísticos calculados.

```java
public class Par<T, U> {
    private final T primero;
    private final U segundo;

    public Par(T primero, U segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() { return primero; }
    public U getSegundo() { return segundo; }
}

public class Estadistica {
    // Función que devuelve dos valores tipados encapsulados en el Par genérico
    public static Par<Double, Double> calcularEstadisticas(double[] datos) {
        double media = 0.0;
        double desviacion = 0.0;
        
        // (Cálculos matemáticos omitidos por brevedad)
        media = 5.5; 
        desviacion = 1.2;
        
        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] array = {2.0, 4.0, 6.0, 8.0};
        Par<Double, Double> resultado = calcularEstadisticas(array);
        
        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
```

```java
class Par <P , Q> {
    private final P primero;
    private final Q segundo;

    public Par {P primero, Q segundo} {
        this.primero = primero;
        this.segundo = segundo;
    }

    public P getPrimero(){
        return this.primero
    }

    public Q getSegundo(){
        return this.segundo
    }
}
```

```java
class Estadisticas {
    public static Par <Double , Double> mediayDesviacionTipica (List <Double> valores) {
        //codigo de calculo

        return new Par <Double, Double>(media , media);
    }
}
```

```java
class Colegio {
    public static Par <Alumno , Double> obtenerAlumnosYsusNotas (List <Double> valores) {
        //codigo de calculo
        double media;
        double mediana;

        return new Par <Double, Double>(media , mediana);
    }
    //Sin gerenics!!
    List <Double> valores = ...;
    Par mediayDesviacionTipica = mediayDesviacionTipica(valores);
    double media = (Double) mediayDesviacionTipica.getPrimero

    Par resultadoAlumno = ...

    Alumno alumno = (Alumno) resultadoAlumno; //Dowcasting (peligroso)
}
```

```java
//sin generics!

class Par {
    private Object primero;
    private Object segundo;

        public primero getPrimero(){
        return this.primero
    }

    public segundo getSegundo(){
        return this.segundo
    }
}
```

## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Para declarar un método genérico, el parámetro de tipo debe especificarse antes del tipo de retorno del método. La gran ventaja reside en establecer relaciones semánticas estrictas entre los parámetros de entrada y el valor de salida de una función aislada.

Si se definiera el método como `public static Object seleccionaUno(Object a, Object b)`, existirían dos carencias enormes. Primero, se permitiría pasar un `String` y un `Integer` simultáneamente sin que el compilador se quejara. Segundo, el resultado sería de tipo `Object`, requiriendo un *downcasting* peligroso y tedioso por parte del llamador para recuperar el tipo original.

Al usar un parámetro de tipo `<T>`, se resuelven ambos problemas. Se obliga a que los dos argumentos compartan un tipo compatible `T`, y el valor de retorno asume inmediatamente ese mismo tipo `T`, eliminando la necesidad de realizar conversiones en el bloque que llama al método.

```java
public class Utilidades {

    // Método genérico: el tipo <T> se deduce al invocar el método
    public static <T> T seleccionaUno(T a, T b) {
        if (Math.random() > 0.5) {
            return a;
        }
        return b;
    }

    public static void main(String[] args) {
        // Correcto: Ambos son String, retorna un String directamente sin casting
        String elegido = seleccionaUno("Opción A", "Opción B");
        
        // Error de compilación si se mezclan tipos incompatibles 
        // y se intenta asignar a un tipo específico:
        // String fallo = seleccionaUno("Hola", 42); 
    }
}
```
```java
class PruebaGenerics {
    public static seleccionarUnoDeLosDos(T primero, T segundo){
        Random rand = new Random();
        return rand.nextBoolean() ? primero : segundo;
    }

    public static void main String [] args{
        Alumno a1 = new Alumno ("Alan");
        Alumno a2 = new Alumno ("Bea");

        Alumno unoAlAzar = seleccionaUnoDeLosDos(a1 , a2);

        Double d1 = 10.0;
        Double d2 = 30.0;

        Double unoAlAlzar  = seleccionaUnoDeLosDos(d1 , d2);
    }
}
```

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Sí, en Java se pueden acotar los parámetros de tipo utilizando límites (*bounds*). Mediante la sintaxis `<T extends ClaseBase>`, se restringe el tipo genérico para que solo acepte clases que hereden de `ClaseBase` o implementen dicha interfaz. Esto permite utilizar los métodos de esa clase superior sobre el objeto genérico dentro del código.

La primera solución, sin parámetros de tipo, utiliza polimorfismo clásico definiendo las coordenadas directamente con la superclase `Number`. La segunda solución aplica genericidad con restricción (`<T extends Number>`). 

Respecto al borrado de tipos (*type erasure*), el compilador eliminará la información genérica en la segunda solución y sustituirá la `T` no por `Object`, sino por el límite superior declarado. Por lo tanto, en el código binario compilado (el *.class*), ambas soluciones resultan en atributos de tipo `Number`.

```java
// Solución 1: Sin generics (Polimorfismo con herencia básica)
class PuntoNumber {
    private Number x;
    private Number y;

    public PuntoNumber(Number x, Number y) {
        this.x = x; this.y = y;
    }
    public Number getX() { return x; }
    public Number getY() { return y; }
    
    public double calcularDistanciaA(PuntoNumber otro) {
        return Math.sqrt(Math.pow(this.x.doubleValue() - otro.x.doubleValue(), 2) + 
                         Math.pow(this.y.doubleValue() - otro.y.doubleValue(), 2));
    }
}

// Solución 2: Con generics y límites (Bounded Type Parameters)
class PuntoGen<T extends Number> {
    private T x;
    private T y;

    public PuntoGen(T x, T y) {
        this.x = x; this.y = y;
    }
    public T getX() { return x; }
    public T getY() { return y; }
    
    public double calcularDistanciaA(PuntoGen<?> otro) {
        return Math.sqrt(Math.pow(this.x.doubleValue() - otro.x.doubleValue(), 2) + 
                         Math.pow(this.y.doubleValue() - otro.y.doubleValue(), 2));
    }
}
```

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

La diferencia fundamental reside en la homogeneidad que imponen los parámetros de tipo. En la solución sin genericidad (usando `Number`), las coordenadas son tratadas de forma totalmente independiente. Esto permite instanciar un `PuntoNumber` pasando un `Integer` para la 'x' y un `Double` para la 'y' en la misma instancia, ya que ambos cumplen con ser `Number`. 

En la solución con genericidad, al usar la clase `PuntoGen<T extends Number>`, se declara un único tipo `T` para toda la instancia. Al crear un `PuntoGen<Integer>`, el compilador fuerza rígidamente a que tanto 'x' como 'y' sean obligatoriamente enteros. Se gana consistencia en el estado interno del objeto, impidiendo mezclas accidentales de dominios numéricos.

Además, difieren notablemente en el tipo de retorno de los métodos. El método `getX()` de `PuntoNumber` devuelve invariablemente una referencia `Number`. Si el usuario sabe que introdujo un entero y desea usar métodos de la clase `Integer`, deberá realizar un *downcasting* manual. Por el contrario, en `PuntoGen<Integer>`, el método `getX()` devuelve un `Integer` directo y tipado, sin necesidad de conversiones, gracias a la validación estática del compilador.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.

Para resolver este problema arquitectónico y evitar la validación en tiempo de ejecución (`instanceof`) y el consiguiente *downcasting*, se recurre a un patrón avanzado de genericidad donde la propia interfaz se parametriza con un tipo `T`. 

Al hacerlo, la interfaz dicta que la distancia solo se puede calcular con objetos del tipo estricto `T`. Cuando una clase concreta implementa la interfaz, se pasa a sí misma como parámetro de tipo (por ejemplo, `class Punto2D implements Punto<Punto2D>`). Esto fuerza a que la firma del método en la subclase acepte exclusivamente el tipo derivado exacto.

```java
// La interfaz requiere un parámetro genérico T
public interface Punto<T> { 
    public double distanciaA(T p); 
} 

// Punto2D parametriza la interfaz pasándose a sí misma
public class Punto2D implements Punto<Punto2D> { 
     private final double x, y; 
     
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    // Ahora el método exige estáticamente un Punto2D, evitando el instanceof
    public double distanciaA(Punto2D p2d) { 
        return Math.sqrt(Math.pow(x - p2d.x, 2) + Math.pow(y - p2d.y, 2)); 
    } 
} 

// Punto3D actúa de la misma manera respecto a sí misma
public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z; 
    
    public Punto3D(double x, double y, double z) { 
        this.x = x; this.y = y; this.z = z; 
    } 

    @Override 
    public double distanciaA(Punto3D p3d) { 
        return Math.sqrt(Math.pow(x - p3d.x, 2) + 
                         Math.pow(y - p3d.y, 2) + 
                         Math.pow(z - p3d.z, 2)); 
    } 
} 
```

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

En Java, aunque `String` es un subtipo de `Object`, una `List<String>` **no** es un subtipo de `List<Object>`. Si el lenguaje lo permitiera, se podría pasar una lista de cadenas a un método que espera una lista de objetos, y este método podría insertar un `Integer` en ella. Al retornar, la lista original de cadenas contendría un entero, rompiendo toda la seguridad de tipos. Por esto, los tipos genéricos en Java son **invariantes**: no existe relación de herencia entre las parametrizaciones, aunque exista entre sus tipos subyacentes.

Por el contrario, un array de `String[]` **sí** es considerado un subtipo de `Object[]`. Esta decisión de diseño histórico en Java permite que los arrays sean **covariantes**. Sin embargo, esto introduce una vulnerabilidad en tiempo de ejecución: si se asigna un `String[]` a una variable `Object[]` y se intenta guardar un entero, el compilador lo permitirá, pero la Máquina Virtual de Java lanzará una excepción `ArrayStoreException` durante la ejecución.

* **Covariante**: Preserva la dirección de la jerarquía de subtipos. Si `A` es hijo de `B`, entonces `Contenedor<A>` es hijo de `Contenedor<B>` (sucede con los arrays en Java).
* **Contravariante**: Invierte la jerarquía de subtipos. Si `A` es hijo de `B`, entonces `Contenedor<B>` se comporta como un subtipo aplicable a `Contenedor<A>`.
* **Invariante**: No conserva ninguna jerarquía. `Contenedor<A>` y `Contenedor<B>` son tipos completamente incompatibles entre sí (sucede con los genéricos en Java de forma predeterminada).

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Un comodín o *wildcard* (`?`) en Java representa un "tipo desconocido". Se emplea fundamentalmente en las firmas de los métodos para flexibilizar la rigidez invariante de los genéricos, permitiendo que un método acepte colecciones de familias enteras de tipos de forma segura, definiendo límites superiores o inferiores.

La sintaxis `List<? extends T>` establece un límite superior (covarianza). Significa "una lista de un tipo que es `T` o cualquier subclase de `T`". Se utiliza exclusivamente en escenarios de **lectura** (como productores de datos), porque se garantiza que cualquier elemento que se extraiga será al menos un `T`, pero el compilador prohíbe añadir nuevos elementos, ya que desconoce el tipo exacto de la lista. 

Por otro lado, `List<? super T>` establece un límite inferior (contravarianza). Significa "una lista de un tipo que es `T` o cualquier superclase de `T`". Se emplea en escenarios de **escritura** (como consumidores de datos). Como se sabe que la lista puede contener elementos de tipo `T` o superiores, es seguro añadir instancias de `T`, garantizando que no se romperá el tipado.

```java
import java.util.List;

public class UtilidadesWildcards {

    // Ejemplo (i): Uso de ? extends T (Lectura / Covarianza)
    // Acepta List<Number>, List<Integer>, List<Double>...
    public static double sumar(List<? extends Number> numeros) {
        double total = 0.0;
        for (Number num : numeros) { // Se asegura que todo lo leído es un Number
            total += num.doubleValue();
        }
        // numeros.add(3); // ERROR: No se puede escribir
        return total;
    }

    // Ejemplo (ii): Uso de ? super T (Escritura / Contravarianza)
    // Acepta List<Integer>, List<Number>, List<Object>...
    public static void añadirEnteros(List<? super Integer> destino) {
        // Seguro añadir Integer, porque la lista es de Integer o superiores
        destino.add(10);
        destino.add(20);
        
        // Integer leido = destino.get(0); // ERROR: Retorna Object, lectura insegura
    }
}
```
```