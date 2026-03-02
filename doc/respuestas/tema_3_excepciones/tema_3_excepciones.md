
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En lenguajes como C, al carecer de un mecanismo nativo de excepciones, la gestión de errores se realiza tradicionalmente a través de los valores de retorno de las funciones o manipulando punteros/variables globales. Al calcular una raíz cuadrada, la función necesita una vía para indicar al código llamador que la operación ha fallado sin imprimir el error directamente.

La **primera opción** consiste en devolver un "valor centinela" o código de error específico. Como una raíz cuadrada real nunca puede ser negativa, se puede devolver un número negativo (como `-1.0`) para señalar el error. El código que llama a la función debe verificar si el resultado es válido antes de usarlo.

La **segunda opción** separa el estado de éxito/error del resultado matemático. Se devuelve un código de estado (por ejemplo, un número entero donde `0` es éxito y `-1` es error), mientras que el resultado de la operación se devuelve modificando una variable a través de un puntero pasado por referencia. 

```c
#include <stdio.h>
#include <math.h>

// Opción 1: Devolver valor centinela
double raiz_opcion1(double numero) {
    if (numero < 0) {
        return -1.0; // Valor especial que indica error
    }
    return sqrt(numero);
}

// Opción 2: Devolver código de error y usar puntero para el resultado
int raiz_opcion2(double numero, double *resultado) {
    if (numero < 0) {
        return -1; // Código de error
    }
    *resultado = sqrt(numero);
    return 0; // Éxito
}

int main() {
    // Uso de Opción 1
    double res1 = raiz_opcion1(-4.0);
    if (res1 < 0) {
        printf("Error: Numero negativo (Op1)\n");
    }

    // Uso de Opción 2
    double res2;
    if (raiz_opcion2(-4.0, &res2) != 0) {
        printf("Error: Numero negativo (Op2)\n");
    }
    return 0;
}

```

## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una **excepción** es un evento anómalo que ocurre durante la ejecución de un programa y que interrumpe el flujo normal de las instrucciones. Representa una situación de error o un comportamiento inesperado (como intentar leer un archivo que no existe o dividir por cero) que el entorno de ejecución o el propio código detecta.

El objetivo principal de utilizar excepciones es separar la lógica de negocio del código de gestión de errores. Cuando se implementa una función, se lanzan excepciones para delegar la responsabilidad del error a los niveles superiores del programa, evitando retornar códigos de error opacos. Cuando se llama a una función, las excepciones se capturan para manejar la situación de forma centralizada y estructurada, garantizando que los errores no pasen desapercibidos y permitiendo que el programa se recupere de forma elegante si es posible.

## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java, el paradigma cambia radicalmente. Ya no es necesario depender de valores centinela ni de parámetros de salida. La función devuelve estrictamente el tipo de dato esperado (un `double`) y, en caso de error, interrumpe su ejecución creando y arrojando un objeto que representa el fallo.

Desde el método llamador (`main`), se establece un bloque de control estructurado (`try-catch`). El código asume que todo irá bien dentro de la sección `try`, pero si se lanza la excepción, el flujo salta automáticamente al bloque `catch` correspondiente para gestionar la información de error de forma aislada.

```java
class Calculadora {
    // Método que calcula la raíz o lanza una excepción
    public static double raiz(double numero) {
        if (numero < 0) {
            // Se crea y lanza el objeto excepción
            throw new IllegalArgumentException("No se puede calcular la raiz de un numero negativo.");
        }
        return Math.sqrt(numero);
    }
}

public class Main {
    public static void main(String[] args) {
        try {
            // Se intenta ejecutar el código propenso a errores
            double resultado = Calculadora.raiz(-4.0);
            System.out.println("El resultado es: " + resultado);
        } catch (IllegalArgumentException e) {
            // Se controla la excepción desde el exterior
            System.out.println("Error controlado: " + e.getMessage());
        }
        System.out.println("El programa continua su ejecucion normal...");
    }
}

```

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

**Lanzar** (`throw`) una excepción es el acto de generar una señal de alarma cuando se detecta un error; en el ejemplo, `Calculadora.raiz` lanza `IllegalArgumentException` al recibir `-4.0`, interrumpiendo su ejecución normal inmediatamente. **Controlar** o **capturar** (`catch`) consiste en interceptar esa señal en un nivel superior (en el `main`) para procesar el error y evitar que el programa colapse de forma abrupta.

La **propagación** es el proceso automático que ocurre cuando una función lanza una excepción y no la captura internamente. El entorno de ejecución busca un manejador iterando hacia atrás en la pila de llamadas (Call Stack). En este proceso de retroceso (conocido como *stack unwinding*), las funciones intermedias que no controlan la excepción son abortadas y extraídas de la pila.

Es crucial entender que las funciones que son abortadas durante la propagación **no se reanudan** bajo ninguna circunstancia. Si `Calculadora.raiz` fue llamada por una función `operacionCompleja`, y esta a su vez por `main`, un error en `raiz` destruirá el contexto de `operacionCompleja` por completo si no hay un `try-catch` en ella. El flujo se retomará exclusivamente después del bloque `catch` en el `main` que finalmente interceptó el problema.

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La principal ventaja es la **limpieza del código y la delegación de responsabilidades**. En C, si una función de bajo nivel falla, cada una de las funciones intermedias en la pila de llamadas debe verificar el código de retorno y pasarlo explícitamente hacia arriba (generando una gran cantidad de código repetitivo de validación). Con la propagación de excepciones, las funciones intermedias pueden ignorar completamente los errores que no saben manejar; la excepción subirá automáticamente hasta encontrar el bloque adecuado.

Otra ventaja fundamental es la **imposibilidad de ignorar errores silenciosamente**. En C, si un programador olvida comprobar el código de retorno de una función que falló, el programa continuará su ejecución en un estado inconsistente, provocando errores difíciles de rastrear más adelante. Con las excepciones, si un error se propaga y nadie lo captura en toda la pila de llamadas, el programa finalizará de forma evidente, asegurando que los fallos sean visibles de inmediato (*fail-fast*).

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

Sí, en los lenguajes orientados a objetos modernos (como Java, C#, C++ y Python), las excepciones son instancias concretas de clases específicas. Cuando ocurre un error, se crea un objeto en memoria dinámica que representa dicho evento, en lugar de manejar simples números o códigos primitivos.

En términos de encapsulación, esto permite agrupar toda la información relevante sobre el error dentro del mismo objeto. Una excepción no solo informa de que "algo falló", sino que puede contener mensajes de texto detallados, referencias a otras excepciones previas, códigos de error de bases de datos o el estado exacto de las variables en el momento de la falla. Al ser objetos, aprovechan el polimorfismo, permitiendo capturar categorías enteras de errores capturando una clase base.

Dado que son clases, es completamente posible crear **excepciones personalizadas**. Basta con definir una nueva clase que herede de la jerarquía de excepciones del lenguaje (como extender `Exception` en Java). Esto permite a los desarrolladores modelar errores específicos de su dominio de negocio (por ejemplo, `SaldoInsuficienteException` o `UsuarioNoEncontradoException`), enriqueciendo la expresividad del sistema.

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

La información más crítica e invaluable que encapsula un objeto excepción (y que no existe en un simple código de retorno de C) es la **traza de la pila** (*Stack Trace*). Esta traza registra con precisión la jerarquía exacta de llamadas a métodos, incluyendo el nombre de los archivos y los números de línea exactos donde se originó el fallo y por dónde se propagó.

Además de la traza, las excepciones suelen llevar un mensaje de texto descriptivo y legible para humanos (`message`), explicando la naturaleza semántica del error. También pueden incluir un objeto "causa" (`cause`), que es otra excepción encapsulada en caso de que un error de bajo nivel haya desencadenado un error de alto nivel. Todo este contexto empaquetado facilita enormemente la depuración y resolución de problemas.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

Sí, un único bloque `try` puede estar seguido de múltiples bloques `catch`. Esto permite definir manejadores específicos para diferentes tipos de errores que podrían generarse dentro del mismo segmento de código protegido (por ejemplo, tener un `catch` para errores matemáticos y otro `catch` para errores de lectura de archivos).

Es importante destacar que **solo se ejecuta un único bloque `catch**`: el primero cuya declaración de tipo coincida con el objeto excepción lanzado (o sea una superclase del mismo). Debido a este comportamiento, el orden de los bloques `catch` es crítico; las excepciones más específicas (subclases) deben capturarse antes que las excepciones más genéricas (superclases). Una vez que un bloque `catch` finaliza, el flujo de ejecución salta el resto de bloques `catch` y continúa después de la estructura.

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

```

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

Sí, es perfectamente legal tener una estructura `try-finally` omitiendo los bloques `catch` (como se vio en el ejemplo anterior). Este patrón se utiliza cuando un método desea garantizar la ejecución de un código de limpieza local, pero no tiene la responsabilidad o la capacidad semántica de manejar la excepción en sí, permitiendo que esta se propague al llamador.

El bloque `finally` se ejecuta **siempre**, con independencia de que el código lance errores o finalice correctamente. Aún más notable: si dentro del bloque `try` (o dentro de un `catch`) se ejecuta una sentencia `return` para salir anticipadamente del método, el sistema pausa la ejecución del `return`, salta al bloque `finally`, lo ejecuta en su totalidad, y solo entonces devuelve el valor y finaliza el método original.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java, el sistema de excepciones se divide en dos categorías impuestas por el compilador. Las excepciones **controladas** (*checked exceptions*) son aquellas que el programador está **obligado** a manejar explícitamente (ya sea con un `try-catch` o declarando que las propaga). Representan fallos externos al código de los cuales un programa robusto debería intentar recuperarse. Las excepciones **no controladas** (*unchecked exceptions*) no requieren un manejo obligatorio y suelen representar errores de lógica o *bugs* del programador (fallos que no deberían ocurrir en absoluto si el código estuviera bien escrito).

La clase `RuntimeException` (y todas sus subclases) actúa como la raíz de las excepciones no controladas. El compilador de Java permite que estas excepciones ocurran sin forzar su declaración ni su captura, aliviando el código de bloques `try-catch` masivos para problemas que generalmente son fatales e irrecuperables.

**Situaciones para Excepciones Controladas (Se esperan y se gestionan):**

* Intentar abrir un archivo de configuración y que no exista en disco (`FileNotFoundException`).
* Perder la conexión de red mientras se descarga información (`IOException`).
* Intentar conectar a una base de datos con credenciales incorrectas (`SQLException`).
* Intentar cargar una clase dinámica por nombre y que falte el .jar (`ClassNotFoundException`).

**Situaciones para Excepciones No Controladas (Errores de programación a corregir):**

* Intentar invocar un método sobre una referencia nula (`NullPointerException`).
* Pasar un argumento inválido a una función, como el -4 en la raíz cuadrada (`IllegalArgumentException`).
* Acceder a la posición 10 de un array que solo tiene 5 elementos (`IndexOutOfBoundsException`).
* Intentar convertir una cadena con letras en un número entero (`NumberFormatException`).

## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

La palabra reservada `throws` se coloca en la firma (declaración) de un método para advertir explícitamente que dicho método es susceptible de lanzar una o varias excepciones controladas (*checked exceptions*). Funciona como un contrato o advertencia oficial hacia cualquier componente externo que desee utilizar esa función.

Es una alternativa al bloque `try-catch` porque delega la responsabilidad del manejo del error. En lugar de capturar y resolver el problema dentro de la propia función (lo cual puede no tener sentido si la función es de bajo nivel y no sabe qué decisión de negocio tomar), el uso de `throws` le indica al compilador que la función no se hará cargo de la anomalía, obligando de forma estricta al método que la invocó a capturarla o a seguir propagándola.

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

Sí, sintácticamente es completamente legal declarar excepciones no controladas en la cláusula `throws` de un método. Sin embargo, para el compilador de Java, esto no cambia nada: no obligará al método llamador a poner un bloque `try-catch` ni a propagarla explícitamente.

El método llamador no debería intentar capturarla sistemáticamente. Como las excepciones no controladas derivan de `RuntimeException` e indican errores lógicos (*bugs* como nulos o índices fuera de rango), la solución correcta no es rodear la llamada con un `try-catch` para ocultar el fallo, sino corregir el código para evitar que el error ocurra en primer lugar.

El único sentido práctico de incluir una excepción no controlada en la firma mediante `throws` es meramente documental. Sirve para advertir a otros programadores, leyendo el código fuente o el JavaDoc, sobre el comportamiento interno del método y las validaciones de negocio que realiza (por ejemplo, advertir explícitamente que puede lanzar `IllegalArgumentException` si los parámetros son inválidos).

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

La recomendación general (y la filosofía detrás del diseño de Java) es utilizar excepciones controladas (*checked*) para condiciones excepcionales de las cuales se espera que el programa cliente **pueda recuperarse**. Suelen estar relacionadas con el entorno de ejecución, operaciones de Entrada/Salida o interacciones de red. Por el contrario, las excepciones no controladas (*unchecked*) se recomiendan para indicar violaciones del contrato de la API, precondiciones no cumplidas o errores lógicos internos.

Este sistema dual de excepciones **no existe en la mayoría de los lenguajes** modernos. Lenguajes como C#, Python, Ruby, C++ o Kotlin decidieron omitir las excepciones controladas por completo al considerarlas engorrosas y promotoras de código "sucio" (bloques catch vacíos solo para silenciar al compilador).

En estos lenguajes donde solo existe una opción, **todas las excepciones son no controladas** (comportándose como las subclases de `RuntimeException` en Java). Es responsabilidad exclusiva de la disciplina del programador documentar qué errores puede emitir una función y decidir en qué punto de la arquitectura deben ser interceptados y tratados, sin coacción del compilador.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene pleno sentido y es una práctica común conocida como **traducción de excepciones**. Ocurre cuando se captura una excepción técnica de bajo nivel dentro de un `catch` y se lanza un nuevo objeto de excepción, generalmente más abstracto y personalizado para el dominio del negocio, ocultando detalles de implementación que el llamador no necesita conocer.

También es posible **relanzar la misma excepción** (`throw e;`). Esto tiene sentido cuando se necesita intervenir en el punto de fallo para realizar tareas de auditoría (como escribir en un archivo de log el error exacto) o realizar limpiezas parciales muy específicas antes de dejar que la excepción original continúe su camino natural hacia los manejadores superiores.

```java
public class BaseDatos {
    
    // Ejemplo de traducción de excepción (lanzar nueva en el catch)
    public void buscarUsuario(String id) throws UsuarioNoEncontradoException {
        try {
            // Intento de operación SQL real
            ejecutarQuery(id);
        } catch (SQLException e) {
            // Ocultamos el error de SQL y lanzamos un error de negocio
            throw new UsuarioNoEncontradoException("No se encontro el usuario: " + id);
        }
    }

    // Ejemplo de relanzar la misma excepción
    public void procesarArchivo(String ruta) throws IOException {
        try {
            abrirYLeer(ruta);
        } catch (IOException e) {
            // Intervenimos sólo para registrar el evento en el sistema de logs
            Logger.log("Fallo crítico al procesar: " + e.getMessage());
            // Relanzamos exactamente la misma excepción para que la gestione el invocador
            throw e; 
        }
    }
}

```

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

El mecanismo de "causa" (también llamado encadenamiento de excepciones o *Exception Chaining*) consiste en que un objeto excepción guarda una referencia interna a otra excepción previa que originó el problema. Esto permite traducir una excepción de bajo nivel a una de alto nivel (como se vio en la pregunta anterior) sin perder de forma destructiva el rastro técnico original del fallo, crucial para el equipo de desarrollo.

Sí, cuando una excepción encadenada sale por pantalla o se imprime en consola mediante `printStackTrace()`, la causa original **sí se ve**. La traza principal muestra la excepción de alto nivel y, a continuación, imprime una sección identificada como `"Caused by: "` seguida de la traza de la excepción original de bajo nivel, permitiendo una depuración completa.

```java
// Definición de una excepción personalizada
class FalloConexionServidorException extends Exception {
    // El constructor recibe el mensaje y la causa original (Throwable)
    public FalloConexionServidorException(String mensaje, Throwable causa) {
        super(mensaje, causa); 
    }
}

public class ServicioExterno {
    public void conectar() throws FalloConexionServidorException {
        try {
            // Simulamos una operación de red de bajo nivel
            throw new java.net.SocketTimeoutException("Timeout de 5000ms");
        } catch (java.net.SocketTimeoutException e) {
            // Capturamos la de bajo nivel y lanzamos la personalizada,
            // pasando 'e' como el argumento que representa la causa subyacente.
            throw new FalloConexionServidorException("No se pudo contactar con el proveedor de pagos.", e);
        }
    }
}

```