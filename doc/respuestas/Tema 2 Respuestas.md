<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Encapsulación". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 2. Encapsulación

## 1. En Programación Orientada a Objetos (POO), ¿Qué buscan la **encapsulación** y **la ocultación** de información? Enumera brevemente algunas ventajas de la ocultación de información.

En Programación Orientada a Objetos, la encapsulación y la ocultación de información buscan controlar el acceso al estado interno de los objetos y a la forma en que este puede modificarse. La encapsulación consiste en agrupar datos y operaciones relacionadas dentro de una misma unidad, la clase, mientras que la ocultación limita el acceso directo a los detalles internos, exponiendo solo aquello que es necesario para utilizar el objeto correctamente.
Desde esta perspectiva, un objeto se concibe como una entidad que ofrece un conjunto de servicios bien definidos, pero que esconde cómo están implementados internamente. Esto supone un cambio importante respecto a C/C++ sin orientación a objetos, donde es habitual trabajar con estructuras de datos cuyos campos son accesibles directamente y donde el control del acceso depende más de la disciplina del programador que del propio lenguaje.

Entre las ventajas de la ocultación de información se encuentran una mayor robustez, al evitar modificaciones accidentales del estado interno; una mejor mantenibilidad, ya que los cambios internos de una clase no afectan al resto del programa si su interfaz se mantiene; y una mayor claridad en el uso de las clases. Además, se reduce el acoplamiento entre distintas partes del código, facilitando la reutilización y la evolución del software.

## 2. ¿Qué se entiende por la **interfaz pública** de un objeto o clase en POO? Describe brevemente cómo se relaciona con la ocultación de información.

En Programación Orientada a Objetos, la interfaz pública de una clase u objeto es el conjunto de elementos accesibles desde fuera de la clase, principalmente métodos públicos y, en algunos casos, constructores. Esta interfaz define qué operaciones se pueden realizar sobre los objetos de esa clase y cómo puede interactuar el resto del programa con ellos, sin necesidad de conocer los detalles internos de su implementación.
La interfaz pública actúa como un contrato entre la clase y su entorno. Quien utiliza la clase solo necesita saber qué métodos existen, qué parámetros reciben y qué resultados producen, pero no cómo se realizan internamente esas operaciones. Esto permite usar la clase de forma correcta sin depender de su estructura interna, lo que resulta especialmente útil al trabajar con código modular.
La relación con la ocultación de información es directa, ya que todo aquello que no forma parte de la interfaz pública permanece oculto. Los atributos y métodos internos se mantienen inaccesibles desde el exterior, obligando a interactuar con el objeto únicamente a través de la interfaz definida. Gracias a esto, se protege el estado interno del objeto y se refuerza la encapsulación, mejorando la seguridad y el mantenimiento del programa.

## 3. Brevemente: ¿Por qué hay que ser conscientes y diseñar con cuidado la **interfaz pública** de una clase? ¿Es fácil cambiarla?

La interfaz pública de una clase debe diseñarse con cuidado porque define la forma en que esa clase será utilizada por el resto del programa. Una vez que otros módulos dependen de ella, la interfaz se convierte en el punto de contacto estable entre la clase y su entorno. Decisiones poco meditadas pueden obligar a usar la clase de manera incorrecta o generar dependencias innecesarias.

Diseñar bien la interfaz implica exponer solo lo imprescindible y hacerlo de forma clara y coherente. Una interfaz demasiado amplia o mal definida dificulta la comprensión del código y aumenta el riesgo de errores. Además, desde el punto de vista del diseño orientado a objetos, la interfaz pública representa el comportamiento que la clase promete ofrecer, no su implementación interna.

Cambiar la interfaz pública no suele ser fácil, especialmente cuando la clase ya se utiliza en varios puntos del programa. Cualquier modificación puede obligar a revisar y adaptar mucho código existente. Por este motivo, se considera una buena práctica pensar la interfaz pública como algo relativamente estable y evitar cambios frecuentes una vez que la clase forma parte del diseño general del sistema.

## 4. ¿Qué son las **invariantes de clase** y por qué la ocultación de información nos ayuda?

Las invariantes de clase son condiciones que deben cumplirse siempre para que los objetos de una clase se encuentren en un estado válido. Estas condiciones describen relaciones o restricciones sobre los atributos internos y definen qué valores o combinaciones de valores son correctos. Mientras un objeto existe, sus invariantes deben mantenerse verdaderas antes y después de la ejecución de cualquier método público.
Las invariantes representan reglas internas del diseño de la clase. Por ejemplo, pueden indicar que un valor no sea negativo o que ciertos atributos mantengan una relación coherente entre sí. Si una invariante se rompe, el objeto queda en un estado inconsistente y su comportamiento puede volverse impredecible, lo que suele provocar errores difíciles de detectar.

La ocultación de información ayuda a preservar las invariantes porque impide que el código externo modifique directamente los atributos del objeto. Al obligar a acceder al estado interno solo mediante métodos controlados, la clase puede comprobar y garantizar que cualquier cambio respeta las condiciones establecidas. De este modo, se protege la coherencia interna del objeto y se refuerza la fiabilidad del programa.


## 5. Pon un ejemplo de una clase `Punto` en `Java`, con dos coordenadas, `x` e `y`, de tipo `double`, con un método `calcularDistanciaAOrigen`, y que haga uso de la ocultación de información. ¿Cuál es la interfaz pública de la clase `Punto`? ¿Qué significa `public` y `private`?

A continuación se muestra un ejemplo sencillo de una clase `Punto` en Java que representa un punto en el plano mediante dos coordenadas de tipo `double`. La ocultación de información se aplica declarando los atributos como privados y ofreciendo métodos públicos para interactuar con el objeto sin exponer directamente los valores internos.

```java
public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}
```

La interfaz pública de la clase `Punto` está formada por los elementos accesibles desde fuera de la clase, en este caso el constructor `Punto(double x, double y)` y el método `calcularDistanciaAOrigen()`. Estos definen cómo puede utilizarse un objeto `Punto` desde otras partes del programa. Las coordenadas `x` e `y` no forman parte de la interfaz pública, ya que no pueden accederse directamente.

La palabra clave `public` indica que un atributo, método o constructor es accesible desde cualquier otra clase. Por el contrario, `private` restringe el acceso exclusivamente al interior de la propia clase. Gracias a `private`, se evita que el estado interno del objeto sea modificado de forma directa y sin control, obligando a utilizar los métodos definidos por la clase.

Este diseño permite cambiar la implementación interna de la clase sin afectar al código que la utiliza. La ocultación de información protege el estado del objeto, facilita el mantenimiento del programa y ayuda a garantizar que los objetos se usen de forma coherente y segura.

## 6. En Java, ¿A quiénes se pueden aplicar los modificadores `public` o `private`?

En Java, los modificadores de acceso public y private se utilizan para controlar la visibilidad de los distintos elementos que componen una clase. Su objetivo es definir desde dónde pueden ser utilizados dichos elementos y, con ello, reforzar la encapsulación y la ocultación de información dentro del diseño orientado a objetos.

Estos modificadores pueden aplicarse a atributos, métodos y constructores. Un atributo o método declarado como public puede ser accedido desde cualquier otra clase, mientras que si se declara como private solo puede ser utilizado desde el interior de la propia clase. En el caso de los constructores, public permite crear objetos desde cualquier parte del programa, mientras que private restringe la creación de instancias al propio código de la clase.

Además, el modificador public puede aplicarse a la definición de una clase, indicando que es visible desde cualquier otro paquete. Sin embargo, private no puede utilizarse para clases de nivel superior, ya que estas deben ser accesibles al menos desde su propio paquete. En conjunto, el uso adecuado de public y private permite delimitar claramente qué forma parte de la interfaz pública de una clase y qué queda reservado a su implementación interna.


## 7. En POO, la visibilidad puede ser pública o privada, pero ¿existen más tipos de visibilidad? ¿Qué ocurre en Java? ¿Y en otros lenguajes?

En Programación Orientada a Objetos no solo existen las visibilidades pública y privada, sino que pueden definirse niveles intermedios de acceso. Estos niveles permiten ajustar con mayor precisión desde dónde puede utilizarse un atributo o método. La idea es ofrecer distintos grados de encapsulación según las necesidades del diseño, evitando tanto una exposición excesiva como un aislamiento innecesario.

En Java existen cuatro niveles de visibilidad: `public`, `private`, `protected` y la visibilidad por defecto. `public` permite el acceso desde cualquier parte del programa; `private` lo restringe a la propia clase; `protected` permite el acceso desde la misma clase, desde clases del mismo paquete y desde subclases; y la visibilidad por defecto limita el acceso a las clases del mismo paquete.

En otros lenguajes orientados a objetos también existen varios niveles de visibilidad, aunque con diferencias en los detalles. Por ejemplo, en C++ se utilizan `public`, `private` y `protected`, con reglas específicas sobre herencia. En lenguajes como C#, además de estos modificadores, existen otros como `internal`, que limita el acceso al ensamblado. Aunque la terminología y las reglas concretas varían, el objetivo común es el mismo: controlar el acceso y reforzar la encapsulación y reducir el acoplamiento.

## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

Los miembros de instancia declarados como `private` están ocultos para (a) otras clases, pero no para (b) otras instancias de la misma clase. En Java, el modificador `private` restringe el acceso al nivel de clase, no al nivel de objeto. Esto significa que cualquier método definido dentro de una clase puede acceder a los atributos privados de cualquier instancia de esa misma clase.

public class Punto {

    private double x;
    private double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En este método, el acceso a `otro.x` y `otro.y` es válido aunque ambos atributos sean `private`. Esto se debe a que el código que realiza el acceso pertenece a la misma clase `Punto`. La restricción de `private` impide el acceso desde otras clases, pero no impide que diferentes objetos de la misma clase colaboren entre sí.

## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

En los lenguajes orientados a objetos, los métodos **getter** y **setter** son métodos públicos que permiten acceder y modificar, respectivamente, los atributos privados de una clase. Un *getter* devuelve el valor de un atributo, mientras que un *setter* asigna un nuevo valor. Su uso está relacionado con la encapsulación, ya que permiten mantener los atributos ocultos y controlar el acceso a ellos.

El objetivo no es simplemente sustituir el acceso directo a una variable, sino conservar el control sobre el estado interno del objeto. Aunque inicialmente un *getter* o un *setter* pueda limitarse a devolver o asignar un valor, en el futuro puede añadirse lógica adicional, como validaciones o transformaciones, sin cambiar la interfaz pública de la clase.

Permiten decidir qué atributos pueden leerse, cuáles pueden modificarse y bajo qué condiciones, contribuyendo a preservar las invariantes de clase y a reforzar un diseño orientado a objetos coherente y seguro.

## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

Cuando se afirma que la ocultación de información mejora la “seguridad” del programa, no se está haciendo referencia a la seguridad informática en el sentido de evitar ataques externos o impedir que el programa sea “hackeado”. El término se utiliza en un contexto de diseño del software y se refiere a la protección del estado interno de los objetos frente a usos incorrectos dentro del propio programa.
En este contexto, seguridad significa evitar que otras partes del código modifiquen directamente los atributos de una clase de forma arbitraria o inconsistente. Al declarar los atributos como `private` y obligar a interactuar con ellos mediante métodos controlados, se reduce el riesgo de introducir errores lógicos o estados inválidos en los objetos.

Por tanto, la mejora de la seguridad está relacionada con la robustez y fiabilidad del diseño. La ocultación de información ayuda a mantener la coherencia interna de los objetos y a prevenir errores de programación, lo que hace que el sistema sea más estable y mantenible.

## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

Un miembro de instancia es aquel que pertenece a cada objeto concreto creado a partir de una clase. Cada vez que se instancia la clase, se crea una copia independiente de esos miembros. Su valor puede variar de un objeto a otro. Los atributos de instancia representan el estado propio de cada objeto, y los métodos de instancia suelen operar sobre ese estado particular.
Un miembro de clase, en cambio, pertenece a la clase en sí misma y no a las instancias individuales. En Java se declara utilizando la palabra clave `static`. Existe una única copia compartida por todos los objetos de esa clase. Este tipo de miembro se emplea cuando la información o el comportamiento es común a todas las instancias, por ejemplo, un contador global o una constante compartida.

Los miembros de clase también pueden ocultarse. Al igual que los miembros de instancia, pueden declararse como `public` o `private`. Si un miembro de clase se declara como `private`, solo podrá ser utilizado desde el interior de la propia clase, aunque sea compartido por todas las instancias. La ocultación de información se aplica tanto a miembros de instancia como a miembros de clase.

## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

Sí, tiene sentido que los constructores sean `private`, aunque no sea lo habitual en clases de uso general. Declarar un constructor como privado impide que se puedan crear objetos de esa clase desde fuera de ella. De este modo, se controla estrictamente cómo y cuándo se crean las instancias.
Este recurso se utiliza cuando se desea centralizar la creación de objetos, por ejemplo mediante métodos estáticos dentro de la propia clase. Así, la clase puede decidir qué instancias crear, reutilizarlas o limitar su número. También es útil cuando se quiere impedir la creación directa de objetos y forzar el uso de un mecanismo específico de construcción.

Otra situación común es cuando la clase solo contiene miembros estáticos y no tiene sentido crear objetos de ella. En ese caso, declarar el constructor como privado evita que alguien intente instanciarla por error. Un constructor privado es una herramienta válida de diseño para reforzar la encapsulación y el control sobre la creación de instancias.

## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

En Java, los miembros de clase se indican mediante la palabra clave `static`. Esto significa que dichos miembros no pertenecen a cada objeto individual, sino a la clase en sí misma. Existe una única copia compartida por todas las instancias. Se utilizan cuando la información o el comportamiento es común a todos los objetos y no depende del estado particular de uno concreto.

En el caso de la clase `Punto`, los valores máximos de `x` e `y` establecidos hasta el momento no describen a un punto individual, sino al conjunto de todos los puntos creados. Deben modelarse como miembros de clase. Además, pueden declararse como `private` para mantener la ocultación de información y ofrecer métodos públicos de acceso.

public class Punto {

    private double x;
    private double y;

    private static double maxX = Double.NEGATIVE_INFINITY;
    private static double maxY = Double.NEGATIVE_INFINITY;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;

        if (x > maxX) {
            maxX = x;
        }
        if (y > maxY) {
            maxY = y;
        }
    }

    public static double getMaxX() {
        return maxX;
    }

    public static double getMaxY() {
        return maxY;
    }
}
```

En este diseño, `maxX` y `maxY` son miembros de clase porque están declarados como `static`, y su valor es compartido por todos los objetos `Punto`. Los métodos `getMaxX` y `getMaxY` también son estáticos, ya que acceden a información asociada a la clase y no a una instancia concreta. De este modo, se mantiene la encapsulación y se controla el acceso a estos datos globales.

## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

public static Punto crearPuntoRedondeado(double x, double y) {
    double xRedondeado = Math.round(x);
    double yRedondeado = Math.round(y);
    return new Punto(xRedondeado, yRedondeado);
}
Sí, se ha utilizado `static`, ya que un método factoría es un método de clase. Su función es crear objetos sin depender de una instancia previa, por lo que debe poder invocarse directamente sobre la clase `Punto`.

## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

Si se desea cambiar la representación interna de la clase `Punto` sin modificar su **interfaz pública**, puede sustituirse los dos atributos `double` por un único array interno de dos posiciones. Desde el exterior, la clase seguirá utilizándose igual, ya que el constructor y los métodos públicos mantienen la misma firma. Este es precisamente uno de los beneficios de la encapsulación: permitir cambios internos sin afectar al código que usa la clase.
La modificación afecta a la implementación interna. En lugar de almacenar `x` e `y` como variables independientes, se almacenan en un array privado. Los métodos públicos acceden a las posiciones correspondientes del array para realizar los cálculos necesarios. El resto del programa no necesita conocer este detalle.

public class Punto {

    private double[] coords = new double[2];

    public Punto(double x, double y) {
        coords[0] = x;
        coords[1] = y;
    }

    public double calcularDistanciaAOrigen() {
        return Math.sqrt(coords[0] * coords[0] + coords[1] * coords[1]);
    }

    public double calcularDistanciaAPunto(Punto otro) {
        double dx = this.coords[0] - otro.coords[0];
        double dy = this.coords[1] - otro.coords[1];
        return Math.sqrt(dx * dx + dy * dy);
    }
}
Desde el punto de vista de quien utiliza la clase, no ha cambiado nada. La interfaz pública permanece intacta, mientras que la estructura interna puede evolucionar libremente. Esto demuestra cómo la ocultación de información protege al resto del sistema frente a cambios en la implementación.

## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

Aunque un atributo vaya a disponer de métodos getter y setter públicos, no es recomendable declararlo directamente como `public`. Si el atributo fuese público, cualquier parte del programa podría acceder a él y modificarlo sin ningún tipo de control. En cambio, al declararlo `private` y ofrecer métodos de acceso, se mantiene la posibilidad de introducir validaciones o lógica adicional sin alterar la interfaz pública.

La convención más habitual en Java y en la mayoría de los diseños orientados a objetos es declarar los atributos como `private`. De este modo, el estado interno queda protegido y la clase conserva el control sobre cómo se accede o se modifica. Incluso cuando un getter y un setter parecen limitarse a devolver o asignar un valor, su existencia permite cambiar la implementación interna en el futuro sin afectar al código externo.

Esta práctica está directamente relacionada con las invariantes de clase. Si los atributos fueran públicos, cualquier parte del programa podría asignar valores que violen las condiciones que definen un estado válido. En cambio, mediante setters controlados, la clase puede comprobar que los nuevos valores respetan dichas invariantes antes de aceptarlos. Por tanto, mantener los atributos privados es fundamental para preservar la coherencia interna y la robustez del diseño.

## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

Una clase es inmutable cuando los objetos que se crean a partir de ella no pueden cambiar su estado una vez construidos. La clase no ofrece métodos públicos que alteren esos valores. Si se necesita un “cambio”, se crea un nuevo objeto con los valores deseados en lugar de modificar el existente.

Un método modificador es cualquier método que altera el estado interno de un objeto. Su finalidad es cambiar uno o varios atributos de la instancia. Aunque muchos métodos modificadores adoptan la forma de un setter (por ejemplo, `setX(...)`), no todos los métodos que modifican el estado son setters. Un método que incremente un contador, que desplace un punto o que cambie un estado interno también es modificador aunque no siga la convención de nombre típica.
Por tanto, un método modificador no es necesariamente un setter; el setter es simplemente un caso particular de método que asigna directamente un valor a un atributo. En cambio, un método modificador puede realizar cálculos o aplicar reglas antes de actualizar el estado.
Las clases inmutables presentan ventajas importantes. Facilitan el razonamiento sobre el programa, ya que el estado de un objeto no cambia inesperadamente, y ayudan a mantener automáticamente las invariantes de clase. Además, reducen efectos secundarios y errores derivados de modificaciones no controladas, lo que mejora la robustez y el mantenimiento del software.

## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

No es recomendable incluir métodos *setter* siempre ni como una convención automática. Añadir un *setter* implica permitir que el estado interno del objeto pueda modificarse desde el exterior, lo que amplía la interfaz pública de la clase. Cada punto adicional de modificación aumenta la complejidad del diseño y el riesgo de usos incorrectos.
Desde el punto de vista del modelado, solo deberían existir setters cuando tenga sentido que ese atributo cambie libremente después de la creación del objeto. En muchos casos, es preferible proporcionar métodos con un significado más específico que representen acciones del dominio, en lugar de permitir modificaciones genéricas de atributos. Esto hace que el código sea más expresivo y coherente con el problema que se está modelando.
La inclusión indiscriminada de setters puede comprometer las invariantes de clase. Cada modificación del estado debe garantizar que el objeto siga siendo válido, y cuantos más métodos permitan cambios, mayor será el esfuerzo necesario para mantener la coherencia interna. Por tanto, la práctica habitual en un diseño orientado a objetos cuidadoso es no añadir setters por defecto, sino únicamente cuando estén justificados por el comportamiento esperado de la clase.

## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

La clase `String` en Java es inmutable. Esto significa que, una vez creado un objeto de tipo `String`, su contenido no puede modificarse. Cualquier operación que parezca cambiar la cadena, como convertirla a mayúsculas o añadir texto, en realidad genera un nuevo objeto con el resultado, manteniendo intacta la cadena original.
Al concatenar dos cadenas mediante el operador `+`, se crea un nuevo objeto `String` que contiene el resultado de la concatenación. Las cadenas originales no se alteran. Este comportamiento es coherente con la inmutabilidad, pero implica que cada concatenación supone la creación de un nuevo objeto y la copia de los caracteres implicados.
Si se necesita construir una cadena muy larga mediante múltiples concatenaciones sucesivas, no es recomendable utilizar directamente `String`. En estos casos se debe emplear una clase mutable como `StringBuilder`, que permite modificar el contenido interno sin crear un nuevo objeto en cada operación. Esto mejora considerablemente la eficiencia en tiempo y memoria cuando se realizan muchas concatenaciones.

## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

En Programación Orientada a Objetos, los objetos pueden compararse de dos formas: por identidad o por contenido. Comparar por identidad significa comprobar si dos referencias apuntan exactamente al mismo objeto en memoria. Comparar por contenido significa verificar si dos objetos distintos representan la misma información según los criterios del dominio. Ambas comparaciones son conceptualmente diferentes y deben elegirse según lo que se quiera comprobar.
En Java, el operador `==` aplicado a objetos compara identidad, no contenido. Por su parte, el método `equals` está definido en la clase base `Object`, de la que heredan todas las clases. Por defecto, la implementación de `equals` en `Object` se comporta igual que `==`, es decir, comprueba si ambas referencias apuntan al mismo objeto. Para comparar por contenido, es necesario sobrescribir (`override`) el método `equals` en la clase correspondiente.
Las cadenas en Java, deben compararse utilizando el método `equals`, no el operador `==`. El método `equals` en la clase `String` está redefinido para comparar el contenido carácter por carácter. Usar `==` puede dar resultados inesperados, ya que solo comprueba si ambas referencias señalan al mismo objeto, no si contienen el mismo texto.

## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

Las clases wrapper son clases que encapsulan valores de tipos primitivos para poder tratarlos como objetos dentro de un modelo orientado a objetos. En Java, por ejemplo, cada tipo primitivo (`int`, `double`, `boolean`, etc.) tiene una clase asociada (`Integer`, `Double`, `Boolean`, etc.). Estas clases permiten que valores básicos puedan utilizarse en contextos donde se requieren objetos, como en colecciones.
El proceso de convertir un valor primitivo en su clase wrapper se denomina boxing, y el proceso inverso se llama unboxing. En versiones modernas de Java, este proceso es automático: el compilador realiza la conversión implícitamente cuando es necesario. Sin embargo, conceptualmente implica la creación de un objeto que contiene el valor primitivo.
Las ventajas de las clases wrapper incluyen la posibilidad de almacenar valores primitivos en estructuras que solo admiten objetos, representar la ausencia de valor mediante `null`, y disponer de métodos útiles asociados al tipo (por ejemplo, conversión de texto a número). Además, permiten integrar los tipos básicos dentro del modelo orientado a objetos de forma coherente.
No todos los lenguajes orientados a objetos distinguen entre tipos primitivos y objetos. En algunos lenguajes, todos los valores son objetos, por lo que no se necesitan clases wrapper. En cambio, en lenguajes como Java, donde existen tipos primitivos por razones de eficiencia, las clases wrapper son necesarias para unificar el tratamiento de datos dentro del sistema de tipos.

## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

En Programación Orientada a Objetos, un tipo de dato enumerado es un tipo que define un conjunto finito y cerrado de valores posibles. Se utiliza cuando una variable solo puede tomar uno entre varios valores predefinidos, lo que evita el uso de números o cadenas sin significado explícito. Desde el punto de vista del diseño, un enumerado mejora la claridad del código y restringe los valores válidos desde el propio sistema de tipos.
En Java, un tipo enumerado (`enum`) es en realidad una clase especial. Aunque su sintaxis es distinta a la de una clase convencional, cada enumerado define una clase que hereda implícitamente de `java.lang.Enum`. Cada uno de sus valores es una instancia única de esa clase. Esto significa que los enumerados no son simples constantes, sino objetos con comportamiento y estado controlado.
En términos de encapsulación, los enumerados en Java ofrecen ventajas importantes. Al ser clases, pueden tener atributos privados, constructores (implícitamente privados) y métodos públicos. Esto permite asociar datos y comportamiento a cada valor del enumerado y centralizar la lógica relacionada dentro del propio tipo. Además, el conjunto de valores posibles está completamente controlado por la definición del `enum`, lo que impide la creación de valores no válidos y refuerza las invariantes del diseño.

## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

En Java, un tipo enumerado permite definir un conjunto fijo de instancias y asociarles atributos y comportamiento. En el caso de `Mes`, cada mes puede modelarse como una instancia única con información propia, como el número de días y su posición en el año. El constructor del enumerado es implícitamente `private`, lo que impide crear nuevos meses fuera de los definidos.
Los atributos se declaran como privados para mantener la encapsulación, y los métodos públicos permiten consultar la información asociada a cada mes. Además, los métodos relacionados con las estaciones tienen en cuenta el hemisferio, ya que las estaciones se invierten entre el norte y el sur.

public enum Mes {

    ENERO(1, 31),
    FEBRERO(2, 28),
    MARZO(3, 31),
    ABRIL(4, 30),
    MAYO(5, 31),
    JUNIO(6, 30),
    JULIO(7, 31),
    AGOSTO(8, 31),
    SEPTIEMBRE(9, 30),
    OCTUBRE(10, 31),
    NOVIEMBRE(11, 30),
    DICIEMBRE(12, 31);

    private int ordinalAnio;
    private int dias;

    private Mes(int ordinalAnio, int dias) {
        this.ordinalAnio = ordinalAnio;
        this.dias = dias;
    }

    public int getDias() {
        return dias;
    }

    public int getOrdinalAnio() {
        return ordinalAnio;
    }

    public boolean esDePrimavera(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == MARZO || this == ABRIL || this == MAYO)
                : (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE);
    }

    public boolean esDeVerano(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == JUNIO || this == JULIO || this == AGOSTO)
                : (this == DICIEMBRE || this == ENERO || this == FEBRERO);
    }

    public boolean esDeOtoño(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE)
                : (this == MARZO || this == ABRIL || this == MAYO);
    }

    public boolean esDeInvierno(boolean enHemisferioNorte) {
        return enHemisferioNorte
                ? (this == DICIEMBRE || this == ENERO || this == FEBRERO)
                : (this == JUNIO || this == JULIO || this == AGOSTO);
    }
}
Este enumerado no solo define los doce meses, sino que encapsula la lógica asociada a ellos dentro del propio tipo. De este modo, se refuerza la claridad del diseño y se evita dispersar reglas relacionadas con los meses por distintas partes del programa.

## 24. Añade a la clase `Mes` del ejercicio anterior cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

Para añadir estos métodos al tipo enumerado `Mes`, basta con incorporar cuatro métodos públicos que determinen si el mes contiene **algún día** de una estación concreta. Como las estaciones se invierten entre hemisferio norte y sur, el parámetro `enHemisferioNorte` permite ajustar la lógica según corresponda.
Se considera que un mes “tiene algunos días” de una estación si al menos parte del mes pertenece a ella (por ejemplo, marzo contiene días de invierno y de primavera en el hemisferio norte). La implementación puede expresarse directamente mediante comparaciones con las constantes del enumerado.

public boolean esDePrimavera(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
    } else {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
    }
}

public boolean esDeVerano(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
    } else {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
    }
}

public boolean esDeOtoño(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == SEPTIEMBRE || this == OCTUBRE || this == NOVIEMBRE || this == DICIEMBRE;
    } else {
        return this == MARZO || this == ABRIL || this == MAYO || this == JUNIO;
    }
}

public boolean esDeInvierno(boolean enHemisferioNorte) {
    if (enHemisferioNorte) {
        return this == DICIEMBRE || this == ENERO || this == FEBRERO || this == MARZO;
    } else {
        return this == JUNIO || this == JULIO || this == AGOSTO || this == SEPTIEMBRE;
    }
}

De este modo, la lógica relacionada con las estaciones queda completamente encapsulada dentro del propio tipo `Mes`. El resto del programa no necesita conocer las reglas concretas de correspondencia entre meses y estaciones, ya que el enumerado ofrece métodos claros y expresivos para consultarlo.

