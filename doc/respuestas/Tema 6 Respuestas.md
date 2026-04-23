<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Genericidad". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: clases y objetos, encapsulación, excepciones, composición, herencia y polimorfismo.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 6. Genericidad

## 1. Empleando `void*` en C o `Object` en Java, pon un ejemplo de una estructura de datos, que empleando un array primitivo, permita alojar cualquier tipo de dato.

Para ilustrar la capacidad de alojar tipos de datos heterogéneos o genéricos sin utilizar herramientas de genericidad propiamente dichas, se presenta a continuación la implementación de una **Pila (Stack)**. En el lenguaje C, esto se logra mediante el uso de punteros genéricos `void*`, mientras que en Java se utiliza la clase base `Object`, de la cual derivan todas las demás clases.
En la implementación en C, se define una estructura que contiene un array de punteros `void*`. Dado que un puntero de este tipo puede apuntar a cualquier dirección de memoria, la estructura es capaz de almacenar referencias a enteros, estructuras complejas o caracteres indistintamente. Sin embargo, esta flexibilidad delega toda la responsabilidad de la gestión de memoria y del control de tipos al programador, aumentando el riesgo de errores de acceso si no se realiza un seguimiento estricto de qué tipo de dato hay en cada posición.

```c
#include <stdio.h>

struct PilaC {
    void* elementos[10];
    int tope;
};

void push(struct PilaC* p, void* elemento) {
    if (p->tope < 10) {
        p->elementos[p->tope++] = elemento;
    }
}
```
En el caso de Java, se emplea un array de tipo `Object[]`. Gracias al polimorfismo por herencia, cualquier objeto (ya sea un `String`, un `Integer` o una instancia de una clase propia) puede ser asignado a una referencia de tipo `Object`. Al igual que en C, el array permite mezclar diferentes tipos de objetos, pero para recuperar los métodos específicos de cada clase al extraer un elemento, se requiere realizar un **downcasting** explícito.

```java
public class PilaJava {
    private Object[] elementos;
    private int tope;

    public PilaJava(int capacidad) {
        elementos = new Object[capacidad];
        tope = 0;
    }

    public void apilar(Object elemento) {
        elementos[tope++] = elemento;
    }

    public Object desapilar() {
        return elementos[--tope];
    }
}
```

Es relevante destacar que este enfoque presenta una limitación importante en cuanto a la **seguridad de tipos**. Al utilizar `Object[]`, el compilador de Java no puede impedir que se introduzca un objeto de un tipo inesperado en la pila. Esto obliga a realizar comprobaciones adicionales mediante el operador `instanceof` antes de procesar los datos extraídos, para evitar la excepción `ClassCastException` durante la ejecución del programa.

## 2. Brevemente, ¿Qué significa la **programación genérica**? ¿Es el ejemplo anterior un ejemplo básico de programación genérica? 

La **programación genérica** se define como un paradigma de programación centrado en el diseño de algoritmos y estructuras de datos que son independientes de los tipos de datos específicos sobre los que operan. El objetivo principal es permitir que el programador escriba una lógica única —como una ordenación o una búsqueda— que funcione de manera idéntica para una amplia variedad de tipos, sin sacrificar la eficiencia ni la legibilidad. En lenguajes modernos, esto se logra mediante la parametrización, donde el tipo se convierte en una variable que se resuelve en el momento de la declaración o instanciación.

Este enfoque busca maximizar la **reutilización del código** y minimizar la redundancia. En lugar de mantener múltiples versiones de una clase (por ejemplo, una `ListaEnteros` y una `ListaCadenas`), se define una única plantilla que describe el comportamiento general. La programación genérica actúa como un contrato donde se especifica cómo se manipulan los datos, delegando la definición del "qué" es el dato concreto a quien utiliza la estructura o el método.

Respecto al ejemplo anterior basado en `void*` o `Object`, no se considera programación genérica en el sentido estricto del término, sino más bien una forma de **programación polimórfica basada en la herencia** (en Java) o en la **omisión de tipos** (en C). Aunque ambos métodos permiten que una estructura aloje cualquier tipo de dato, carecen de la característica fundamental de los genéricos: la vinculación de un tipo específico en la declaración. En el ejemplo del array de `Object`, la estructura "olvida" qué tipo de objeto está manejando, tratándolos a todos simplemente como la raíz de la jerarquía.

La verdadera programación genérica en Java aporta **seguridad de tipos (Type Safety)**, algo que no ocurre en el ejemplo del `Object[]`. Mientras que en el ejemplo anterior el compilador permite mezclar peras con manzanas en la misma bolsa sin rechistar, la programación genérica asegura que si una bolsa se define para peras, solo admita peras. Por tanto, el uso de `void*` u `Object` es un precursor funcional, pero no cumple con los requisitos de robustez y verificación estática que definen al paradigma genérico moderno.

## 3. Indica los problemas respecto al chequeo de tipos, de emplear `void*` o `Object` cuando se crean estructuras de datos genéricas. 

El principal inconveniente de emplear `void*` o `Object` radica en el **aplazamiento de la detección de errores** al tiempo de ejecución. Cuando se utiliza una estructura basada en `Object`, el compilador de Java pierde la capacidad de verificar la coherencia de los datos que se insertan o extraen. Esto significa que es posible introducir accidentalmente un objeto de tipo `Coche` en una estructura destinada a almacenar `Estudiante` sin que se genere ningún aviso, ya que ambas clases heredan de la raíz común.

Esta falta de control estricto obliga al programador a recurrir al **casting explícito** cada vez que se recupera un elemento. Al realizar una conversión de tipo hacia abajo (downcasting), como transformar un `Object` de nuevo a `Integer`, se asume un riesgo constante: si el objeto real no coincide con el tipo esperado, el programa lanzará una excepción `ClassCastException` y se detendrá abruptamente. En lenguajes como C, el uso de `void*` es aún más crítico, pues una conversión incorrecta de punteros puede derivar en accesos a memoria inválidos (segmentation faults) que son extremadamente difíciles de depurar.

Otro problema significativo es la **pérdida de legibilidad y de intención** en la interfaz del programador. Al observar la firma de un método que recibe un `Object`, no queda claro qué tipo de datos espera realmente la lógica interna de la función. Esto debilita el "contrato" entre el desarrollador que crea la estructura y el que la utiliza, forzando a este último a leer la implementación o la documentación para saber qué objetos son seguros de pasar como argumento, eliminando las ventajas de la verificación automática que ofrece un lenguaje tipado.

Finalmente, el uso de tipos universales fomenta prácticas de programación poco robustas, como la creación de estructuras **heterogéneas por accidente**. En la mayoría de los casos de desarrollo de software, se desea que una colección sea homogénea (que todos sus elementos sean del mismo tipo). Al no poder imponer esta restricción mediante el sistema de tipos del compilador, se aumenta la complejidad del código, ya que cada fragmento de lógica que procese la estructura deberá incluir comprobaciones manuales de tipo mediante el operador `instanceof`, lo que ensucia el código y perjudica el rendimiento.

## 4. Vamos entonces con mecanismos de mejora de la programación genérica ¿Qué son los **parámetros de tipo**? 

Los **parámetros de tipo** son el mecanismo central que hace posible la verdadera programación genérica en lenguajes como Java. De forma análoga a cómo un método tradicional recibe parámetros de valor (como un `int` o un `String`) para ejecutar su lógica con distintos datos, una clase o método genérico recibe un tipo como parámetro. Estos se representan convencionalmente mediante letras mayúsculas encerradas entre ángulos (como `<T>` para "Type", `<E>` para "Element" o `<K, V>` para "Key" y "Value"). Un parámetro de tipo actúa como un comodín o una variable estructural que representa a una clase o interfaz real, la cual será especificada más adelante por quien utilice el código.

La gran mejora que introducen estos parámetros radica en trasladar el control y la verificación de los datos desde el tiempo de ejecución hacia el tiempo de compilación. Cuando se instancia una clase genérica proporcionando un tipo concreto (lo que se conoce como **argumento de tipo**), por ejemplo al declarar una `PilaGenerica<String>`, el compilador de Java impone una estricta seguridad de tipos. Se garantiza de forma automática que únicamente se puedan insertar cadenas de texto en esa pila en particular. Este mecanismo elimina por completo la necesidad de realizar conversiones manuales (casting) al recuperar los elementos y, como consecuencia directa, erradica la posibilidad de sufrir fallos críticos por tipos incompatibles durante la ejecución.

Para visualizar esta evolución, se puede transformar la pila basada en `Object` en una estructura verdaderamente genérica. Al sustituir la clase raíz `Object` por el parámetro de tipo `T`, la clase se vuelve dinámica en su declaración pero estrictamente estática y segura en su uso. Quien instancia el objeto establece un "contrato" inquebrantable con el compilador respecto a la familia de datos que alojará esa estructura.

```java
public class PilaGenerica<T> {
    private T[] elementos;
    private int tope;

    // Debido al "borrado de tipos" en Java, los arrays genéricos se instancian 
    // como Object[] y se castean internamente a T[]. 
    // La seguridad se mantiene en la interfaz pública (métodos apilar y desapilar).
    @SuppressWarnings("unchecked")
    public PilaGenerica(int capacidad) {
        elementos = (T[]) new Object[capacidad];
        tope = 0;
    }

    public void apilar(T elemento) {
        elementos[tope++] = elemento;
    }

    public T desapilar() {
        return elementos[--tope];
    }
}
```

## 5. En Java existe "generics", en C++ existen "templates". Pon un ejemplo de uso de programación genérica en ambos, instanciando una lista o vector dinámico que solo admite `String`. Introduce valores, y luego haz un recorrido de ellos mostrando cómo cada elemento es del tipo concreto con seguridad.

Para implementar una lista dinámica que restrinja el tipo de datos a cadenas de texto, en Java se utiliza la clase `ArrayList` junto con la especificación del parámetro de tipo `<String>`. Por otro lado, en C++ se emplea la clase de plantilla `std::vector` de la biblioteca estándar (STL), indicando el tipo entre ángulos como `<std::string>`. En ambos lenguajes, el compilador utiliza esta información para asegurar que solo se inserten objetos compatibles, bloqueando cualquier intento de introducir tipos numéricos u otros objetos en tiempo de compilación.

A continuación se muestra el ejemplo en **Java**:

```java
import java.util.ArrayList;

public class EjemploGenericos {
    public static void main(String[] args) {
        // Instanciación de una lista que solo admite String
        ArrayList<String> listaStrings = new ArrayList<>();
        
        listaStrings.add("Estructura");
        listaStrings.add("Genérica");
        // listaStrings.add(100); // Error de compilación: El tipo no es String

        // Recorrido seguro: cada elemento se extrae directamente como String
        for (String s : listaStrings) {
            System.out.println("Elemento: " + s + " | Mayúsculas: " + s.toUpperCase());
        }
    }
}
```
Y su equivalente en **C++** empleando *Templates*:
```cpp
#include <iostream>
#include <vector>
#include <string>

int main() {
    // Instanciación de un vector que solo admite std::string
    std::vector<std::string> vectorStrings;

    vectorStrings.push_back("Algoritmo");
    vectorStrings.push_back("Eficiente");
    // vectorStrings.push_back(3.14); // Error de compilación: Tipo incompatible

    // Recorrido empleando el tipo concreto sin necesidad de punteros void*
    for (const std::string& s : vectorStrings) {
        std::cout << "Dato: " << s << " | Longitud: " << s.length() << std::endl;
    }

    return 0;
}
```
La diferencia técnica fundamental reside en el procesamiento interno del código. Mientras que Java utiliza el **borrado de tipos (Type Erasure)** y mantiene una única versión del código compilado que internamente maneja `Object` (pero con comprobaciones y castings automáticos invisibles), C++ realiza una **instanciación de plantillas**. Esto significa que el compilador de C++ genera una versión de la clase `vector` específica para `std::string` en el binario final, lo que optimiza el rendimiento al evitar niveles de indirección, a costa de un ligero aumento en el tamaño del ejecutable.

Desde el punto de vista de la seguridad, ambos mecanismos eliminan la ambigüedad que presentaban los ejemplos con `void*` u `Object`. Al recorrer los elementos, el programador tiene la certeza absoluta de que cada objeto extraído es del tipo declarado. Esto permite invocar métodos específicos, como `toUpperCase()` en Java o `length()` en C++, sin riesgo de errores de acceso ilegal a memoria o excepciones de conversión de tipo en tiempo de ejecución.

## 6. Sobre el funcionamiento de la programación genérica. ¿Qué hace el compilador cuando se instancia una clase que tiene parámetros de tipo? ¿Hace lo mismo C++ y Java? ¿Qué es el "type erasure" de Java y la "instanciación de plantillas" de C++?

## 6. Funcionamiento del compilador en la programación genérica

El proceso de compilación de estructuras genéricas varía significativamente entre lenguajes, aunque el objetivo final sea el mismo: proporcionar flexibilidad sin perder el control de tipos. Mientras que en la programación estructurada de C el compilador trataba los tipos de forma rígida y lineal, en la programación genérica el compilador actúa como un supervisor que valida la coherencia entre el tipo solicitado por el programador y las operaciones permitidas sobre dicho tipo. Sin embargo, Java y C++ gestionan esta validación mediante arquitecturas internas opuestas: el **borrado de tipos** y la **instanciación de plantillas**.

En Java, el compilador implementa lo que se denomina **borrado de tipos (Type Erasure)**. Durante la fase de compilación, el sistema verifica que los tipos sean correctos, pero una vez terminada la comprobación, elimina toda la información referente a los parámetros de tipo `<T>`. Estos parámetros se sustituyen por su límite superior (habitualmente la clase `Object`) y el compilador inserta automáticamente las operaciones de **casting** necesarias para que el programador reciba el tipo concreto. El resultado es un único archivo `.class` que sirve para cualquier instanciación de la clase, garantizando que el código sea compatible con versiones antiguas de la Máquina Virtual de Java que no conocían los genéricos.

Por el contrario, C++ utiliza la **instanciación de plantillas**. En lugar de borrar la información de tipo, el compilador genera un fragmento de código máquina específico para cada tipo de dato con el que se instancie la plantilla. Si en un programa se declara un `std::vector<int>` y un `std::vector<std::string>`, el compilador producirá dos versiones distintas de la lógica del vector en el binario ejecutable. Este proceso es similar a cómo operan las macros del preprocesador de C, pero con la diferencia crucial de que el compilador de C++ realiza un análisis sintáctico y semántico completo para asegurar que las operaciones (como sumas o comparaciones) sean válidas para el tipo proporcionado antes de generar el código.

La diferencia fundamental reside en el equilibrio entre el tamaño del ejecutable y el rendimiento. Java prioriza la **compacidad del código**, ya que al haber una sola versión de la clase genérica, el tamaño del archivo binario no aumenta aunque se use con muchos tipos distintos, aunque esto introduce una ligera sobrecarga por el manejo de referencias a `Object`. En cambio, C++ busca la **máxima eficiencia**, puesto que el código generado está optimizado para cada tipo concreto —lo cual es música para los oídos de un desarrollador que busca exprimir cada ciclo del procesador— a costa de un fenómeno conocido como **code bloat**, donde el tamaño del ejecutable crece proporcionalmente a la variedad de tipos utilizados.

## 7. Vamos a crear una nueva clase con parámetros de tipo. Define en Java una clase `Par`, que permite alojar dos valores de tipos diferentes. Incluye un constructor y un getter para cada tipo. Pon un ejemplo de uso de ese `Par`, por ejemplo para especificar el tipo de retorno de una función que devuelve en un `Par` la media y desviación típica de un array de `double`. 

Para permitir que una estructura almacene dos objetos de naturalezas distintas manteniendo la integridad de sus tipos, se recurre a la definición de clases con múltiples **parámetros de tipo**. En Java, esto se logra declarando la clase con una lista de variables de tipo separadas por comas, como `<T, S>`. Esta técnica es la evolución orientada a objetos de las estructuras `struct` de C, con la ventaja crítica de que los tipos no se pierden ni se generalizan a `void*`, sino que se preservan individualmente para cada una de las variables miembro.

A continuación se define la clase `Par`, diseñada para encapsular dos valores de tipos potencialmente diferentes, aplicando los principios de encapsulación mediante atributos privados y métodos de acceso:

```java
public class Par<T, S> {
    private final T primero;
    private final S segundo;

    public Par(T primero, S segundo) {
        this.primero = primero;
        this.segundo = segundo;
    }

    public T getPrimero() {
        return primero;
    }

    public S getSegundo() {
        return segundo;
    }
}
```
El uso de esta clase es especialmente útil para resolver la limitación de los métodos que, por definición, solo pueden devolver un único valor. Mientras que en lenguajes como C se suele recurrir al paso de punteros por referencia para "devolver" múltiples resultados, en Java se puede instanciar un objeto `Par` que empaquete ambos resultados. En el ejemplo propuesto, una función puede calcular estadísticas sobre un array y devolver tanto la media como la desviación típica (ambos de tipo `Double`) dentro de una única estructura.

```java
public class Estadisticas {
    public static Par<Double, Double> calcularResultados(double[] datos) {
        double suma = 0;
        for (double d : datos) suma += d;
        double media = suma / datos.length;

        double sumaCuadrados = 0;
        for (double d : datos) sumaCuadrados += Math.pow(d - media, 2);
        double desviacion = Math.sqrt(sumaCuadrados / datos.length);

        // Se devuelve un único objeto que contiene ambos valores Double
        return new Par<>(media, desviacion);
    }

    public static void main(String[] args) {
        double[] valores = {10.0, 12.0, 23.0, 8.0};
        Par<Double, Double> resultado = calcularResultados(valores);

        System.out.println("Media: " + resultado.getPrimero());
        System.out.println("Desviación: " + resultado.getSegundo());
    }
}
```
Se observa que, al utilizar `Par<Double, Double>`, el compilador garantiza que los valores recuperados mediante los métodos `get` no requieran ninguna conversión manual. Esta aproximación mejora la legibilidad del código y reduce la ambigüedad, ya que la propia firma del método indica claramente que se retornan dos valores de tipo numérico decimal, manteniendo el rigor semántico del programa.


## 8. En Java, se pueden declarar parámetros de tipo también a nivel de método, no solo a nivel de clase. Pon un ejemplo con un método genérico `seleccionaUno`, que pasados dos objetos del mismo tipo, te devuelva aleatoriamente uno de ellos. Muestra la diferencia de definirlo con dos `Object`, a definirlo con dos parámetros de tipo, en terminos de (i) evitar downcasting y (ii) forzar que ambos objetos sean del mismo tipo. 

Los **métodos genéricos** permiten introducir parámetros de tipo de forma independiente a la definición de la clase. Mientras que una clase genérica vincula el tipo a toda la instancia, un método genérico declara su propia variable de tipo (situada antes del tipo de retorno) para ser utilizada únicamente dentro de su ámbito de ejecución. Esto resulta especialmente útil en métodos de utilidad o algoritmos que operan sobre diferentes tipos de datos sin necesidad de que la clase contenedora sea genérica en sí misma, facilitando la creación de librerías de herramientas más flexibles.

Si se define el método empleando la clase `Object`, el sistema pierde la trazabilidad del tipo real de los argumentos. En este escenario, el compilador permite pasar, por ejemplo, un `String` y un `Integer` simultáneamente, ya que ambos heredan de la raíz común `Object`. Al recuperar el objeto seleccionado, el tipo de retorno será obligatoriamente `Object`, lo que fuerza al programador a realizar un **downcasting** manual y arriesgado. Este proceso no solo ensucia el código, sino que delega la seguridad al tiempo de ejecución, donde un error de lógica puede provocar una excepción crítica.

```java
// Versión con Object: Insegura y requiere casting
public static Object seleccionaUno(Object a, Object b) {
    return Math.random() > 0.5 ? a : b;
}

// Ejemplo de uso con Object
String s = (String) seleccionaUno("Hola", 123); // Error potencial en ejecución
```
En contraste, la versión genérica utiliza un parámetro `<T>` que actúa como un contrato de **homogeneidad**. Al invocar el método, el compilador infiere el tipo de los argumentos y se asegura de que ambos coincidan; si se intenta mezclar tipos incompatibles, se genera un error de compilación inmediato. Además, el método devuelve directamente un objeto del tipo `T`, eliminando por completo la necesidad de realizar conversiones manuales. Esto garantiza que si se proporcionan dos objetos de una clase específica, el resultado mantendrá todas las propiedades y métodos de esa clase sin necesidad de comprobaciones adicionales.

```java
// Versión con Generics: Segura y sin casting
public static <T> T seleccionaUno(T a, T b) {
    return Math.random() > 0.5 ? a : b;
}

// Ejemplo de uso con Generics
String s = seleccionaUno("Hola", "Mundo"); // Seguro, T es String
// Integer i = seleccionaUno("Hola", 100); // Error de compilación detectado
```

## 9. ¿Se pueden establecer restricciones en los parámetros de tipo? Por ejemplo, si quiero definir un tipo genérico `<T>`, ¿puedo decir que tenga que ser, al menos, un número para poder tratarlo como tal? Pon un ejemplo en Java de un `Punto` con dos coordenadas, metodos `getX`, `getY`, y una función `calcularDistanciaA` otro `Punto`. Permite que esas coordenadas sean cualquier tipo de número. Pon dos soluciones: una simplemente creando coordenadas de tipo `Number` y otra añadiendo generics para reforzar el chequeo de tipos y saber exactamente con qué tipo de número trabaja el `Punto`. En este caso y respecto al "type erasure", ¿cuál es el tipo final tras la compilación?

Para restringir un parámetro de tipo en Java se utilizan los **tipos acotados (Bounded Type Parameters)** mediante la palabra reservada `extends`. Esto establece un "límite superior", indicando que el tipo genérico debe ser la clase especificada o una de sus descendientes. Gracias a este límite, el compilador permite invocar dentro de la clase genérica los métodos propios de la clase límite (como `doubleValue()` en el caso de `Number`), lo cual sería imposible en un genérico sin restricciones donde solo se podrían usar métodos de la clase raíz `Object`.

A continuación se presentan las dos soluciones propuestas para la clase `Punto`. La primera utiliza el polimorfismo clásico de la clase `Number`, mientras que la segunda emplea la genericidad para asegurar que un punto mantenga la coherencia de su tipo de dato (por ejemplo, que sea exclusivamente un punto de enteros o un punto de decimales), aplicando la fórmula de la distancia euclídea:

$$d = \sqrt{(x_2 - x_1)^2 + (y_2 - y_1)^2}$$

### Solución 1: Uso de polimorfismo con `Number`

```java
public class PuntoPolimorfico {
    private Number x, y;

    public PuntoPolimorfico(Number x, Number y) {
        this.x = x;
        this.y = y;
    }

    public Number getX() { return x; }
    public Number getY() { return y; }

    public double calcularDistanciaA(PuntoPolimorfico otro) {
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

### Solución 2: Uso de parámetros de tipo acotados (`T extends Number`)

```java
public class PuntoGenerico<T extends Number> {
    private T x, y;

    public PuntoGenerico(T x, T y) {
        this.x = x;
        this.y = y;
    }

    public T getX() { return x; }
    public T getY() { return y; }

    // El uso de <T> aquí fuerza a que el otro punto sea del mismo tipo numérico
    public double calcularDistanciaA(PuntoGenerico<T> otro) {
        double dx = this.x.doubleValue() - otro.getX().doubleValue();
        double dy = this.y.doubleValue() - otro.getY().doubleValue();
        return Math.sqrt(dx * dx + dy * dy);
    }
}
```

Respecto al fenómeno del **type erasure**, el tipo final tras la compilación no es `Object`, sino la clase que actúa como límite superior. En este caso, el compilador sustituye todas las instancias de `T` por **`Number`**. Esto asegura que el bytecode sea compatible con la jerarquía de clases de Java, permitiendo que el entorno de ejecución maneje las coordenadas como números genéricos, mientras que la seguridad de tipos específica (que un `Punto<Integer>` no reciba accidentalmente un `Double`) ha sido validada y garantizada previamente por el compilador.

Esta técnica combina la flexibilidad de trabajar con cualquier tipo numérico y el rigor de asegurar que, una vez instanciado el objeto, no se mezclen tipos de forma incoherente. A diferencia del uso de punteros en C, donde la interpretación del dato recae totalmente en la precaución del programador, aquí es la propia estructura del lenguaje la que protege la integridad de la información y automatiza las conversiones de tipo necesarias mediante castings invisibles en el código compilado.

## 10. Sobre las soluciones anteriores. Si bien ambas permiten trabajar con distintos tipos de número sin duplicar la clase `Punto`, reflexiona sobre el refuerzo del chequeo de tipos con generics. ¿Permiten ambas crear un punto con una coordenada de tipo entero y la otra coordenada de tipo real? ¿Qué tipo devuelve el `getX` con la solucion sin generics y qué tipo devuelve el que tiene la solución con generics?

La principal diferencia funcional entre ambas soluciones radica en la **homogeneidad** de los datos. La versión basada en polimorfismo clásico (`Number`) es inherentemente heterogénea; permite crear un punto donde la coordenada `x` sea un `Integer` y la `y` sea un `Double`. Esto es posible porque cada atributo es una referencia independiente a la clase base. Por el contrario, la solución con genéricos (`<T extends Number>`) impone una restricción de uniformidad: al instanciar la clase como `PuntoGenerico<Integer>`, el parámetro `T` se fija para ambos atributos, obligando a que tanto `x` como `y` sean del mismo tipo específico.

En cuanto al comportamiento de los métodos de acceso, existe una distinción fundamental en la información que recibe el programador. En la solución sin genéricos, el método `getX()` devuelve siempre una referencia de tipo **`Number`**. Esto implica que, aunque se sepa que se guardó un entero, el compilador solo garantiza que el objeto es un número, obligando a realizar un casting manual si se desea acceder a métodos específicos de `Integer`. En cambio, en la solución genérica, `getX()` devuelve el **tipo exacto `T`** con el que se instanció el objeto. Si se trabaja con un `PuntoGenerico<Integer>`, el método devolverá directamente un `Integer`, permitiendo su uso inmediato sin conversiones adicionales.

El refuerzo del chequeo de tipos con genéricos actúa, por tanto, como un filtro de coherencia en tiempo de compilación. Mientras que la solución con `Number` trata a todas las coordenadas bajo un "manto de abstracción" que iguala su comportamiento (perdiendo su identidad específica), la genericidad permite que el `Punto` se adapte al tipo de dato concreto sin perder su esencia. Esto evita errores sutiles de lógica, como intentar operar con la precisión de un `Double` sobre un valor que el programador creía erróneamente que era un `Integer`, capturando estas inconsistencias antes de que el programa llegue a ejecutarse.

Finalmente, es importante señalar que la solución genérica ofrece una **autodocumentación** del código mucho más potente. Al observar una declaración de tipo `PuntoGenerico<Float>`, queda explícitamente claro para cualquier desarrollador que ese punto opera con precisión simple. En la versión polimórfica, la intención queda oculta tras la clase `Number`, lo que obligaría a revisar el código de instanciación o a realizar comprobaciones con `instanceof` para determinar la naturaleza real de las coordenadas, aumentando la complejidad técnica y la probabilidad de fallos en el mantenimiento del software.

## 11. Hagamos un ejemplo avanzado. El siguiente código, con interfaz `Punto`, que define un método `calcularDistanciaA(Punto p)`, junto con las implementaciones `Punto2D` y `Punto3D`. Añade generics para asegurarnos que la sobreescritura del método calcular distancia a otro `Punto` siempre es sobre un `Punto` del mismo tipo, evitando `instanceof` y el downcasting.
```java
public interface Punto { 
    public double distanciaA(Punto p); 
} 

public class Punto2D implements Punto { 
     private final double x, y; 
     public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto p) { 
        if (p instanceof Punto2D) { 
            Punto2D p2d = (Punto2D) p; 
            return Math.sqrt(Math.pow(x - p2d.x, 2) 
                    + Math.pow(y - p2d.y, 2)); 
        } else { 
            throw new RuntimeException("p debe ser Punto 2D"); 
        } 
    } 
} 
public class Punto3D implements Punto { 
    // Igual que Punto2D, pero con tres coordenadas
    ...
} 
```

El diseño original de la interfaz presenta una debilidad estructural debido a que el parámetro del método `distanciaA` es demasiado general. Al aceptar cualquier objeto que implemente la interfaz `Punto`, el compilador permite técnicamente pasar un `Punto3D` a un método de `Punto2D`. Esta falta de especificidad obliga al desarrollador a introducir lógica de control manual mediante `instanceof` y conversiones de tipo (casting) que pueden fallar en tiempo de ejecución, rompiendo el principio de seguridad de tipos que busca la programación orientada a objetos.

Para solucionar este problema, se debe convertir la interfaz en una estructura genérica parametrizada por un tipo `<T>`. De esta forma, cada clase que implemente la interfaz puede especificar exactamente qué tipo de objeto es capaz de procesar en sus métodos. Al declarar que `Punto2D` implementa `Punto<Punto2D>`, se está vinculando el contrato de la interfaz al propio tipo de la clase, lo que permite que la sobreescritura del método reciba directamente una instancia de la misma clase.

```java
public interface Punto<T> { 
    public double distanciaA(T p); 
} 

public class Punto2D implements Punto<Punto2D> { 
    private final double x, y; 
    
    public Punto2D(double x, double y) { 
        this.x = x; this.y = y; 
    } 

    @Override 
    public double distanciaA(Punto2D p) { 
        // El compilador garantiza que 'p' es de tipo Punto2D.
        // Se accede directamente a los atributos sin necesidad de casting.
        return Math.sqrt(Math.pow(this.x - p.x, 2) + Math.pow(this.y - p.y, 2)); 
    } 
} 

public class Punto3D implements Punto<Punto3D> { 
    private final double x, y, z; 

    public Punto3D(double x, double y, double z) { 
        this.x = x; this.y = y; this.z = z; 
    } 

    @Override 
    public double distanciaA(Punto3D p) { 
        return Math.sqrt(Math.pow(this.x - p.x, 2) 
                       + Math.pow(this.y - p.y, 2) 
                       + Math.pow(this.z - p.z, 2)); 
    } 
}
```
Esta aproximación traslada la responsabilidad de la validación del programador al compilador. Si se intentara calcular la distancia entre un punto de dos dimensiones y uno de tres dimensiones, el código simplemente no compilaría, eliminando la necesidad de lanzar excepciones en tiempo de ejecución. Además, el código resulta mucho más limpio y eficiente, ya que se eliminan las comprobaciones de jerarquía de clases y se permite que el entorno de ejecución trabaje con tipos concretos y seguros.

## 12. Dado que `String` es subtipo de `Object`, ¿significa eso que `List<String>` es subtipo de `List<Object>`? ¿Y que `String[]` es subtipo de `Object[]`? Razona por qué la respuesta es diferente en cada caso y qué problema en tiempo de ejecución puede aparecer con los arrays. A partir de estos ejemplos, define qué significa que un tipo genérico sea **covariante**, **contravariante** o **invariante** respecto a su parámetro de tipo.

Aunque `String` es un subtipo de `Object`, la relación no se transfiere de la misma manera a las listas que a los arrays. En Java, los tipos genéricos son **invariantes**, lo que significa que `List<String>` **no** es un subtipo de `List<Object>`. Si esta relación fuera permitida, se podría asignar una lista de cadenas a una referencia de lista de objetos y, a través de esta última, insertar un `Integer`. Esto rompería la seguridad de tipos, provocando errores al intentar recuperar elementos de la lista original esperando que todos fueran cadenas. El compilador de Java impide esta asignación para garantizar que una colección homogénea se mantenga íntegra.

Por el contrario, los arrays en Java son **covariantes**, por lo que `String[]` **sí** es un subtipo de `Object[]`. Esta decisión de diseño se tomó en las primeras versiones del lenguaje para permitir que métodos generales (como los de ordenación) pudieran aceptar cualquier array de objetos antes de que existieran los genéricos. Sin embargo, esta flexibilidad introduce un riesgo de error en tiempo de ejecución: si se utiliza una referencia de `Object[]` que apunta a un array real de `String` para intentar almacenar un `Integer`, la Máquina Virtual de Java lanzará una excepción de tipo `ArrayStoreException`. A diferencia de los genéricos, los arrays conservan información sobre su tipo en tiempo de ejecución y protegen su contenido de inserciones incompatibles.

A partir de estos comportamientos, se pueden definir formalmente las tres variantes de relación entre tipos complejos $G$ basados en una jerarquía de tipos donde $A$ es subtipo de $B$ ($A \subseteq B$):

* **Invariante:** No existe relación de subtipado entre $G<A>$ y $G<B>$. Es el comportamiento por defecto de los genéricos en Java; una caja de manzanas no es una caja de frutas, es simplemente una caja de manzanas.
* **Covariante:** Se preserva la relación de subtipado, por lo que $G<A>$ es subtipo de $G<B>$. Esto ocurre en los arrays de Java y permite que una "fila de manzanas" sea tratada como una "fila de frutas".
* **Contravariante:** Se invierte la relación de subtipado, de modo que $G<B>$ se convierte en subtipo de $G<A>$. En Java, esto se logra mediante el uso de comodines (wildcards) con la palabra reservada `super`, como en `List<? super String>`, permitiendo que una estructura acepte cualquier superclase de un tipo dado.

## 13. Java permite recuperar covarianza y contravarianza en tipos genéricos de forma controlada mediante **wildcards**. ¿Qué es un wildcard (`?`)? Muestra la diferencia entre `List<? extends T>` y `List<? super T>`, indicando en qué casos se usa cada uno. Pon dos ejemplos: (i) un método que reciba una lista de números y calcule su suma, usando `? extends`; (ii) un método que reciba una lista y le añada varios números enteros, usando `? super`.

Los **comodines o wildcards**, representados por el símbolo `?`, son una herramienta de Java diseñada para flexibilizar la rigidez de la invarianza en los tipos genéricos. Mientras que una `List<Number>` solo acepta exactamente ese tipo, una lista con comodines representa un tipo desconocido que cumple con ciertas restricciones. Esto permite que el código sea más reutilizable, aceptando familias de tipos en lugar de un único tipo concreto, lo que resulta esencial al diseñar librerías o APIs que deben interactuar con diferentes jerarquías de clases.

El uso de `List<? extends T>` establece un **límite superior** y habilita la **covarianza**. Indica que la lista puede ser de tipo `T` o de cualquier subclase de `T`. Esta estructura se comporta como un "productor" de datos: se puede leer de ella con total seguridad, ya que cualquier elemento extraído será, como mínimo, de tipo `T`. Sin embargo, el compilador prohíbe añadir elementos a la lista (excepto `null`), porque no puede garantizar si la lista real es de tipo `T`, de una subclase `A` o de una subclase `B`.

Por otro lado, `List<? super T>` establece un **límite inferior** y habilita la **contravarianza**. Especifica que la lista puede ser de tipo `T` o de cualquier superclase de `T` (hasta llegar a `Object`). Este formato actúa como un "consumidor" de datos: se pueden insertar objetos de tipo `T` con total seguridad, ya que cualquier lista que sea superclase de `T` podrá alojarlos. No obstante, al leer de ella, solo se garantiza que los elementos son de tipo `Object`, perdiendo la información específica del tipo original.

Esta dualidad se resume en el principio **PECS (Producer Extends, Consumer Super)**. Se emplea `extends` cuando el objetivo es obtener o leer información de la estructura, y se emplea `super` cuando el propósito es introducir o escribir datos en ella. A continuación, se presentan los ejemplos solicitados:

```java
import java.util.List;
import java.util.ArrayList;

public class EjemploWildcards {

    // (i) Método Productor: Solo lee (extends)
    public static double sumarElementos(List<? extends Number> lista) {
        double suma = 0.0;
        for (Number n : lista) {
            suma += n.doubleValue();
        }
        return suma;
    }

    // (ii) Método Consumidor: Solo escribe (super)
    public static void añadirEnteros(List<? super Integer> lista) {
        lista.add(10);
        lista.add(20);
        lista.add(30);
    }

    public static void main(String[] args) {
        // Uso de extends: acepta List<Integer>, List<Double>, etc.
        List<Integer> enteros = List.of(1, 2, 3);
        System.out.println("Suma: " + sumarElementos(enteros));

        // Uso de super: acepta List<Integer>, List<Number>, List<Object>
        List<Number> numeros = new ArrayList<>();
        añadirEnteros(numeros); 
        System.out.println("Lista tras añadir: " + numeros);
    }
}
```
