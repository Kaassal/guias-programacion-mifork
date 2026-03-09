
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Excepción: Gestiona una situación anomala

Errores
- De entrada de usuario (de validación)
- De programacion 
- De entrada salida (E/S)


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

``` java
public class Calculadora {
    public double raiz(double numero) {
        if (numero < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular raíz de número negativo"
            );
        }
        return Math.sqrt(numero);
    }
}

public class Main {
    public static void main(String[] args) {
        Calculadora calc = new Calculadora();
        
        try {
            double resultado = calc.raiz(-4);
            System.out.println("Raíz: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}
``` 

En Java, cuando el método raiz() detecta un número negativo, lanza una excepción usando throw. Esta excepción interrumpe la ejecución del método y salta al bloque catch.

El código que llama la función utiliza la estructura try-catch:

    El bloque try contiene el código que podría lanzar una excepción.
    El bloque catch especifica qué tipo de excepción se captura y cómo se maneja. En este caso, se imprime el mensaje de error encapsulado en la excepción, permitiendo que el programa continúe de forma controlada en lugar de terminar abruptamente.



## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

a) ¿Qué es "lanzar" una excepción?

Lanzar una excepción significa ejecutar la instrucción throw, creando un objeto excepción y deteniendo inmediatamente la ejecución del método. En el ejemplo anterior, cuando se invoca calc.raiz(-4) y la función detecta un número negativo, se ejecuta throw new IllegalArgumentException(...), lo que crea el objeto excepción y abandona el método sin devolver ningún valor.

b) ¿Qué es "controlar" o "capturar" una excepción?

Controlar o capturar una excepción es el acto de interceptarla usando un bloque catch. En el main, el bloque catch (IllegalArgumentException e) captura la excepción que fue lanzada en raiz(), permitiendo ejecutar código alternativo como imprimir un mensaje de error. Sin este bloque, el programa finalizaría abruptamente con un error no controlado.

c) ¿Qué es que se "propague" una excepción?

Propagar una excepción significa que, si no se captura en el método donde se lanzó, la excepción "viaja" hacia arriba por la pila de llamadas buscando un catch que pueda manejarla.

d) ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción?

Desde el punto de vista de la pila: cuando raiz() lanza la excepción, ese método desaparece de la pila inmediatamente; el control retrocede un nivel buscando un catch en main(), donde lo encuentra.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

### Respuesta


## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

### Respuesta


## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Cada objeto de la clase excepción tiene 3 elementos

- Un mensaje
- Una traza de llamadas
    - La traza nos ayuda a saber donde se originó la excepción y por donde se propagó
- Opcionalmente otra excepcion causa de esta

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí puede haber mas de un bloque `catch` , no es super comun pero se puede hacer.

Solo se ejecuta uno
- Se comprueban en el orden que estan declarados, el primero que encaje con la excepción es el primero que se ejecuta.

> Nota: Esto significa que se deben ordenar de mas especifica a más general.

Ejemplo: Exception (engloba a) > IOException (engloba a) > AccessDeniedException

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar la liberación de recursos frente a las rupturas abruptas del flujo que causan las excepciones, se utiliza el bloque **`finally`**. El entorno de ejecución asegura que el código dentro del bloque `finally` se ejecute siempre, sin importar si el bloque `try` finalizó con éxito, si lanzó una excepción que fue capturada por un `catch`, o si lanzó una excepción que se propagará a niveles superiores.

```java
import java.io.*;

public class GestorFicheros {
    
    // Ejemplo de try-catch-finally
    public void leerConCatch(String ruta) {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(ruta);
            // Operaciones de lectura que podrían fallar
        } catch (FileNotFoundException e) {
            System.out.println("Error: Archivo no encontrado.");
        } finally {
            // Este bloque se ejecuta SIEMPRE, haya error o no
            if (fis != null) {
                try { fis.close(); } catch (IOException ex) {}
            }
        }
    }

    // Ejemplo de try-finally (sin catch, la excepción se propaga hacia arriba)
    public void leerSinCatch(String ruta) throws FileNotFoundException {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(ruta);
            // Operaciones de lectura. Si fallan, se corta aquí.
        } finally {
            // Se garantiza el cierre del recurso antes de que 
            // la excepción continúe propagándose al método llamador.
            if (fis != null) {
                try { fis.close(); } catch (IOException ex) {}
            }
        }
    }
}

        }
    }
}
```

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí es correcto tener un `try-catch` que no pase por bloques `catch` y que vaya directamete a un `finally`. Esto es util para garantizar que el codigo que se va a ejecutar, pero que no tiene la responsabilidad de gestionar errores, permitiendo que esta se propague a la llamada.

El bloque `finally` se ejecuta **siempre**, aunque el código lance errores o finalice correctamente. Aún más notable: Si dentro del bloque `try` (o dentro de un `catch`) se ejecuta una sentencia `return` para salir anticipadamente del método, el sistema pausa la ejecución del `return`, salta al bloque `finally`, lo ejecuta en su totalidad, y solo entonces devuelve el valor y finaliza el método original.


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

Excepciones no controladas (No estamos obligados a poner try-catch):
- Tipicamemte errores de programación que una vez solventados no vuelven a ocurrrir

Excecpciones controladas (obliga a poner try-catch):
- Tipicamente errores por causas externas y que siempre pueden llegar a ocurrir ej: errores de E/S

TODO:Insertar diagrama mermaid con excepciones

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

Se usa en la declaracion de un metodo para advertir que puede hacer un return de una o mas exceciones controladas. 

Alternativa al bloque try-catch que delega la responsabilidad del manejo de errores, haciendo que la la excepcion se capture o se propague cuando el metodo es llamado.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

En este ejemplo, el método `leerPrimeraLinea` no intenta gestionar el fallo en caso de que el archivo no se encuentre o no pueda leerse. Su firma utiliza `throws IOException` (que abarca también `FileNotFoundException`) para advertir a sus clientes. Sin embargo, asume su responsabilidad interna liberando el recurso de memoria con el uso de `finally`.

```java
import java.io.*;

public class Lector {

    // La firma indica explícitamente que propaga la excepción
    public String leerPrimeraLinea(String rutaArchivo) throws IOException {
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader(rutaArchivo));
            return reader.readLine(); 
        } finally {
            // El finally asegura el cierre del fichero si llegó a abrirse, 
            // independientemente de que la lectura falle y se propague el error.
            if (reader != null) {
                reader.close();
            }
        }
    }
}

```


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí que se puede pero

- El compilador no obliga a usar try-cacth o throws en el codigo llamador.
- Se puede usar por propositos de documentación
- No es lo habitual

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

- No hay ambas opciones en todos los lenguajes
- La más tipuca es la de "no controlada"

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene sentido para hacer un log.
Sí se puede relanzar el mismo objeto excepción, tras , por ejemplo, haber ejecutado algo en el catch.

TODO: mezclar fcon respuesta de ia 


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

Exception chaining 

TODO: Completar
