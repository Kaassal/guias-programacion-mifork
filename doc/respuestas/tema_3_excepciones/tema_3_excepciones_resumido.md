# TEMA 3. Excepciones (Resumido)

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C, al carecer de excepciones nativas, se utilizan dos enfoques principales para gestionar errores devolviendo la responsabilidad al código llamador:

1. **Devolver un valor centinela**: La función devuelve un valor especial e inválido (ej. `-1.0`) que indica que ha ocurrido un error.
2. **Devolver código de error y usar punteros**: La función retorna un entero que actúa como código de estado (`0` para éxito, `-1` para error), mientras que el resultado real de la operación matemática se guarda modificando una variable pasada por referencia (puntero).

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un evento anómalo que interrumpe el flujo normal de ejecución. Se utiliza para gestionar situaciones de error separando la lógica de negocio de la lógica de control de fallos.

Tipos de errores habituales:
- De entrada de usuario o validación.
- De programación o lógica (*bugs*).
- De Entrada/Salida (E/S) o entorno.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

```java
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

En Java, cuando el método `raiz()` detecta un número negativo, lanza una excepción usando `throw`. Esta excepción interrumpe la ejecución del método y salta al bloque `catch`. El código llamador utiliza `try-catch` para interceptar la excepción e imprimir el mensaje encapsulado, evitando que el programa termine abruptamente.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

* **Lanzar (`throw`)**: Es el acto de generar la señal de error, instanciando un objeto excepción y deteniendo inmediatamente el método actual (ej. `throw new IllegalArgumentException(...)`).
* **Capturar/Controlar (`catch`)**: Consiste en interceptar esa señal de error en un bloque de código para manejarla de forma segura e impedir que el programa colapse.
* **Propagar**: Si una excepción no se captura en la función donde nace, viaja hacia arriba por la pila de llamadas (*stack unwinding*) buscando un bloque `catch` que se haga cargo.
* **Efecto en la pila**: Las funciones intermedias que no controlan la excepción son abortadas y eliminadas de la pila. **No se reanudan** bajo ninguna circunstancia.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

* **Limpieza de código y delegación**: Evita escribir código repetitivo de validación (`if (error != 0)`) en cada nivel. Las funciones intermedias simplemente ignoran los errores que no saben gestionar, delegándolos a niveles superiores.
* **Evita fallos silenciosos (*fail-fast*)**: Si un programador olvida controlar un error, el programa con excepciones termina de forma evidente y segura. En C, ignorar un código de retorno provoca que el programa continúe en un estado corrupto e inconsistente.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Sí, en lenguajes modernos las excepciones son **objetos** reales instanciados a partir de clases.

* **Ventajas de encapsulación**: Agrupan toda la información relevante del fallo (mensaje descriptivo, traza de ejecución, causa previa) en un solo paquete. Además, aprovechan el polimorfismo para agrupar capturas por jerarquía.
* **Excepciones personalizadas**: Al ser clases, se pueden crear errores específicos del dominio de negocio (ej. `SaldoInsuficienteException`) heredando de las clases base de excepciones del lenguaje.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

Cada objeto de la clase excepción contiene tres elementos vitales:
1. **Mensaje de texto**: Explicación legible del motivo del error.
2. **Traza de la pila (*Stack Trace*)**: Lista exacta de las llamadas a métodos, archivos y líneas de código por donde pasó la ejecución, vital para saber dónde y cómo se originó el fallo.
3. **Causa**: Opcionalmente, encapsula otra excepción que fue el detonante original del problema.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, puede haber más de un bloque `catch` para un solo `try`.

* **Solo se ejecuta uno**: El primero que encaje con el tipo de la excepción lanzada.
* Por este motivo, deben ordenarse siempre **de más específicos a más generales** (ej. colocar `FileNotFoundException` antes que un `Exception` genérico).

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Para garantizar la ejecución de código crítico (como cerrar ficheros o conexiones) frente a las rupturas abruptas, se usa el bloque **`finally`**. Se garantiza su ejecución siempre, haya o no haya saltado una excepción.

```java
import java.io.*;

public class GestorFicheros {
    
    // Con catch: Maneja el error localmente
    public void leerConCatch(String ruta) {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(ruta);
        } catch (FileNotFoundException e) {
            System.out.println("Error: Archivo no encontrado.");
        } finally {
            if (fis != null) {
                try { fis.close(); } catch (IOException ex) {}
            }
        }
    }

    // Sin catch: Propaga la excepción, pero asegura la limpieza
    public void leerSinCatch(String ruta) throws FileNotFoundException {
        FileInputStream fis = null;
        try {
            fis = new FileInputStream(ruta);
        } finally {
            if (fis != null) {
                try { fis.close(); } catch (IOException ex) {}
            }
        }
    }
}
```

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, es correcto y habitual tener un `try-finally` omitiendo el `catch`. Se usa para garantizar la limpieza de recursos sin asumir la responsabilidad de gestionar el error, dejando que este se propague hacia arriba.

El bloque `finally` se ejecuta **siempre**, incluso si el código finaliza con éxito o falla. De hecho, si hay una instrucción `return` dentro del `try`, la ejecución se pausa, salta al `finally` para ejecutarlo en su totalidad, y solo entonces se efectúa el `return` real.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo...

En Java, las excepciones se dividen en dos categorías:

* **No controladas (*unchecked*)**: Derivan de `RuntimeException`. El compilador no obliga a capturarlas. Suelen ser errores de programación o lógica que no deberían ocurrir si el código está bien (ej. `NullPointerException`, `IllegalArgumentException`, `IndexOutOfBoundsException`).
* **Controladas (*checked*)**: Son el resto. El compilador **obliga** a usar `try-catch` o a declararlas con `throws`. Representan fallos externos de los que el programa podría intentar recuperarse (ej. `IOException`, `FileNotFoundException`, `SQLException`).

*(Jerarquía conceptual)*:
`Throwable` -> `Exception` (Controladas) -> `RuntimeException` (No controladas)

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

`throws` se usa en la firma de un método para advertir explícitamente que dicho método es susceptible de lanzar ciertas excepciones controladas. 

Es una alternativa al `try-catch` porque **delega la responsabilidad**. En lugar de gestionar el error internamente (donde quizás no se tenga el contexto de negocio adecuado), se indica al compilador que la anomalía debe ser gestionada obligatoriamente por el método llamador.

## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

```java
import java.io.*;

public class Lector {
    // Declara explícitamente que propaga la excepción
    public String leerPrimeraLinea(String ruta) throws IOException {
        BufferedReader reader = null;
        try {
            reader = new BufferedReader(new FileReader(ruta));
            return reader.readLine(); 
        } finally {
            // Asegura el cierre del fichero independientemente del error
            if (reader != null) {
                reader.close();
            }
        }
    }
}
```

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

Sí se puede sintácticamente, pero:
* El compilador **no obligará** al código llamador a usar `try-catch` ni `throws`.
* No es habitual ni recomendable capturarlas sistemáticamente; la solución a las *RuntimeException* es corregir el error lógico, no silenciarlo.
* Su único sentido al ponerlas en el `throws` es puramente **documental**, para advertir a otros desarrolladores sobre las validaciones internas de la función.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomiendan las **controladas** para problemas del entorno (E/S, red) donde es posible la recuperación. Las **no controladas** se recomiendan para violaciones de contrato o errores lógicos internos.

Este sistema dual es casi exclusivo de Java. En la gran mayoría de lenguajes modernos (C#, Python, Ruby, Kotlin), **solo existen las excepciones no controladas**, recayendo la responsabilidad de la gestión íntegramente en la disciplina del desarrollador y no en el compilador.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene mucho sentido y existen dos escenarios típicos:

1. **Traducción de excepciones**: Se captura un error técnico (`SQLException`) y dentro del `catch` se lanza un error de alto nivel (`UsuarioNoEncontradoException`), ocultando los detalles de implementación a las capas superiores.
2. **Relanzar la misma excepción (`throw e;`)**: Útil para intervenir temporalmente en el flujo, por ejemplo, para registrar el error en un archivo de Log, y luego dejar que la excepción original siga su camino hacia arriba.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

El encadenamiento de excepciones (*Exception Chaining*) consiste en que un objeto excepción guarda en su interior la referencia a la excepción original de más bajo nivel que provocó el fallo.

Esto permite "traducir" excepciones sin perder de forma destructiva el rastro técnico original. Y sí, **sí se ve**. Al imprimir la traza (ej. `printStackTrace()`), aparece primero la excepción de alto nivel y debajo, indicado con `"Caused by: "`, se imprime la traza del detonante original, siendo invaluable para la depuración.
