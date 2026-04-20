# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

Polimorfismo: Tiene como objetivo exentender programas de forma mas sencilla y segura, por el que las modificaciones vienen principalmente añadiendo nuevas clases, antes que tocando el codigo de las clases existentes

Mecanismos para implementar el polimorfismo:

Java:
 - Sobreescritura de metodos en:
    - Clases abstractas (con sus metodos abstractos)
    - Interfaces
        - No tiene estado (atributos)
        - Todos* los metodos son sin codigo (abstract)
        - Una clase puede implementar varias interfaces

> Fun fact:  En versiones mas recientes de java (>Java 8) se permite dar codigo en los metodos de la interfaz con la keyword: default

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

Ligadura dinamica: Comprobar que metodo se usa en tiempo de ejecucuón

Depende del leguaje, Java y Pyhton usan ligadura dinamica, C++ no 


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

Si, con super 


## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

### Respuesta


## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

### Respuesta


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

### Respuesta


## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

### Respuesta


## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

- Interfaces
    - No tiene estado (atributos)
    - Todos* los metodos son sin codigo (abstract)
    - Una clase puede implementar varias interfaces

> Fun fact:  En versiones mas recientes de java (>Java 8) se permite dar codigo en los metodos de la interfaz con la keyword: default

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `    - Interfaces
        - No tiene estado (atributos)
        - Todos* los metodos son sin codigo (abstract)
        - Una clase puede implementar varias interfacesPunto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

### Respuesta


## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

### Respuesta
