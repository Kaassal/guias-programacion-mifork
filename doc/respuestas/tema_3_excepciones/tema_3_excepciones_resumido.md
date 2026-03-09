
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

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

