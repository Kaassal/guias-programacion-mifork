# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden.

Implementa es-un

Vease B es-un A


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 

### Respuesta

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.

### Respuesta

## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.

### Respuesta


## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.

### Respuesta


## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.

### Respuesta


## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?

### Respuesta


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?

Una clase puede tener mas de una superclase

En java no se permite la herencia multiple


## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

### Respuesta


## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?

Herencia impone una dependecia muy fuerte de las clases que heredan respecto a la superbase. Es menos flexible ya que B y C siempre son un A. 

Mientras que en composicion pueden usar A en un momento y otra clase en otro momento, es decir la referencia de BC -> A puede cambiar durante la ejecución


## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?


¿Por qué favorecer la composición frente a la herencia?

**Rigidez vs. Flexibilidad:** La herencia es estática; se define al compilar y no se puede cambiar la clase padre de un objeto mientras el programa se ejecuta. La composición es dinámica; permite cambiar el comportamiento en tiempo de ejecución simplemente sustituyendo el objeto interno por otro.

**Acoplamiento:** La herencia crea un acoplamiento fuerte (una clase hija depende profundamente de la implementación de la clase padre). La composición genera un acoplamiento débil (la clase solo usa métodos a través de una interfaz).

**Mantenimiento:** La composición fomenta clases más pequeñas, cohesivas y fáciles de probar, evitando arrastrar todo el código y la complejidad de una jerarquía de herencia profunda.



Imagina un videojuego donde un personaje ataca.

1. El problema con la Herencia (Rígido)
Si usamos herencia, el comportamiento queda fijado para siempre al crear el objeto. Un Guerrero no puede decidir usar magia de repente porque su clase ya está definida.
```Java

abstract class Personaje {
    public abstract void atacar();
}

class Guerrero extends Personaje {
    @Override
    public void atacar() {
        System.out.println("Ataca con una espada.");
    }
}

// Uso:
Personaje p = new Guerrero();
p.atacar(); 
// Si ahora quiero que ataque con un arco, no puedo. 
// Tendría que crear un nuevo objeto de clase Arquero.
```

2. La solución con Composición (Flexible)
Al usar composición ("tiene-un" arma en lugar de "es-un" tipo específico de atacante), el comportamiento se puede intercambiar en plena ejecución.

```Java

// Interfaz que define el comportamiento
interface Arma {
    void usar();
}

class Espada implements Arma {
    public void usar() { System.out.println("Ataca con una espada."); }
}

class Arco implements Arma {
    public void usar() { System.out.println("Dispara una flecha."); }
}

// La clase usa la composición ("tiene-un" Arma)
class Personaje {
    private Arma arma;

    // Se inyecta el comportamiento inicial
    public Personaje(Arma arma) {
        this.arma = arma;
    }

    // ¡La magia de la composición! Se puede cambiar en tiempo de ejecución
    public void cambiarArma(Arma nuevaArma) {
        this.arma = nuevaArma;
    }

    public void atacar() {
        arma.usar(); // Delega la acción al objeto compuesto
    }
}

// Uso:
public class Main {
    public static void main(String[] args) {
        // Nace con una espada
        Personaje p = new Personaje(new Espada());
        p.atacar(); // "Ataca con una espada."

        // Cambia el comportamiento en tiempo de ejecución dinámicamente
        p.cambiarArma(new Arco());
        p.atacar(); // "Dispara una flecha."
    }
}

```

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?

### Respuesta


## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.

### Respuesta
