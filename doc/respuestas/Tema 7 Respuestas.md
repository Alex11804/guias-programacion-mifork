<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Aspectos funcionales". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia, polimorfismo y genericidad.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 7. Aspectos funcionales

## 1. ¿Qué es un puntero a una función? Pon un ejemplo de código en C, donde se define una función y que reciba una cadena de caracteres como parámetro y devuelva la cadena en mayúsculas. Crea un puntero en una variable local a dicha función llamado `aMayusculas` e invócala con el puntero.

En el lenguaje C, un **puntero a una función** se define como una variable que almacena la dirección de memoria del punto de entrada de una función. A diferencia de los punteros convencionales que apuntan a datos (como un `int` o un `char`), estos permiten tratar el comportamiento como una entidad que puede ser referenciada, asignada a variables o pasada como argumento a otras funciones. Esta característica es la base de los mecanismos de *callbacks* y es el antecedente conceptual más directo a las expresiones lambda en lenguajes de alto nivel.

La declaración de este tipo de punteros requiere especificar de forma estricta la **firma de la función**, es decir, el tipo de dato que retorna y los tipos de sus parámetros. El nombre del puntero debe ir entre paréntesis precedido por un asterisco para indicar que se trata de una referencia a una función y no de una función que devuelve un puntero. Una vez inicializado con el nombre de una función existente, el puntero puede invocarse de la misma manera que la función original, facilitando la creación de código más genérico y flexible.

A continuación, se presenta la implementación de la función para convertir a mayúsculas y el uso del puntero solicitado:

```c
#include <stdio.h>
#include <ctype.h>

// Definición de la función
char* convertirAMayusculas(char* cadena) {
    for (int i = 0; cadena[i] != '\0'; i++) {
        cadena[i] = (char)toupper((unsigned char)cadena[i]);
    }
    return cadena;
}

int main() {
    char texto[] = "hola mundo";

    // Declaración del puntero a la función e inicialización
    char* (*aMayusculas)(char*) = convertirAMayusculas;

    // Invocación a través del puntero
    aMayusculas(texto);

    printf("Resultado: %s\n", texto);

    return 0;
}
```
Es relevante observar que, aunque en C se trabaja directamente con direcciones de memoria, este concepto permite entender la evolución hacia Java. Mientras que en C se maneja el puntero `aMayusculas` de forma explícita, en el paradigma funcional de Java se utilizaría una interfaz funcional (como `UnaryOperator<String>`) para lograr un comportamiento análogo, encapsulando la dirección de la lógica bajo una estructura de objeto más segura y abstracta.</String>

## 2. ¿Qué es una **función lambda** en un lenguaje de programación? Pon un ejemplo similar al anterior en Javascript y otro en Java con funciones lambda. Usa una variable local `aMayusculas` para apuntar a la función lambda. Por simplicidad, en Java, emplea `Function<String, String>` para el tipo de la referencia a la función lambda.
Una función lambda se define como una **función anónima**, es decir, una unidad de lógica que no posee un identificador o nombre propio. En términos conceptuales, representa la evolución de los punteros a funciones vistos en C, permitiendo encapsular un comportamiento específico de manera compacta para ser tratado como una variable más. Esta capacidad de tratar las funciones como "datos" permite que el código sea más flexible, facilitando la programación declarativa donde se indica qué se desea obtener en lugar de detallar cada paso del control de flujo.

En la práctica, estas expresiones se componen de una lista de parámetros, un símbolo de flecha y un cuerpo que contiene la ejecución. Al no requerir una declaración formal de un método dentro de una clase (en el caso de Java) o de una función global, se reduce significativamente el código redundante o "boilerplate". Se emplean principalmente para operaciones breves que se pasan como argumentos a funciones de orden superior, como aquellas encargadas de transformar o filtrar colecciones de datos.

A continuación se presentan los ejemplos solicitados en ambos lenguajes:

**Ejemplo en JavaScript:**
```javascript
// Definición de la función lambda y asignación a la variable local
const aMayusculas = (cadena) => cadena.toUpperCase();

// Invocación de la función
console.log(aMayusculas("hola mundo"));
```

**Ejemplo en Java:**
```java
import java.util.function.Function;

public class EjemploLambda {
    public static void main(String[] args) {
        // Definición de la lambda usando la interfaz funcional de la biblioteca estándar
        Function<String, String> aMayusculas = (cadena) -> cadena.toUpperCase();

        // Invocación mediante el método apply
        String resultado = aMayusculas.apply("hola mundo");
        
        System.out.println(resultado);
    }
}
```
Es importante notar que, mientras en JavaScript la variable `aMayusculas` simplemente almacena la función, en Java se requiere de una **interfaz funcional** (en este caso `Function<String, String>`) para dar soporte al sistema de tipos. El uso de la genericidad en la interfaz de Java permite especificar que la entrada será de tipo `String` y la salida también, manteniendo la seguridad de tipos que caracteriza al lenguaje.</String,>

## 3. ¿Qué es el **paradigma funcional**? ¿Por qué a algunos lenguajes orientados a objetos como Java 8, se les llama multi-paradigma? ¿Qué quiere decir que las funciones son "ciudadanos de primera clase"?

El **paradigma funcional** se fundamenta en el tratamiento de la computación como la evaluación de funciones matemáticas, evitando el cambio de estado y la mutación de datos. A diferencia del modelo imperativo de C, donde el programador describe detalladamente los pasos para modificar la memoria, este enfoque es **declarativo**, centrándose en definir qué es el resultado a través de transformaciones lógicas. Sus pilares fundamentales son la inmutabilidad (los datos no se modifican tras su creación) y el uso de **funciones puras**, las cuales garantizan que para una misma entrada siempre se obtendrá la misma salida, sin generar efectos secundarios en el resto del sistema.

Se denomina a lenguajes como Java 8 **multi-paradigma** porque permiten integrar y combinar características de distintos modelos de programación dentro de un mismo entorno. Históricamente, Java era un lenguaje estrictamente orientado a objetos, donde cualquier comportamiento debía estar encapsulado obligatoriamente dentro de una clase. Al introducir elementos funcionales, como las expresiones lambda, el lenguaje ofrece la flexibilidad de utilizar la **herencia y el polimorfismo** para estructurar la arquitectura del sistema, mientras se emplea el **paradigma funcional** para realizar operaciones de procesamiento de datos de manera más compacta, legible y fácil de paralelizar.

El concepto de que las funciones son **"ciudadanos de primera clase"** (*first-class citizens*) significa que el lenguaje las trata con los mismos privilegios y capacidades que a cualquier otro tipo de dato, como un entero o una referencia a un objeto. En la práctica, esto implica que una función puede ser asignada a una variable local, ser pasada como parámetro a otros métodos o incluso ser el valor de retorno de otra función. Si bien en C los punteros a funciones permitían una aproximación a este concepto, en el paradigma funcional moderno esta capacidad está integrada de forma nativa y segura, facilitando la creación de **funciones de orden superior** que pueden manipular el comportamiento de otras partes del código como si de simples datos se tratase.

## 4. Explica la sintaxis básica de una función lambda en Java.

La sintaxis de una expresión lambda en Java se estructura fundamentalmente en tres componentes: los parámetros de entrada, el operador de flecha `->` y el cuerpo de la función. El operador actúa como un separador lógico que indica que los parámetros de la izquierda se utilizarán para ejecutar la lógica definida a la derecha. Este diseño elimina la necesidad de declarar formalmente un método (con sus modificadores de acceso y nombre), permitiendo que el comportamiento se defina de manera compacta en el mismo lugar donde se requiere.

En cuanto a los **parámetros**, el lenguaje ofrece flexibilidad mediante la **inferencia de tipos**, lo que permite omitir el tipo de dato si el compilador puede deducirlo del contexto. Si la función solo recibe un parámetro, los paréntesis son opcionales; sin embargo, si no recibe ninguno o recibe varios, su uso es obligatorio. Por ejemplo, en una función que sume dos valores, se escribiría `(a, b) -> ...`, delegando en Java la tarea de identificar que `a` y `b` corresponden a los tipos definidos en la interfaz funcional correspondiente.

El **cuerpo de la lambda** puede presentarse de forma simple o compuesta. Si la lógica consiste en una única expresión, no es necesario utilizar llaves `{}` ni la palabra reservada `return`, ya que el resultado se devuelve de forma implícita. En situaciones donde se requiere una lógica más compleja con múltiples sentencias, se debe encerrar el código entre llaves y especificar el valor de retorno explícitamente, manteniendo una estructura similar a la de un bloque de código convencional en C o Java estándar.

```java
// Sintaxis con un solo parámetro y cuerpo simple
nombre -> "Hola, " + nombre;

// Sintaxis sin parámetros
() -> System.out.println("Ejecución sin argumentos");

// Sintaxis con múltiples parámetros, tipos explícitos y bloque de código
(int x, int y) -> {
    int resultado = x + y;
    return resultado * 2;
};
```

## 5. Ahora recibamos una función como parámetro a un método y la llamaremos desde dentro. Amplia los ejemplos anteriores de Java y JavaScript con un método llamado `transformar`, que reciba un `String` como parámetro y luego una función transformadora como lo es `aMayúsculas` y la invoque desde dentro.

La capacidad de pasar una función como argumento a otra se conoce formalmente como **funciones de orden superior**. Este concepto permite desacoplar la estructura de un algoritmo (el control de flujo) de la lógica específica que se desea aplicar (el comportamiento). En lugar de escribir múltiples métodos para diferentes transformaciones de texto, se define un único método genérico que delega la responsabilidad de la transformación a la función recibida por parámetro.

En Java, este proceso requiere el uso de una **interfaz funcional** como tipo del parámetro. Al invocar el método, se pasa la referencia de la lambda y, dentro del cuerpo del método receptor, se utiliza el método abstracto definido en la interfaz (como `apply` en el caso de `Function`) para ejecutar la lógica. Para un programador con base en C, este patrón es conceptualmente idéntico a pasar un puntero a una función a una rutina de procesamiento, pero con la seguridad de tipos que aporta la genericidad.

A continuación se detallan las implementaciones solicitadas:

---

### Ejemplo en JavaScript
En JavaScript, al ser un lenguaje con tipado dinámico, no se requiere definir interfaces. La función se pasa directamente como un objeto más.

```javascript
// Método que recibe una cadena y una función transformadora
function transformar(cadena, funcionTransformadora) {
    return funcionTransformadora(cadena);
}

// Definición de la lógica
const aMayusculas = (s) => s.toUpperCase();

// Invocación pasando la función como parámetro
const resultado = transformar("hola mundo", aMayusculas);
console.log(resultado); // Imprime: HOLA MUNDO
```

---

### Ejemplo en Java
En Java se emplea la interfaz `Function<String, String>`, donde el primer parámetro del genérico indica el tipo de entrada y el segundo el de salida.

```java
import java.util.function.Function;

public class EjemploTransformar {
    public static void main(String[] args) {
        String texto = "hola mundo";
        
        // Se define la lambda
        Function<String, String> aMayusculas = (s) -> s.toUpperCase();

        // Se invoca el método pasando la cadena y la función
        String resultado = transformar(texto, aMayusculas);
        
        System.out.println(resultado);
    }

    /**
     * Método de orden superior que aplica una función a una cadena.
     */
    public static String transformar(String cadena, Function<String, String> funcion) {
        // Se invoca el comportamiento mediante el método apply
        return funcion.apply(cadena);
    }
}
```

Este enfoque de "inyectar" comportamiento mediante parámetros es la base de la extensibilidad en el paradigma funcional. Permite que el método `transformar` sea reutilizado con cualquier otra lógica (como convertir a minúsculas o cifrar el texto) sin necesidad de modificar su código interno, cumpliendo así con el principio de abierto/cerrado de la programación orientada a objetos.

## 6. Ahora, invoca `transformar`, con una nueva función lambda directamente en la llamada a `transformar`, por ejemplo, una función lambda que invierta la cadena. Define la función de inversión justo cuando la estás pasando como parámetro.

La definición de una función lambda directamente en la llamada a un método representa uno de los usos más comunes y potentes del paradigma funcional. Al evitar la creación de una variable local intermedia, se logra una mayor **proximidad léxica**, lo que significa que la lógica específica se encuentra exactamente en el lugar donde se aplica. Este enfoque es especialmente útil para comportamientos que son de un solo uso o cuya implementación es lo suficientemente breve como para no requerir un nombre identificador que la describa fuera de ese contexto.

En Java, el compilador utiliza la **inferencia de tipos** para determinar que la expresión lambda pasada como argumento debe cumplir con el contrato de la interfaz funcional esperada por el método (en este caso, `Function<String, String>`). Al programador solo le corresponde definir la lógica de transformación, la cual, para invertir una cadena, puede apoyarse de forma eficiente en la clase `StringBuilder`. Desde la perspectiva de un desarrollador de C++, esto es equivalente a pasar una función anónima directamente a una rutina de procesamiento sin haberla declarado previamente en el archivo de cabecera o en el cuerpo del programa.

A continuación, se presentan las implementaciones solicitadas:
---
### Ejemplo en JavaScript
En JavaScript, la inversión de una cadena se suele realizar convirtiendo el texto en un arreglo de caracteres, invirtiendo el arreglo y uniéndolo de nuevo.

```javascript
// Invocación con la lambda de inversión definida en la misma línea
const resultado = transformar("reconocer", (s) => s.split('').reverse().join(''));

console.log(resultado); // Imprime: reconocer
```
---
### Ejemplo en Java
Para invertir la cadena en Java, se emplea la funcionalidad integrada en `StringBuilder`, que es más eficiente para manipular caracteres que el uso de concatenaciones simples.

```java
public class EjemploLambdaInmediata {
    public static void main(String[] args) {
        // Se invoca el método definido anteriormente pasando la lógica "al vuelo"
        String resultado = transformar("Estructuras", (s) -> new StringBuilder(s).reverse().toString());
        
        System.out.println(resultado); // Imprime: sarutcurtsE
    }

    public static String transformar(String cadena, Function<String, String> funcion) {
        return funcion.apply(cadena);
    }
}
```
Es interesante notar cómo la sintaxis se vuelve sumamente limpia. En lugar de gestionar la aritmética de punteros para recorrer una cadena de fin a principio como se haría en C, se delega la responsabilidad a una **operación declarativa** que describe la intención del código (invertir) en lugar de los detalles técnicos de implementación del ciclo.</String,>

## 7. ¿Qué se entiende por cierre o "closure" en el contexto de las funciones lambda? Pon un ejemplo en Java de cómo una función lambda es capaz de acceder a una variable local en el contexto donde fue definida. Modifica el ejemplo anterior, creando otra función lambda para transformar una cadena, pero que lo que haga es concatenar a la cadena de entrada otra cadena que está en una variable local definida fuera de la función lambda.

La definición de una función lambda directamente en la llamada a un método representa uno de los usos más comunes y potentes del paradigma funcional. Al evitar la creación de una variable local intermedia, se logra una mayor **proximidad léxica**, lo que significa que la lógica específica se encuentra exactamente en el lugar donde se aplica. Este enfoque es especialmente útil para comportamientos que son de un solo uso o cuya implementación es lo suficientemente breve como para no requerir un nombre identificador que la describa fuera de ese contexto.

En Java, el compilador utiliza la **inferencia de tipos** para determinar que la expresión lambda pasada como argumento debe cumplir con el contrato de la interfaz funcional esperada por el método (en este caso, `Function<String, String>`). Al programador solo le corresponde definir la lógica de transformación, la cual, para invertir una cadena, puede apoyarse de forma eficiente en la clase `StringBuilder`. Desde la perspectiva de un desarrollador de C++, esto es equivalente a pasar una función anónima directamente a una rutina de procesamiento sin haberla declarado previamente en el archivo de cabecera o en el cuerpo del programa.

A continuación, se presentan las implementaciones solicitadas:
---

### Ejemplo en JavaScript
En JavaScript, la inversión de una cadena se suele realizar convirtiendo el texto en un arreglo de caracteres, invirtiendo el arreglo y uniéndolo de nuevo.

```javascript
// Invocación con la lambda de inversión definida en la misma línea
const resultado = transformar("reconocer", (s) => s.split('').reverse().join(''));

console.log(resultado); // Imprime: reconocer
```
---
### Ejemplo en Java
Para invertir la cadena en Java, se emplea la funcionalidad integrada en `StringBuilder`, que es más eficiente para manipular caracteres que el uso de concatenaciones simples.

```java
public class EjemploLambdaInmediata {
    public static void main(String[] args) {
        // Se invoca el método definido anteriormente pasando la lógica "al vuelo"
        String resultado = transformar("Estructuras", (s) -> new StringBuilder(s).reverse().toString());
        
        System.out.println(resultado); // Imprime: sarutcurtsE
    }

    public static String transformar(String cadena, Function<String, String> funcion) {
        return funcion.apply(cadena);
    }
}
```
Es interesante notar cómo la sintaxis se vuelve sumamente limpia. En lugar de gestionar la aritmética de punteros para recorrer una cadena de fin a principio como se haría en C, se delega la responsabilidad a una **operación declarativa** que describe la intención del código (invertir) en lugar de los detalles técnicos de implementación del ciclo.</String,>


## 8. Reflexiona: ¿en qué se diferencia entonces una función lambda de los punteros a funciones que hay en C?

La diferencia fundamental reside en el **nivel de abstracción** y en la naturaleza del elemento referenciado. Mientras que un puntero a función en C es simplemente una variable que almacena una dirección de memoria física hacia una sección de código ejecutable, una expresión lambda en Java es una representación de alto nivel que el compilador traduce en una instancia de un objeto. Esto implica que la lambda no es solo "código", sino una entidad integrada en el ecosistema de la Máquina Virtual de Java (JVM), beneficiándose de la gestión automática de memoria (Garbage Collector) y de la seguridad de tipos, evitando los riesgos de acceso indebido a memoria comunes en C.

Otro factor diferenciador determinante es la capacidad de gestionar el **estado y el contexto**. Como se ha analizado anteriormente, las lambdas en Java permiten la creación de *closures*, capturando variables locales del entorno donde son definidas. Un puntero a función en C carece de esta capacidad de forma nativa; para que una función en C pueda acceder a datos externos que no sean globales, es obligatorio pasarle explícitamente dichos datos como parámetros adicionales (frecuentemente mediante punteros `void*` para simular genericidad). En Java, este proceso es transparente para el desarrollador, lo que facilita un diseño más limpio y modular.

Finalmente, la integración con el **sistema de tipos** marca una brecha significativa en cuanto a la robustez del código. En C, el uso de punteros a funciones puede volverse propenso a errores si las firmas no coinciden exactamente, y el lenguaje permite realizar conversiones de tipo (*casting*) que pueden comprometer la estabilidad del programa. En cambio, Java utiliza las interfaces funcionales y la genericidad para garantizar, en tiempo de compilación, que la lambda cumple estrictamente con el contrato esperado. Esto permite que el lenguaje ofrezca herramientas más potentes, como el API Stream, que sería sumamente complejo de implementar y mantener utilizando únicamente punteros a funciones tradicionales.

## 9. Devolvamos ahora funciones. Creemos ahora una función que sea capaz de crear funciones "descuento". Una función "descuento", decrementa un porcentaje pasado como parámetro. Por simplicidad, usa `Function<Double, Double>` para su tipo. La función `crearDescuento(porcentaje)`, recibe solo el porcentaje de descuento a aplicar y devuelve la función de descuento. Prueba a crear dos descuentos distintos y aplicarlos a una cantidad. Explica la closure en la función descuento.

La capacidad de un método para retornar una función es la otra cara de la moneda de las funciones de orden superior. Este patrón permite crear **fábricas de comportamiento**, donde un método general se parametriza para generar funciones específicas que pueden ser utilizadas más tarde. En Java, esto se logra definiendo el tipo de retorno como una interfaz funcional, lo que permite que el método devuelva una implementación concreta de esa interfaz en forma de expresión lambda.

En este caso, el método `crearDescuento` actúa como un generador de lógica matemática. Al recibir un porcentaje, no realiza el cálculo de forma inmediata, sino que construye y entrega una "receta" (la lambda) que sabe cómo aplicar ese descuento específico a cualquier cantidad futura. Para un programador habituado a la rigidez de las funciones en C, este dinamismo permite una gran flexibilidad, ya que se pueden generar infinitas variantes de una operación sin necesidad de escribir múltiples funciones con nombres distintos.

A continuación se muestra la implementación del generador de descuentos y su aplicación:

```java
import java.util.function.Function;

public class GeneradorDescuentos {
    public static void main(String[] args) {
        // Se crean dos funciones de descuento distintas
        Function<Double, Double> descuentoDiez = crearDescuento(10.0);
        Function<Double, Double> descuentoBlackFriday = crearDescuento(50.0);

        double precioOriginal = 200.0;

        // Se aplican las funciones creadas
        System.out.println("Precio con 10%: " + descuentoDiez.apply(precioOriginal));
        System.out.println("Precio Black Friday: " + descuentoBlackFriday.apply(precioOriginal));
    }

    /**
     * Retorna una función que aplica un porcentaje de descuento.
     */
    public static Function<Double, Double> crearDescuento(double porcentaje) {
        // La lambda captura la variable local 'porcentaje'
        return (precio) -> precio * (1 - porcentaje / 100);
    }
}
```
La **closure** en este ejemplo es fundamental: la lambda devuelta "encapsula" el valor de la variable `porcentaje` en el momento de su creación. Aunque el método `crearDescuento` termina su ejecución y sale de la pila de llamadas (stack frame), la función resultante mantiene una referencia al valor que se le pasó. Esto permite que `descuentoDiez` siempre reste un 10% y `descuentoBlackFriday` un 50%, a pesar de que ambas fueron generadas por el mismo bloque de código. En C, lograr un comportamiento similar requeriría el uso de estructuras de datos persistentes en el montón (*heap*) o variables estáticas, lo que resultaría mucho más complejo de gestionar.

## 10. En Java, que es un lenguaje con comprobación estática de tipos, donde los tipos se declaran, toda función lambda tiene un tipo, que se conoce como **interfaz funcional**. ¿Qué es una **interfaz funcional**? ¿Qué requisitos tiene?

Una **interfaz funcional** se define como el soporte tipado que permite a Java integrar el paradigma funcional dentro de su sistema de objetos. Dado que Java es un lenguaje de tipado estático, toda expresión lambda debe estar asociada a un tipo de dato concreto para que el compilador pueda validar la seguridad de la operación. En lugar de crear un tipo de dato completamente nuevo para las funciones, se optó por reutilizar la estructura de las interfaces, convirtiéndolas en el "contrato" que describe la firma de una lambda (parámetros y valor de retorno).

El requisito técnico indispensable para que una interfaz sea considerada funcional es que posea **exactamente un método abstracto**. Esta característica, conocida como **SAM** (*Single Abstract Method*), es la que permite la correspondencia biunívoca con una lambda: al haber solo un método sin implementar, no existe ambigüedad sobre qué comportamiento está definiendo la expresión anónima. Si se intentara utilizar una lambda con una interfaz que tiene dos o más métodos abstractos, el compilador no podría determinar a cuál de ellos se refiere la lógica proporcionada y generaría un error.

Es importante destacar que la presencia de otros métodos no invalida necesariamente su naturaleza funcional, siempre que estos no sean abstractos. Una interfaz funcional puede contener cualquier número de **métodos predeterminados** (*default*) o **métodos estáticos**, ya que estos ya poseen una implementación propia y no requieren que la lambda los defina. Asimismo, se suele emplear la anotación `@FunctionalInterface` para indicar explícitamente la intención de la interfaz; aunque no es obligatoria, su uso es una buena práctica porque actúa como un seguro, provocando un error de compilación si alguien intenta añadir un segundo método abstracto por error.

| Característica | Requisito / Estado |
| :--- | :--- |
| **Métodos Abstractos** | Exactamente uno (SAM). |
| **Métodos `default`** | Permitidos (cualquier cantidad). |
| **Métodos `static`** | Permitidos (cualquier cantidad). |
| **Anotación `@FunctionalInterface`** | Opcional, pero recomendada para validación. |

Este diseño permite que el programador aproveche la **genericidad** de Java para crear herramientas sumamente flexibles. Por ejemplo, al usar una interfaz funcional genérica, se puede definir un comportamiento que funcione con cualquier objeto, manteniendo la robustez del tipado que se espera en un entorno profesional, pero con la agilidad visual y sintáctica de un lenguaje funcional puro.

## 11. Creemos una interfaz funcional a mano. Por ejemplo, define la interfaz funcional del ejemplo que transforma la cadena en otra. Llámale `Transformador`, que define una función que convierte una cadena de texto (`String`) en otra (`String`).

Para definir una interfaz funcional de forma manual, se debe declarar una interfaz estándar que contenga un único método abstracto. Este método actuará como la "firma" o el contrato que cualquier expresión lambda asignada a dicha interfaz deberá cumplir. Al crear una interfaz propia como `Transformador`, se gana claridad semántica en el código, ya que el nombre de la interfaz y de su método expresan directamente la intención del negocio, a diferencia de las interfaces genéricas estándar como `Function`.

Es una práctica altamente recomendada emplear la anotación `@FunctionalInterface` justo encima de la declaración. Aunque el compilador de Java identificará la interfaz como funcional siempre que tenga un solo método abstracto, esta anotación obliga al compilador a verificar dicha condición. Si accidentalmente se intentara añadir un segundo método abstracto, se generaría un error en tiempo de compilación, protegiendo así la integridad de las lambdas que dependan de ella.

```java
/**
 * Interfaz funcional personalizada para la transformación de textos.
 */
@FunctionalInterface
public interface Transformador {
    // El único método abstracto que define el comportamiento
    String ejecutar(String entrada);
}
```
Una vez definida, la interfaz se utiliza como cualquier otro tipo de dato en Java. Se puede declarar una variable de tipo `Transformador` y asignarle una expresión lambda cuya estructura de parámetros y retorno coincida con el método `ejecutar`. Desde la perspectiva de la programación orientada a objetos, se está creando una implementación anónima de la interfaz, pero con la sintaxis simplificada y legible que ofrece el paradigma funcional.

```java
public class EjemploInterfazManual {
    public static void main(String[] args) {
        // Se instancia la interfaz manual usando una lambda
        Transformador inversor = (s) -> new StringBuilder(s).reverse().toString();

        // Uso del método definido en la interfaz
        String resultado = inversor.ejecutar("Java Funcional");
        System.out.println(resultado);
    }
}
```

## 12. Ahora hagamos la interfaz funcional algo más genérica y empleando generics, para que permita definir un `Transformador` de un tipo en otro. Pon un ejemplo de un transformador que redondea un `Double` en un `Integer`.

La evolución hacia una **interfaz funcional genérica** permite que un solo contrato sea capaz de procesar diferentes tipos de datos, aplicando los conceptos de **genericidad** que ya se conocen en Java. Al sustituir tipos concretos (como `String`) por parámetros de tipo (frecuentemente denominados `<T, R>`, donde *T* es el tipo de entrada y *R* el de retorno), se crea una herramienta universal. Este enfoque es el que utiliza la biblioteca estándar de Java para proporcionar soluciones flexibles que se adaptan a cualquier necesidad de transformación sin tener que reescribir la interfaz para cada nueva clase o estructura de datos.

La implementación requiere definir estos parámetros en la cabecera de la interfaz y utilizarlos en la firma del método abstracto. Al declarar una variable con esta interfaz, es necesario especificar los tipos reales que se van a manejar, lo que permite al compilador realizar una **comprobación estática de tipos** rigurosa. Para un desarrollador con base en C, esto equivale a crear una estructura de procesamiento que no depende de tipos fijos, pero con la ventaja de que la Máquina Virtual de Java garantiza que los datos procesados siempre coincidan con lo esperado, evitando errores de segmentación o interpretaciones erróneas de la memoria.

A continuación se muestra la definición de la interfaz genérica y el ejemplo de redondeo solicitado:

```java
/**
 * Interfaz funcional genérica para transformar un tipo T en un tipo R.
 * @param <T> Tipo de entrada.
 * @param <R> Tipo de resultado.
 */
@FunctionalInterface
public interface TransformadorGenerico<T, R> {
    R transformar(T entrada);
}

// Ejemplo de uso
public class PruebaGenerica {
    public static void main(String[] args) {
        // Se define un transformador que recibe Double y devuelve Integer
        TransformadorGenerico<Double, Integer> redondeador = (d) -> (int) Math.round(d);

        Integer resultado = redondeador.transformar(15.75);
        System.out.println("El número redondeado es: " + resultado); // Imprime: 16
    }
}
```
En el ejemplo del redondeo, se observa cómo la lógica se simplifica al máximo mediante la expresión lambda. La variable `redondeador` queda vinculada a una implementación específica que toma un valor decimal y, mediante la clase `Math` y un *casting* a entero, cumple con el contrato de devolver un `Integer`. Este patrón demuestra la potencia de combinar interfaces funcionales con genéricos: se logra un código altamente reutilizable y legible que mantiene la integridad de los datos en todo momento.

## 13. `Transformador`, en su versión genérica, parece muy útil y reutilizable, hasta el punto de que es igual a una interfaz funcional que ya hay, que es `Function<T, R>`. Muestra las interfaces funcionales predefinidas que hay en Java.

Efectivamente, el lenguaje Java ya provee un conjunto de interfaces funcionales robustas y estandarizadas dentro del paquete `java.util.function`. Estas interfaces cubren los escenarios de uso más comunes, evitando que el desarrollador deba crear sus propias estructuras como `Transformador` para tareas genéricas. Al utilizar estas versiones predefinidas, se garantiza una mayor interoperabilidad entre diferentes librerías y APIs (como el API Stream), permitiendo que piezas de código desarrolladas de forma independiente puedan conectarse sin problemas de compatibilidad de tipos.

Estas interfaces se agrupan principalmente en cuatro categorías fundamentales basadas en su firma y propósito. Un **`Predicate`** evalúa una condición devolviendo un booleano, una **`Function`** transforma un dato en otro, un **`Consumer`** procesa un valor sin retornar nada y un **`Supplier`** genera un valor nuevo. Para un programador con experiencia en genericidad, estas interfaces son el ejemplo perfecto de cómo abstraer el comportamiento mediante tipos parametrizados, permitiendo que la misma estructura lógica sirva para cualquier objeto del sistema.

A continuación se detallan las interfaces principales en la siguiente tabla:

| Interfaz Funcional | Parámetros | Método Abstracto | Propósito |
| :--- | :--- | :--- | :--- |
| **`Predicate<T>`** | `T` | `boolean test(T t)` | Evaluar una condición (filtro). |
| **`Function<T, R>`** | `T` | `R apply(T t)` | Transformar un tipo `T` en un `R`. |
| **`Consumer<T>`** | `T` | `void accept(T t)` | Consumir/procesar un valor (efecto secundario). |
| **`Supplier<T>`** | Ninguno | `T get()` | Proveer o fabricar un valor. |
| **`UnaryOperator<T>`**| `T` | `T apply(T t)` | Caso especial de `Function` donde entrada y salida son iguales. |
| **`BiFunction<T, U, R>`**| `T, U` | `R apply(T t, U u)` | Transformar dos entradas en un resultado. |

---

Es importante mencionar que, además de estas versiones genéricas, Java incluye **especializaciones para tipos primitivos** como `IntPredicate`, `DoubleFunction` o `LongConsumer`. El objetivo de estas variantes es mejorar el rendimiento y optimizar el uso de memoria al evitar el *autoboxing* (la conversión automática entre tipos primitivos y sus clases envolventes como `Integer` o `Double`). Para quienes provienen de C, donde el control sobre el tipo de dato y la eficiencia es crítico, el uso de estas interfaces especializadas resulta ser la opción más cercana al rendimiento nativo del hardware.</T,></T></T></T></T,></T>

## 14. Vamos a ver ejemplos expresivos de funcional en Java. Estudiemos el `List.forEach`, como versión funcional del bucle `for`. Emplea el `forEach` para recorrer una lista de `Integer` y que muestre un mensaje si el entero es positivo.

El método `forEach` representa una transición fundamental del control de flujo externo (el programador gestiona el índice o el iterador) al control de flujo interno (la propia colección gestiona el recorrido). Esta funcionalidad está integrada en la interfaz `Iterable` y acepta como parámetro una interfaz funcional de tipo **`Consumer<T>`**. Al invocar este método, se delega en la lista la responsabilidad de iterar sobre sus elementos, mientras que el programador se limita a proporcionar la lógica que debe aplicarse a cada uno de ellos a través de una expresión lambda.

A diferencia del bucle `for` tradicional de C o el *enhanced for* de Java, el uso de `forEach` fomenta un código más limpio y menos propenso a errores de "fuera de rango" o de manipulación accidental del iterador. En este modelo, el comportamiento definido en la lambda se ejecuta de forma aislada para cada elemento, lo que permite centrarse en la intención del código —qué hacer con el dato— en lugar de en la mecánica del recorrido. Esta abstracción facilita enormemente la lectura y el mantenimiento de las estructuras de control.

A continuación se muestra la implementación para filtrar y mostrar números positivos en una lista de enteros:

```java
import java.util.Arrays;
import java.util.List;

public class EjemploForEach {
    public static void main(String[] args) {
        List<Integer> numeros = Arrays.asList(-5, 10, -2, 8, 0, 15);

        // Se recorre la lista aplicando una acción a cada elemento
        numeros.forEach(n -> {
            if (n > 0) {
                System.out.println("El número " + n + " es positivo.");
            }
        });
    }
}
```
En este ejemplo, la expresión lambda `n -> { ... }` actúa como el **consumidor** de los elementos de la lista. Se observa que, para un programador con base en C, este enfoque simplifica la estructura eliminando la declaración de variables de control (como `int i`), dejando únicamente la lógica condicional necesaria para cumplir con el requisito.

## 15. Repasando el tema de genericidad, fíjate en la firma de `forEach`, ¿por qué se usa `Consumer<? super T>` y no `Consumer<T>`? Explica qué significa **PECS**, y explícalo para el caso de mejorar el ejemplo del método `transformar` la hora de definir el tipo de la función transformadora.

La utilización de `Consumer<? super T>` en el método `forEach` responde a la necesidad de flexibilizar el uso del polimorfismo en las colecciones genéricas. En Java, aunque un `String` sea un `Object`, una `List<String>` no es una `List<Object>`. Sin el uso de comodines (*wildcards*), un consumidor definido para procesar objetos genéricos (`Consumer<Object>`) no podría ser utilizado para recorrer una lista de cadenas, lo cual resultaría contraintuitivo. Al declarar `? super T`, se permite que el método acepte cualquier consumidor capaz de manejar el tipo `T` o cualquiera de sus superclases, aprovechando que una función diseñada para una clase padre puede procesar con seguridad a cualquiera de sus hijos.

El acrónimo **PECS** (*Producer Extends, Consumer Super*) es una regla mnemotécnica fundamental para el diseño de APIs genéricas robustas. Establece que si un parámetro actúa como un **productor** de datos (se leen elementos de él), se debe usar `? extends T` para permitir cualquier subtipo; por el contrario, si un parámetro actúa como un **consumidor** de datos (se le envían elementos para que los procese), se debe usar `? super T`. Este principio garantiza que el código respete el principio de sustitución de Liskov, permitiendo que las estructuras de datos y funciones sean lo más reutilizables posible dentro de la jerarquía de herencia.

Para mejorar el método `transformar` bajo este principio, se debe analizar el rol de la interfaz funcional `Function<T, R>`. En este contexto, la función **consume** un valor de tipo `T` para procesarlo y **produce** un resultado de tipo `R`. Por tanto, para maximizar la compatibilidad, la firma del método debería evolucionar para aceptar una función que pueda consumir "al menos" un `T` y producir "como máximo" un `R`. Esto permite, por ejemplo, utilizar una función que trabaje con `Number` para transformar una lista de `Integer`, devolviendo un resultado que sea un subtipo del esperado.

La firma optimizada del método `transformar` se presentaría de la siguiente manera:

```java
/**
 * Versión mejorada de transformar siguiendo el principio PECS.
 * - La función consume la entrada: ? super T
 * - La función produce el resultado: ? extends R
 */
public static <T, R> R transformar(T entrada, Function<? super T, ? extends R> funcion) {
    return funcion.apply(entrada);
}
```
Al aplicar este cambio, el código gana en **contravarianza** (en la entrada) y **covarianza** (en la salida). Desde la perspectiva de un programador de C++, esto equivale a dotar a las plantillas (*templates*) de una flexibilidad lógica que el sistema de tipos de C++ suele manejar de forma distinta, pero que en Java es vital para que las colecciones y las funciones de orden superior operen de forma armónica con la herencia de clases.

## 16. Referencias a métodos. Podemos obtener una referencia a métodos de objetos o clases. Pon un ejemplo en JavaScript y en Java, de una clase `Persona` con un método `saludar`. En el código principal, crea una `Persona` con un nombre, y obtén una referencia a su método `saludar` en una variable local. Invoca `saludar` con esa referencia a su método `saludar`.

Las **referencias a métodos** constituyen una notación simplificada de las expresiones lambda en aquellos casos donde el cuerpo de la función se limita exclusivamente a invocar un método ya existente. En lugar de definir manualmente el paso de parámetros, se emplea el operador de doble dos puntos (`::` en Java) o la asignación directa (en JavaScript) para delegar la ejecución. Esta técnica no solo mejora la legibilidad al reducir la verbosidad del código, sino que también permite reutilizar la lógica ya encapsulada en las clases, tratando a los métodos como si fueran funciones de primera clase.

Desde una perspectiva técnica, cuando se obtiene una referencia al método de una instancia específica, se crea un vínculo entre esa referencia y el objeto concreto. Esto significa que la referencia "recuerda" sobre qué instancia debe actuar, de forma similar a cómo una *closure* captura una variable local. Mientras que en C un puntero a función es una dirección de memoria cruda, en estos lenguajes modernos la referencia a un método de instancia transporta implícitamente el contexto del objeto (el puntero `this`), garantizando que la ejecución se realice sobre los datos correctos del objeto original.

---

### Ejemplo en JavaScript
En JavaScript, los métodos son funciones que pueden ser referenciadas directamente. Sin embargo, es fundamental utilizar `.bind()` para asegurar que el contexto de `this` siga apuntando a la instancia correcta cuando la función sea invocada fuera del objeto.

```javascript
class Persona {
    constructor(nombre) {
        this.nombre = nombre;
    }

    saludar() {
        console.log(`Hola, mi nombre es ${this.nombre}`);
    }
}

const p = new Persona("Carlos");

// Se obtiene la referencia al método vinculando el contexto de la instancia
const saludarRef = p.saludar.bind(p);

// Invocación a través de la referencia
saludarRef(); 
```

### Ejemplo en Java
En Java, la referencia al método debe asignarse a una interfaz funcional que coincida con la firma del método. En este caso, como `saludar` no recibe parámetros ni devuelve nada, se utiliza la interfaz estándar `Runnable`.

```java
public class Persona {
    private String nombre;

    public Persona(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, mi nombre es " + this.nombre);
    }

    public static void main(String[] args) {
        Persona p = new Persona("Carlos");

        // Referencia al método de una instancia específica
        Runnable saludarRef = p::saludar;

        // Invocación a través de la referencia
        saludarRef.run();
    }
}
```
A diferencia de las lambdas donde se escribiría `() -> p.saludar()`, la referencia `p::saludar` es más directa y descriptiva. Es relevante notar que el compilador de Java realiza la conexión en tiempo de compilación, asegurando que la firma del método de `Persona` sea compatible con el método `run()` de la interfaz `Runnable`.

## 17. ¿Qué tipos de referencias a método se pueden hacer en Java? Pon un ejemplo de referencia a método estático, a constructor, a método de instancia de una instancia concreta y a método de instancia sobre cualquier instancia.

Existen cuatro categorías principales de referencias a métodos en Java, cada una diseñada para simplificar la sintaxis de las expresiones lambda cuando estas simplemente delegan su ejecución a un método o constructor ya definido. Al emplear el operador de doble dos puntos `::`, se indica al compilador que utilice una lógica existente en lugar de definir una nueva de forma anónima. Esta técnica mejora la legibilidad del código al centrarse en el nombre de la operación, lo que resulta especialmente útil en flujos de datos complejos como los que ofrece el API Stream.

La distinción entre estas referencias depende fundamentalmente del contexto y del receptor del método. Mientras que las referencias a **métodos estáticos** y **constructores** son directas y no dependen de un estado previo, las referencias a **métodos de instancia** se dividen según el origen del objeto. Una referencia sobre una **instancia concreta** actúa como un cierre, vinculando la operación a un objeto que ya existe en el entorno léxico. Por el contrario, la referencia sobre una **instancia arbitraria** de un tipo específico interpreta el primer argumento de la función como el objeto sobre el cual se invocará el método, una abstracción que permite aplicar comportamientos a elementos que fluyen dinámicamente a través del programa.

A continuación se presentan ejemplos de cada uno de los cuatro tipos:

```java
import java.util.function.*;
import java.util.ArrayList;
import java.util.List;

public class EjemploReferencias {
    public static void main(String[] args) {
        // 1. Referencia a método estático (ClassName::staticMethodName)
        // Equivalente a: (n) -> Math.abs(n)
        Function<Double, Double> valorAbsoluto = Math::abs;

        // 2. Referencia a un constructor (ClassName::new)
        // Equivalente a: () -> new ArrayList<String>()
        Supplier<List<String>> fabricaDeListas = ArrayList::new;

        // 3. Referencia a método de instancia de una instancia concreta (instance::methodName)
        // Equivalente a: (s) -> prefijo.concat(s)
        String prefijo = "ID-";
        UnaryOperator<String> formateador = prefijo::concat;

        // 4. Referencia a método de instancia sobre una instancia arbitraria de un tipo (ClassName::methodName)
        // El primer argumento de la lambda es el receptor del método.
        // Equivalente a: (String s) -> s.isEmpty()
        Predicate<String> estaVacia = String::isEmpty;

        // Pruebas de ejecución
        System.out.println("Absoluto: " + valorAbsoluto.apply(-10.5));
        System.out.println("Formateado: " + formateador.apply("2026"));
        System.out.println("¿Está vacía?: " + estaVacia.test("Hola"));
    }
}
```
Desde el punto de vista del diseño de sistemas, las referencias a métodos fomentan el principio de reutilización. En lugar de crear múltiples funciones lambda que llamen al mismo método de una clase de utilidad o de negocio, se referencia directamente el método. Para un programador familiarizado con C, esto se puede ver como una forma extremadamente segura y tipada de gestionar punteros a funciones y métodos, donde la Máquina Virtual de Java se encarga de que las firmas y el contexto del objeto sean siempre coherentes.

## 18. Otro ejemplo expresivo. Ordena una lista de `Persona`, cada persona tiene un nombre y una edad (de tipo entero). Ordena la lista de `Persona` con `Collections.sort`, pasándole como comparador una expresión lambda que compare la edad de ambas personas y si tienen la misma edad, se ordene por orden alfabético del nombre. Crea dos versiones: Una con la función de comparación hecha manualmente, y otra empleando `Comparator`.

El proceso de ordenación en Java se apoya en la interfaz funcional `Comparator<T>`, la cual define cómo deben compararse dos objetos de un mismo tipo. En versiones anteriores de Java, era necesario instanciar una clase entera para esta tarea; sin embargo, con la llegada de las funciones lambda, este comportamiento se inyecta directamente en el método de ordenación. Para un programador familiarizado con la función `qsort` de C, este mecanismo resultará natural, con la diferencia fundamental de que Java gestiona la seguridad de tipos y la estructura de los objetos de forma automática, evitando la manipulación de punteros genéricos.

En la **versión manual**, la expresión lambda recibe dos parámetros (las dos personas a comparar) y debe devolver un entero: negativo si la primera es menor, positivo si es mayor y cero si son iguales. Para implementar el criterio doble (edad y luego nombre), se utiliza la lógica de cortocircuito: primero se comparan las edades y, únicamente si el resultado es cero, se procede a comparar los nombres mediante el método `compareTo` de la clase `String`. Este enfoque es potente porque permite un control total sobre la lógica de comparación, similar a escribir el cuerpo de una función de comparación en C pero dentro de un entorno orientado a objetos.

Por otro lado, la **versión con el API Comparator** representa la esencia del paradigma funcional: la composición de funciones. En lugar de escribir la lógica algorítmica, se utilizan métodos estáticos como `comparingInt` y métodos por defecto como `thenComparing` para construir una "cadena de reglas". Esta aproximación es extremadamente legible y declarativa, ya que se lee casi como lenguaje natural. Además, el uso de **referencias a métodos** (`Persona::getEdad`) hace que el código sea aún más compacto, delegando en el lenguaje la responsabilidad de extraer los datos necesarios para realizar la comparación.
---
### Versión 1: Comparación manual con Lambda
```java
import java.util.Collections;
import java.util.List;

// ... dentro del método principal
Collections.sort(personas, (p1, p2) -> {
    // Comparación primaria por edad
    int res = Integer.compare(p1.getEdad(), p2.getEdad());
    
    // Si las edades son iguales (res == 0), se desempata por nombre
    if (res == 0) {
        res = p1.getNombre().compareTo(p2.getNombre());
    }
    return res;
});
```
---
### Versión 2: Empleando el API de Comparator
```java
import java.util.Collections;
import java.util.Comparator;
import java.util.List;

// ... dentro del método principal
Collections.sort(personas, Comparator.comparingInt(Persona::getEdad)
                                     .thenComparing(Persona::getNombre));
```
Esta segunda opción no solo es más breve, sino que reduce la posibilidad de errores lógicos en las comparaciones manuales. Se observa cómo Java permite pasar de un estilo imperativo ("si esto es igual a cero, entonces haz esto otro") a uno puramente declarativo ("compara por edad y luego por nombre"), lo que facilita enormemente el mantenimiento del código a largo plazo.</T>
