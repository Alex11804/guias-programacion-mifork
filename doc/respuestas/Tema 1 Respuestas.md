<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Clases y Objetos". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: ninguno.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->

# TEMA 1. Clases y objetos

## 1. ¿Cuáles son las cuatro características básicas de la programación orientada a objetos? Describe brevemente cada una

Las cuatro características básicas de la programación orientada a objetos son **encapsulación**, **abstracción**, **herencia** y **polimorfismo**. Estas características definen la forma en que se estructuran los programas y cómo interactúan sus componentes, diferenciando este paradigma de la programación procedimental tradicional usada en C.

La **encapsulación** consiste en agrupar datos y operaciones relacionadas dentro de una misma unidad llamada clase, controlando además el acceso a esos datos. La **abstracción** permite centrarse en qué hace un objeto y no en cómo lo hace internamente, mostrando solo la información necesaria al resto del programa y ocultando los detalles de implementación.

La **herencia** permite crear nuevas clases a partir de otras ya existentes, reutilizando código y estableciendo relaciones jerárquicas entre clases. Por último, el **polimorfismo** posibilita que diferentes objetos respondan de manera distinta ante el mismo mensaje u operación, lo que aporta flexibilidad y facilita la extensión del software sin modificar código ya existente.

## 2. Cita cuatro lenguajes populares que permitan la programación orientada a objetos

Existen numerosos lenguajes de programación que permiten aplicar el paradigma de la programación orientada a objetos, algunos de los cuales son ampliamente utilizados tanto en el ámbito académico como profesional. Estos lenguajes proporcionan mecanismos nativos para definir clases, crear objetos y aplicar conceptos como herencia o polimorfismo.

**Java** es uno de los lenguajes orientados a objetos más conocidos y utilizados, especialmente en aplicaciones empresariales, educativas y de desarrollo multiplataforma. **C++** también permite la programación orientada a objetos, aunque combina este paradigma con características procedimentales propias de C, lo que da lugar a un estilo híbrido.

Otros lenguajes populares son **Python**, que soporta orientación a objetos de forma flexible y sencilla, y **C#**, muy utilizado en el entorno de desarrollo de aplicaciones de escritorio, web y videojuegos dentro del ecosistema de Microsoft. Todos ellos permiten estructurar programas basados en clases y objetos, aunque con diferencias en sintaxis y filosofía de diseño.

## 3. Los paradigmas anteriores a la POO, ¿Qué es la **programación estructurada**? y, todavía mejor, ¿Qué es la **programación modular**?

La **programación estructurada** es un paradigma que propone organizar los programas utilizando estructuras de control bien definidas, como secuencia, selección y repetición, evitando el uso indiscriminado de saltos incondicionales como goto. Su objetivo principal es mejorar la claridad, legibilidad y mantenimiento del código, dividiendo los programas en bloques lógicos con un único punto de entrada y salida.

En este paradigma, el programa se concibe como un conjunto de instrucciones que se ejecutan de forma ordenada, apoyándose en funciones o procedimientos para reutilizar código. En lenguajes como C, esta forma de programar se basa en funciones que operan sobre datos, los cuales suelen estar separados de dichas funciones y pueden ser globales o pasados como parámetros.

La **programación modular** va un paso más allá al proponer la división del programa en módulos independientes, cada uno encargado de una responsabilidad concreta. Un módulo agrupa funciones y datos relacionados, definiendo claramente qué partes son visibles desde el exterior y cuáles permanecen ocultas, lo que facilita el trabajo en equipo y la reutilización de código.

A diferencia de la programación estructurada básica, la modular introduce una separación más clara entre las distintas partes del sistema, reduciendo dependencias y mejorando el mantenimiento. Sin embargo, los datos y las funciones siguen siendo entidades separadas, lo que marca una diferencia clave respecto a la programación orientada a objetos, donde ambos se integran en una misma estructura.

## 4. ¿Qué tres elementos definen a un objeto en programación orientada a objetos?

En programación orientada a objetos, un objeto se define a partir de **tres elementos fundamentales** que determinan su comportamiento y su papel dentro del programa. Estos elementos permiten modelar entidades del mundo real o conceptos abstractos de forma estructurada y coherente.

El primer elemento es el **estado**, que representa la información que almacena el objeto en un momento dado. Este estado se define mediante variables internas, llamadas atributos, que contienen los datos propios del objeto y pueden variar a lo largo de su ciclo de vida.

El segundo elemento es el **comportamiento**, que describe las acciones que el objeto puede realizar o las operaciones que se pueden aplicar sobre él. Este comportamiento se implementa mediante métodos o funciones asociadas al objeto. El tercer elemento es la **identidad**, que permite distinguir un objeto de otro incluso aunque tengan el mismo estado y comportamiento, ya que cada objeto es una instancia única dentro del sistema.

## 5. ¿Qué es una clase? ¿Es lo mismo que un objeto? ¿Qué es una instancia? ¿Todos los lenguajes orientados a objetos manejan el concepto de clase?

Una **clase** es una definición o plantilla que describe cómo serán los objetos de un determinado tipo. En ella se especifican los atributos que tendrán (estado) y los métodos que podrán ejecutar (comportamiento), pero la clase en sí no representa un elemento concreto del programa, sino un modelo a partir del cual se crean objetos.

Un **objeto** no es lo mismo que una clase. El objeto es una entidad concreta creada a partir de una clase, con valores específicos para sus atributos y existencia real durante la ejecución del programa. En este contexto, una **instancia** es precisamente cada objeto concreto que se crea a partir de una clase; es decir, “instanciar” una clase significa crear un objeto basado en esa definición.

No todos los lenguajes orientados a objetos manejan el concepto de clase de la misma forma. La mayoría, como Java o C++, se basan directamente en clases, pero existen lenguajes orientados a objetos que utilizan otros enfoques, como los basados en prototipos, donde los objetos se crean a partir de otros objetos sin necesidad de una clase formal. Esto demuestra que la orientación a objetos es un paradigma más amplio que el simple uso de clases.

## 6. ¿Dónde se almacenan en memoria los objetos? ¿Es igual en todos los lenguajes? ¿Qué es la **recolección de basura**? 

En programación orientada a objetos, los **objetos se almacenan en memoria dinámica**, también conocida como *heap*. Esto significa que su espacio de memoria se reserva durante la ejecución del programa y no en tiempo de compilación. A diferencia de las variables locales simples, los objetos suelen persistir más allá del bloque donde se crean, siempre que exista alguna referencia a ellos.

No obstante, **no es igual en todos los lenguajes** cómo se gestiona esa memoria. En lenguajes como C++, el programador puede crear objetos tanto en memoria dinámica como automática y es responsable de liberar manualmente la memoria cuando deja de usarse. En cambio, en lenguajes como Java, los objetos se crean siempre en memoria dinámica y el programador no libera la memoria de forma explícita.

La **recolección de basura** (*garbage collection*) es un mecanismo automático de gestión de memoria presente en algunos lenguajes orientados a objetos, como Java. Su función es detectar objetos que ya no son accesibles desde el programa y liberar la memoria que ocupan, evitando fugas de memoria. Este proceso se realiza de forma automática por el entorno de ejecución, lo que simplifica el desarrollo y reduce errores relacionados con la gestión manual de memoria.

## 7. ¿Qué es un método? ¿Qué es la **sobrecarga de métodos**? 

Un **método** es una función que forma parte de una clase y que define una acción que pueden realizar los objetos creados a partir de ella. A diferencia de las funciones en C, un método está asociado a un objeto o a una clase concreta y puede acceder directamente a los datos internos de ese objeto, es decir, a sus atributos.

Los métodos permiten definir el **comportamiento** de los objetos y son el medio principal mediante el cual estos interactúan entre sí. Al invocar un método, se está enviando un mensaje a un objeto para que ejecute una determinada operación usando su propio estado interno.

La **sobrecarga de métodos** consiste en definir varios métodos con el mismo nombre dentro de una misma clase, pero con distintas listas de parámetros. De esta forma, el compilador puede decidir qué método ejecutar según el número y el tipo de argumentos utilizados en la llamada. Esto mejora la legibilidad del código y permite usar nombres coherentes para operaciones conceptualmente similares, aunque realicen tareas ligeramente distintas.

## 8. Ejemplo mínimo de clase en Java, que se llame Punto, con dos atributos, x e y, con un método que se llame `calculaDistanciaAOrigen`, que calcule la distancia a la posición 0,0. Por sencillez, los atributos deben tener visibilidad por defecto. Crea además un ejemplo de uso con una instancia y uso del método

A continuación se muestra un **ejemplo mínimo de clase en Java** que cumple las condiciones indicadas. La clase se llama `Punto`, tiene dos atributos `x` e `y` con **visibilidad por defecto** (sin modificador) y un método `calculaDistanciaAOrigen` que devuelve la distancia desde el punto hasta el origen `(0,0)` usando la fórmula matemática habitual.
class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }
}

En este ejemplo, la clase define únicamente la estructura y el comportamiento del punto, pero no crea ningún objeto por sí misma. Los atributos `x` e `y` representan el estado del objeto, mientras que el método define una operación que utiliza ese estado. La función `Math.sqrt` pertenece a la biblioteca estándar de Java y calcula la raíz cuadrada.

El siguiente fragmento muestra un **ejemplo de uso**, donde se crea una instancia de la clase `Punto`, se asignan valores a sus atributos y se invoca el método definido anteriormente:

class PruebaPunto {
    public static void main(String[] args) {
        Punto p = new Punto();
        p.x = 3;
        p.y = 4;

        double distancia = p.calculaDistanciaAOrigen();
        System.out.println(distancia);
    }
}

Aquí se observa claramente la diferencia entre clase y objeto: `Punto` es la definición, mientras que `p` es una instancia concreta creada en memoria. El método se ejecuta sobre ese objeto específico y utiliza sus valores internos, lo que ilustra el funcionamiento básico de clases, objetos y métodos en Java.

## 9. ¿Cuál es el punto de entrada en un programa en Java? ¿Qué es `static` y para qué vale? ¿Sólo se emplea para ese método `main`? ¿Para qué se combina con `final`?

El **punto de entrada** de un programa en Java es el método `main`, cuya cabecera tiene una forma concreta y reconocida por la máquina virtual. Este método es el primero que se ejecuta cuando se inicia un programa y actúa como punto de arranque de toda la aplicación. Sin un método `main` correctamente definido, un programa Java no puede ejecutarse de forma independiente.

La palabra clave **`static`** indica que un método o atributo pertenece a la clase y no a los objetos creados a partir de ella. En el caso de `main`, se utiliza `static` porque este método debe poder ejecutarse sin necesidad de crear previamente ningún objeto. La máquina virtual de Java llama directamente al método `main` asociado a la clase, sin instanciarla.

El modificador `static` **no se emplea únicamente en el método `main`**. También se usa para definir atributos compartidos por todas las instancias de una clase o métodos que realizan operaciones generales que no dependen del estado de un objeto concreto. Por ejemplo, funciones auxiliares o constantes suelen declararse como estáticas.

La combinación de **`static` y `final`** se utiliza habitualmente para definir valores constantes asociados a una clase. `static` permite que el valor exista una sola vez y sea compartido, mientras que `final` impide que dicho valor sea modificado una vez inicializado. Esta combinación es común para definir constantes globales, como valores matemáticos o configuraciones fijas, mejorando la claridad y seguridad del código.

## 10. Intenta ejecutar un poco de Java de forma básica, con los comandos `javac` y `java`. ¿Cómo podemos compilar el programa y ejecutarlo desde linea de comandos? ¿Java es compilado? ¿Qué es la **máquina virtual**? ¿Qué es el *byte-code* y los ficheros `.class`?

Para ejecutar Java de forma básica desde la línea de comandos se utilizan dos herramientas principales: `javac` y `java`. El comando `javac` se emplea para **compilar** un archivo con extensión `.java`, generando como resultado uno o varios archivos `.class`. Posteriormente, el comando `java` se utiliza para **ejecutar** el programa, indicando el nombre de la clase que contiene el método `main`, sin incluir la extensión `.class`.

Java **sí es un lenguaje compilado**, pero no de la misma forma que C o C++. El código fuente no se traduce directamente a código máquina del sistema operativo, sino a un formato intermedio. Este paso permite que el mismo programa Java pueda ejecutarse en distintos sistemas sin necesidad de recompilarlo específicamente para cada uno.

La **máquina virtual de Java** (JVM) es un programa que actúa como intermediario entre el sistema operativo y el código Java. Su función es interpretar y ejecutar el código intermedio generado en la compilación, gestionando además aspectos como la memoria, la seguridad y la recolección de basura. Cada sistema operativo tiene su propia implementación de la JVM, pero todas entienden el mismo formato de código.

El **byte-code** es ese código intermedio generado por el compilador `javac` y almacenado en los ficheros `.class`. No es código fuente ni código máquina, sino un conjunto de instrucciones diseñadas para ser ejecutadas por la máquina virtual. Gracias a este enfoque, Java cumple el principio de “escribe una vez, ejecuta en cualquier lugar”, ya que el byte-code puede ejecutarse en cualquier plataforma que disponga de una JVM compatible.

## 11. En el código anterior de la clase `Punto` ¿Qué es `new`? ¿Qué es un **constructor**? Pon un ejemplo de constructor en una clase `Empleado` que tenga DNI, nombre y apellidos

En el código de la clase `Punto`, la palabra clave **`new`** se utiliza para **crear un objeto en memoria** a partir de una clase. Al emplear `new`, se reserva espacio en memoria dinámica para el objeto y se obtiene una referencia a ese objeto. Sin `new`, no se crea ningún objeto, únicamente se puede declarar una variable que todavía no apunta a ninguna instancia válida.

Un **constructor** es un método especial de una clase que se ejecuta automáticamente en el momento en que se crea un objeto con `new`. Su función principal es **inicializar el estado del objeto**, es decir, asignar valores iniciales a sus atributos. Un constructor tiene el mismo nombre que la clase y no devuelve ningún valor, ni siquiera `void`.

Si no se define ningún constructor, Java proporciona uno por defecto que no recibe parámetros y no realiza ninguna inicialización explícita. Sin embargo, es habitual definir constructores propios para asegurarse de que los objetos se crean siempre en un estado válido y coherente desde el primer momento.

A continuación se muestra un ejemplo de una clase `Empleado` con un constructor que inicializa los atributos `dni`, `nombre` y `apellidos`:

class Empleado {
    String dni;
    String nombre;
    String apellidos;

    Empleado(String dni, String nombre, String apellidos) {
        this.dni = dni;
        this.nombre = nombre;
        this.apellidos = apellidos;
    }
}

En este ejemplo, el constructor recibe los datos necesarios para crear un empleado y los asigna a los atributos del objeto recién creado. La palabra clave `this` se utiliza para distinguir entre los atributos de la clase y los parámetros del constructor con el mismo nombre.

## 12. ¿Qué es la referencia `this`? ¿Se llama igual en todos los lenguajes? Pon un ejemplo del uso de `this` en la clase `Punto`

La referencia **`this`** es una variable implícita que existe dentro de los métodos de una clase y que apunta al **objeto actual**, es decir, a la instancia concreta sobre la que se está ejecutando el método. Su uso permite acceder de forma explícita a los atributos y métodos del propio objeto, especialmente cuando existe ambigüedad entre variables locales o parámetros y los atributos de la clase.

Uno de los usos más habituales de `this` se da cuando los parámetros de un método o constructor tienen el mismo nombre que los atributos del objeto. En ese caso, `this` permite diferenciar claramente que se está accediendo al atributo del objeto y no a la variable local. También puede utilizarse para pasar el propio objeto como argumento a otro método o para encadenar constructores dentro de una misma clase.

La referencia no se llama igual en todos los lenguajes orientados a objetos. En Java, C++ y C# se utiliza el nombre `this`, mientras que en otros lenguajes puede recibir nombres distintos (por ejemplo, `self` en Python). El concepto, no obstante, es el mismo: una referencia al objeto actual.

El siguiente ejemplo muestra el uso de `this` en la clase `Punto`, empleándolo para acceder de forma explícita a los atributos del objeto dentro del método:

class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(this.x * this.x + this.y * this.y);
    }
}

En este caso, el uso de `this` no es estrictamente obligatorio, pero deja claro que `x` e `y` pertenecen al objeto actual. Esto mejora la claridad del código y resulta especialmente útil en clases más complejas o cuando existen nombres coincidentes.

## 13. Añade ahora otro nuevo método que se llame `distanciaA`, que reciba un `Punto` como parámetro y calcule la distancia entre `this` y el punto proporcionado

Para ampliar la clase `Punto`, se puede añadir un método que permita calcular la distancia entre **dos objetos de tipo `Punto`**. Este método se llamará `distanciaA` y recibirá como parámetro otro punto, representando la segunda posición con la que se desea calcular la distancia. De este modo, el método opera comparando el objeto actual (`this`) con el objeto proporcionado.

Este enfoque muestra una idea clave de la programación orientada a objetos: **los objetos colaboran entre sí**. El método pertenece a un objeto concreto y utiliza tanto su propio estado como el estado de otro objeto recibido como argumento. El uso de `this` permite dejar claro qué coordenadas corresponden al objeto actual y cuáles al otro punto.

El cálculo se realiza aplicando la fórmula de la distancia entre dos puntos en el plano cartesiano. El método devuelve un valor numérico que representa dicha distancia, sin modificar el estado de ninguno de los objetos implicados.

A continuación se muestra la clase `Punto` con el nuevo método añadido:

class Punto {
    double x;
    double y;

    double calculaDistanciaAOrigen() {
        return Math.sqrt(x * x + y * y);
    }

    double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

En este ejemplo, `this` hace referencia al punto desde el que se invoca el método, mientras que `otro` representa el punto pasado como argumento. Esto permite expresar de forma natural operaciones entre objetos del mismo tipo, una práctica habitual en diseño orientado a objetos.

## 14. El paso del `Punto` como parámetro a un método, es **por copia** o **por referencia**, es decir, si se cambia el valor de algún atributo del punto pasado como parámetro, dichos cambios afectan al objeto fuera del método? ¿Qué ocurre si en vez de un `Punto`, se recibiese un entero (`int`) y dicho entero se modificase dentro de la función? 

En Java, **el paso de parámetros es siempre por valor**, pero es importante distinguir qué valor se está copiando. Cuando se pasa un objeto como un `Punto`, lo que se copia es **la referencia al objeto**, no el objeto completo. Esto implica que dentro del método se puede acceder al mismo objeto que existe fuera de él y, por tanto, **si se modifica un atributo del objeto recibido, el cambio será visible fuera del método**.

Sin embargo, aunque la referencia apunte al mismo objeto, la **referencia en sí es una copia**. Esto significa que si dentro del método se asigna la referencia a otro objeto distinto, ese cambio no afecta a la referencia original fuera del método. Solo las modificaciones realizadas sobre el estado interno del objeto compartido tienen efecto externo.

En el caso de tipos primitivos como `int`, la situación es distinta. Al pasar un entero como parámetro, se copia directamente su valor numérico. Si dicho valor se modifica dentro del método, **el cambio no afecta en absoluto a la variable original** fuera de la función, ya que se trabaja con una copia independiente del valor.

Esta diferencia explica por qué los objetos pueden parecer pasados “por referencia” en Java, cuando en realidad el lenguaje mantiene una semántica uniforme de paso por valor. Lo que varía es si el valor copiado es un dato simple o una referencia a un objeto en memoria.

## 15. ¿Qué es el método `toString()` en Java? ¿Existe en otros lenguajes? Pon un ejemplo de `toString()` en la clase `Punto` en Java

El método **`toString()`** en Java es un método definido en la clase base de la que heredan todas las clases del lenguaje, y su propósito es **proporcionar una representación en forma de texto de un objeto**. Este método se utiliza de manera automática en muchas situaciones, como al imprimir un objeto por pantalla o al concatenarlo con una cadena de texto.

Por defecto, la implementación de `toString()` no suele ser especialmente útil, ya que devuelve información interna como el nombre de la clase y un identificador del objeto. Por este motivo, es habitual **sobrescribir** este método para que devuelva una descripción más clara y significativa del estado del objeto, facilitando la depuración y la comprensión del programa.

La idea de disponer de un método que convierta un objeto en texto existe también en otros lenguajes orientados a objetos, aunque con nombres distintos. Por ejemplo, en C++ se puede lograr un comportamiento similar mediante sobrecarga de operadores, y en Python existe el método `__str__`. El concepto es común, aunque la forma concreta de implementarlo varía según el lenguaje.

El siguiente ejemplo muestra una implementación sencilla de `toString()` en la clase `Punto`:

class Punto {
    double x;
    double y;

    @Override
    public String toString() {
        return "Punto(" + x + ", " + y + ")";
    }
}

En este caso, al imprimir un objeto `Punto`, se obtendrá una salida legible que muestra sus coordenadas. La anotación `@Override` indica que se está redefiniendo un método heredado, lo que ayuda a evitar errores y mejora la claridad del código.

## 16. Reflexiona: ¿una clase es como un `struct` en C? ¿Qué le falta al `struct` para ser como una clase y las variables de ese tipo ser instancias?

Una clase **puede recordar parcialmente** a un `struct` en C, ya que ambos permiten **agrupar datos relacionados** bajo un mismo nombre. En ese sentido, una clase sin métodos y con atributos públicos se parece bastante a un `struct`, porque ambos actúan como contenedores de información. Sin embargo, esta similitud es solo superficial y se limita a la organización de datos.

Lo que le falta al `struct` para ser como una clase es, en primer lugar, la **integración directa del comportamiento con los datos**. En C, las funciones que operan sobre un `struct` están separadas de la definición de los datos, mientras que en una clase los métodos forman parte de la propia estructura. Esto permite que los datos y las operaciones estén fuertemente cohesionados y se acceda a ellos de forma controlada.

Además, un `struct` en C no dispone de mecanismos propios de la orientación a objetos como **encapsulación**, **constructores**, **herencia** o **polimorfismo**. Tampoco existe un concepto implícito equivalente a `this` ni un control de visibilidad que proteja los datos. Todo esto hace que las variables de tipo `struct` sean simples agrupaciones de datos, pero no **instancias de una abstracción con comportamiento**.

Por estas razones, aunque una clase y un `struct` puedan parecer similares en apariencia, una clase representa una entidad mucho más rica. Las variables creadas a partir de una clase no solo almacenan datos, sino que también poseen identidad, comportamiento propio y reglas de acceso, lo que las convierte en verdaderas instancias dentro del paradigma orientado a objetos.

## 17. Quitemos un poco de magia a todo esto: ¿Como se podría “emular”, con `struct` en C, la clase `Punto`, con su función para calcular la distancia al origen? ¿Qué ha pasado con `this`?

La clase `Punto` se puede **emular en C** utilizando un `struct` para representar los datos y una función separada que opere sobre dicho `struct`. En este enfoque, el `struct` agrupa los atributos `x` e `y`, mientras que la función recibe explícitamente el punto sobre el que debe actuar. Aunque no se trata de orientación a objetos real, el resultado funcional es similar.

En C, la función que calcula la distancia al origen no “pertenece” al `struct`, sino que es una función normal que recibe como parámetro una variable de ese tipo. Esta separación es característica de la programación estructurada: los datos y las operaciones están relacionados solo por convención, no por el propio lenguaje.

El siguiente ejemplo muestra cómo se podría hacer esta emulación en C:

#include <math.h>

struct Punto {
    double x;
    double y;
};

double calculaDistanciaAOrigen(struct Punto p) {
    return sqrt(p.x * p.x + p.y * p.y);
}

En este contexto, **`this` ha desaparecido** porque en C no existe una referencia implícita al “objeto actual”. El papel de `this` lo cumple el parámetro `p`, que se pasa explícitamente a la función. En Java, esa referencia se proporciona automáticamente dentro de los métodos; en C, debe pasarse manualmente, lo que hace más evidente pero también más propenso a errores el vínculo entre datos y funciones.

