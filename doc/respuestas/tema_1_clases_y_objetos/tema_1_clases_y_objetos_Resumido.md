# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

**Abstracción**: Su proposito es poder olvidar detalles, con el fin de 

- Manejar temas complejos mas facilmenénte, olvidando detalles que no son importantes para la resolución del problema
- Mejorar la experiencia de lectura y mantenimiento del codigo 

**Encapsulación**: Sirve para

- Agrupar datos y métodos dentro de una clase, protegiendo la integridad del estado interno del objeto al restringir el acceso directo 
- Ocultar detalles de implementación, permitiendo interactuar con él solo a través de una interfaz pública definida

**Herencia**: Crea jerarquias

- Permite que un una "subclase" extienda una clase heredando asi la logica de esta lo que ayuda a escribir menos codigo repetitivo ya que solo se escriben las funciones exclusivas a esa nueva clase.

**Polimorfismo**: Permite usar la misma función para distintas implementaciones usando *dyninamic binding* esto quiere decir que segun la cantidad o tipo de parametros que se le pase a una función hará una cosa u otra


## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Tipado Estático | Compilado (Nativo) | Sin GC (Manual/Ownership)

    C++

    Rust

    Ada

Tipado Estático | Compilado (Nativo) | Con GC

    Go

    D

Tipado Estático | Compilado (Nativo) | ARC (Conte de Referencias)

    Swift

    Objective-C

    Delphi / Object Pascal

Tipado Estático | Híbrido (Bytecode/VM) | Con GC

    Java

    C#

    Kotlin

    Scala

Tipado Dinámico | Interpretado (o JIT) | Con GC

    Python

    JavaScript

    Ruby

    PHP

    Smalltalk

    Groovy


## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

1. Código no estructurado (Ensamblador) Secuencias lineales de instrucciones y control de flujo gestionado mediante saltos incondicionales y arbitrarios (GOTO).

2. Programación Estructurada Organización lógica del algoritmo restringiendo el flujo de ejecución exclusivamente a tres estructuras de control: secuencia, selección e iteración.

3. Programación Modular Descomposición del sistema en unidades funcionales independientes (módulos) con interfaces definidas para agrupar procedimientos y datos relacionados.


## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos? 

Identidad: Propiedad única que distingue a un objeto de cualquier otro, independientemente de su contenido (ej. dirección de memoria).

Estado: Conjunto de valores concretos que tienen los atributos del objeto en un instante determinado.

Comportamiento: Acciones y respuestas a mensajes que el objeto puede ejecutar, definidas por sus métodos.


## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Aquí tienes la respuesta esquemática cubriendo los cuatro puntos de la pregunta:

**Clase:** Es la plantilla o "blueprint" estática que define la estructura (atributos) y el comportamiento (métodos) comunes a un tipo de entidad.

**Diferencia con Objeto:** No son lo mismo. La clase es el concepto abstracto (el molde), mientras que el objeto es la entidad concreta que ocupa memoria (la pieza fabricada).

**Instancia:** Término técnico que designa la materialización específica de una clase en tiempo de ejecución; funcionalmente es sinónimo de objeto.

**Universalidad:** No es un concepto universal. Existen lenguajes orientados a objetos basados en prototipos (como JavaScript, Self o Lua), donde no hay clases y los objetos se crean clonando y extendiendo otros objetos existentes.


## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

### Respuesta


## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

**Método:** Es una subrutina o función encapsulada dentro de una clase que define el comportamiento de los objetos. Es el mecanismo utilizado para interactuar con el objeto y modificar su estado interno.

**Sobrecarga de métodos (Overloading):** Es un tipo de polimorfismo estático (en tiempo de compilación) que consiste en definir múltiples métodos con el mismo nombre dentro de la misma clase. Para que sea válido, deben diferenciarse por su firma (es decir, variar en el número, tipo u orden de sus parámetros).


## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

### Respuesta


## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

### Respuesta

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

### Respuesta


## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

### Respuesta


## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

### Respuesta


## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

### Respuesta


## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

### Respuesta


## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

### Respuesta


## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?


### Respuesta


## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

### Respuesta
