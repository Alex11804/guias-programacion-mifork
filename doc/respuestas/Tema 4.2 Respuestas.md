<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Herencia". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones y Composición.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.2. Herencia

## 1. En orientación a objetos, ¿qué es la **herencia** y su relación con "A es-un B"?. Explica las dos implicaciones principales: (1) **compatibilidad de tipos** y (2) **herencia de estado y comportamiento**. Pon un ejemplo en Java muy sencillo, donde un `Soldado` tiene un `nombre` (privado) y un método `saludar()` que muestra su nombre. Hay dos subtipos: un `Artillero`, que es capaz de disparar cohetes y un `Zapador` que pone minas, ambos heredan el atributo nombre y la capacidad de saludar. Además, y de forma específica, el artillero tiene un número de cohetes y el zapador un número de minas, accesibles mediante "getters" específicos. Respecto a la compatibilidad de tipos, aprovechémosla: crea un array de `Soldado`, mete varios de distinto tipo (son todos compatibles con `Soldado`). Recórrela y que todos te saluden. 
La **herencia** en orientación a objetos es un mecanismo que permite definir una nueva clase a partir de otra ya existente, reutilizando su estructura y comportamiento. Se suele expresar mediante la relación “A es-un B”, lo que indica que la clase derivada (subclase) es un caso particular de la clase base (superclase). Por ejemplo, si se dice que un `Artillero` es-un `Soldado`, significa que todo artillero puede considerarse también un soldado, pero con características adicionales. Este enfoque permite organizar el diseño en jerarquías y evitar duplicación de código.

La primera implicación importante es la **compatibilidad de tipos**. Si una clase B hereda de A, entonces cualquier objeto de tipo B puede utilizarse donde se espera un objeto de tipo A. Esto es similar a cómo en C se puede tratar un tipo más específico como uno más general, pero en orientación a objetos esta relación está formalizada por el lenguaje. Gracias a esto, se pueden manejar colecciones homogéneas (por ejemplo, arrays) que contengan objetos de distintos subtipos, siempre que compartan una superclase común.

La segunda implicación es la **herencia de estado y comportamiento**. La subclase hereda los atributos (estado) y métodos (comportamiento) definidos en la superclase. Esto significa que no es necesario redefinir aquello que ya existe, sino que puede reutilizarse directamente. Además, la subclase puede añadir nuevos atributos y métodos propios, especializando su comportamiento. En el ejemplo, todos los soldados tienen nombre y pueden saludar, pero cada tipo concreto añade sus propias capacidades (cohetes o minas).

A continuación se muestra un ejemplo sencillo en Java que ilustra estos conceptos:

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[3];

        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Pedro", 2);

        for (Soldado s : ejercito) {
            s.saludar();  // todos pueden saludar
        }
    }
}
```

En este ejemplo se observa que el array es de tipo `Soldado`, pero contiene objetos de distintos subtipos (`Artillero` y `Zapador`). Esto es posible gracias a la compatibilidad de tipos. Al recorrer el array, todos los elementos pueden ejecutar el método `saludar()`, ya que ese comportamiento se hereda de la clase base.


## 2. Al crear los soldados concretos, ¿cuántos constructores se ejecutan y en qué orden? ¿Qué significa `super` dentro de un constructor? Si la clase base no tiene visible el constructor sin parámetros, ¿debo llamar a `super` siempre? 
Al crear un objeto de una subclase en Java, no se ejecuta un único constructor, sino una cadena de constructores que recorre la jerarquía de herencia desde la clase base hasta la clase más derivada. En el ejemplo, al crear un Artillero, primero se ejecuta el constructor de Soldado y después el de Artillero. Este orden es obligatorio: primero se inicializa la parte “general” del objeto (la superclase) y después la parte específica (la subclase). Esto asegura que el objeto se construye de forma coherente, de lo más general a lo más concreto.

La palabra clave super dentro de un constructor se utiliza para invocar explícitamente el constructor de la superclase. En el caso de Artillero, super(nombre) llama al constructor de Soldado que recibe el nombre. Esto es necesario porque la subclase no tiene acceso directo a los atributos privados de la superclase (como nombre), por lo que debe delegar su inicialización en el constructor de la clase base. Además, esta llamada a super debe ser la primera instrucción del constructor.

Si no se escribe explícitamente una llamada a super, el compilador inserta automáticamente una llamada al constructor sin parámetros de la superclase (super()). Sin embargo, esto solo funciona si dicho constructor existe y es accesible. Si la clase base no tiene un constructor sin parámetros visible, entonces es obligatorio llamar explícitamente a super(...) indicando qué constructor se desea usar. De lo contrario, el código no compilará.

Por tanto, cuando la clase base no dispone de un constructor por defecto (o este no es accesible), siempre se debe invocar manualmente a super con los parámetros adecuados. Esto es especialmente importante en diseños donde se obliga a inicializar ciertos atributos desde el principio, evitando que los objetos se creen en un estado incompleto o inconsistente.

## 3. Respecto a los objetos de subclases en memoria, los atributos privados de la superclase, ¿forman parte de una instancia de la subclase en memoria? En caso afirmativo ¿implica que se puedan usar desde el código de la subclase? Explícalo con el ejemplo de `Soldado` y alguna de sus subclases.
Sí, los atributos privados de la superclase **forman parte de la instancia completa del objeto en memoria**, incluso cuando ese objeto es de una subclase. Un objeto de tipo `Artillero` o `Zapador` contiene internamente tanto los atributos definidos en `Soldado` (como `nombre`) como los propios de la subclase (como `cohetes` o `minas`). Conceptualmente, es como si el objeto incluyera “la parte Soldado” más “la parte específica”. Esto encaja con la idea de que un `Artillero` es-un `Soldado`, por lo que debe incluir todo lo necesario para comportarse como tal.

Sin embargo, que esos atributos existan en memoria **no implica que sean accesibles directamente desde el código de la subclase**. Si un atributo está declarado como `private` en la superclase, solo puede ser accedido desde dentro de esa misma clase. Por tanto, aunque un objeto `Artillero` tenga un `nombre` almacenado internamente (porque lo hereda de `Soldado`), no puede acceder a él directamente mediante `this.nombre` dentro del código de `Artillero`.

Aplicado al ejemplo, la clase `Soldado` define el atributo `private String nombre`. Cuando se crea un `Artillero`, ese atributo forma parte del objeto, pero el código de `Artillero` no puede manipularlo directamente. La única forma correcta de interactuar con ese atributo desde la subclase es a través de métodos públicos o protegidos definidos en la superclase, como podría ser el método `saludar()` o un posible `getNombre()` si existiera.

Esto refleja claramente la diferencia entre **estructura en memoria** y **visibilidad en el código**. La herencia implica reutilización de estado y comportamiento a nivel de objeto, pero la encapsulación sigue controlando estrictamente quién puede acceder a cada dato. De este modo, se evita que las subclases rompan las invariantes o dependan de detalles internos de la superclase.


## 4. ¿Qué implica en términos de **extensibilidad** de código el hecho de que sean compatibles a nivel de tipos? Ilustra esto añadiendo un nuevo tipo de `Soldado` y demostrando que el código para pedir el saludo a todos los soldados no se modifica.
La **compatibilidad de tipos** en herencia implica que el código puede diseñarse para trabajar con la superclase (`Soldado`) sin necesidad de conocer todos los subtipos concretos que existirán en el futuro. Esto tiene una consecuencia directa en la **extensibilidad**: es posible añadir nuevas clases derivadas sin modificar el código existente que ya trabaja con la superclase. El programa queda “abierto a extensión, pero cerrado a modificación”, lo que reduce errores y facilita la evolución del sistema.

Desde el punto de vista práctico, esto significa que cualquier nueva clase que herede de `Soldado` podrá integrarse automáticamente en estructuras que manejen objetos de tipo `Soldado`. El código que recorre un array o una lista de soldados no necesita cambiar, porque todos los objetos, independientemente de su tipo concreto, cumplen con la interfaz común definida en la superclase (por ejemplo, el método `saludar()`).

Para ilustrarlo, se puede añadir un nuevo tipo de soldado, por ejemplo un `Medico`, que también es-un `Soldado`, pero con una capacidad distinta:
```java
class Medico extends Soldado {
    private int botiquines;

    public Medico(String nombre, int botiquines) {
        super(nombre);
        this.botiquines = botiquines;
    }

    public int getBotiquines() {
        return botiquines;
    }
}
```
Ahora se puede utilizar este nuevo tipo junto con los anteriores sin modificar el código que ya existía:

```java
public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];

        ejercito[0] = new Artillero("Juan", 5);
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Pedro", 2);
        ejercito[3] = new Medico("Ana", 4);

        for (Soldado s : ejercito) {
            s.saludar();  // no cambia
        }
    }
}
```
El bucle que recorre el array no ha sido modificado en absoluto. Esto es posible porque todos los elementos son tratados como `Soldado`, independientemente de su tipo real. Así, la extensibilidad se logra añadiendo nuevas clases sin tocar el código que ya funciona, lo que es una ventaja clave frente a enfoques más rígidos como los habituales en C sin orientación a objetos.

## 5. En Java, cuando trabajo con referencias y herencia. ¿Puedo tener una referencia del supertipo que apunte a objetos reales de un subtipo? ¿Puedo invocar con la referencia del supertipo a métodos públicos del subtipo? ¿En qué consiste el **"upcasting"** y el **"downcasting"**? ¿Qué es el `instanceof`? Pon un ejemplo de recorrido de un array de `Soldado`, comprobando que, si el objeto real es un `Artillero`, solicite el número de cohetes que tiene y los imprima.
Sí, en Java se puede tener una referencia del **supertipo** que apunte a un objeto real de un **subtipo**. Por ejemplo, una variable de tipo `Soldado` puede apuntar a un objeto `Artillero` o `Zapador`. Esto es precisamente una consecuencia de la compatibilidad de tipos en herencia: si `Artillero` es-un `Soldado`, entonces un objeto `Artillero` puede tratarse como un `Soldado`. A esto se le llama **upcasting**, y en Java suele hacerse de forma implícita, sin necesidad de escribir nada especial.

Con una referencia del supertipo solo se pueden invocar directamente los métodos que estén declarados en ese supertipo. Aunque el objeto real sea un `Artillero`, si la referencia es de tipo `Soldado`, el compilador solo permite usar aquello que “sabe” que todo `Soldado` tiene. Por tanto, no se podría llamar directamente a `getCohetes()` usando una referencia `Soldado`, porque ese método no existe en todos los soldados, solo en `Artillero`. Para acceder a comportamiento específico del subtipo hay que hacer un **downcasting**, es decir, convertir la referencia general a una más específica.

El **downcasting** consiste en tratar una referencia del supertipo como una referencia del subtipo real, por ejemplo convertir un `Soldado` en `Artillero`. Esta conversión no es automática, porque no todo `Soldado` es un `Artillero`. Si se hace mal, se produce un error en ejecución (`ClassCastException`). Para evitarlo, se suele usar `instanceof`, que permite comprobar si el objeto real pertenece a un determinado subtipo antes de convertirlo. En otras palabras, `instanceof` responde a preguntas del estilo: “¿este `Soldado` es realmente un `Artillero`?”.

A continuación se muestra un ejemplo donde se recorre un array de `Soldado` y, si el objeto real es un `Artillero`, se obtiene e imprime su número de cohetes:

```java
class Soldado {
    private String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Artillero extends Soldado {
    private int cohetes;

    public Artillero(String nombre, int cohetes) {
        super(nombre);
        this.cohetes = cohetes;
    }

    public int getCohetes() {
        return cohetes;
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public int getMinas() {
        return minas;
    }
}

public class Main {
    public static void main(String[] args) {
        Soldado[] ejercito = new Soldado[4];
        ejercito[0] = new Artillero("Juan", 5);   // upcasting implícito
        ejercito[1] = new Zapador("Luis", 3);
        ejercito[2] = new Artillero("Pedro", 2);
        ejercito[3] = new Zapador("Andrés", 6);

        for (Soldado s : ejercito) {
            s.saludar();

            if (s instanceof Artillero) {
                Artillero a = (Artillero) s;   // downcasting
                System.out.println("Tiene " + a.getCohetes() + " cohetes");
            }
        }
    }
}
```
Aquí se ve la idea completa: el array almacena referencias de tipo `Soldado`, pero algunos objetos reales son `Artillero`. Mientras se recorre, `instanceof` permite comprobar el tipo real del objeto y, solo en ese caso, se hace el downcasting para acceder al método específico `getCohetes()`. Es el típico momento en que Java dice: “sí, es un soldado… pero algunos vienen con extras”.

## 6. Respecto a la ocultación de información y herencia, ¿qué significa acceso **"protegido"** de métodos y/o atributos? ¿Cómo se implementa en Java? Pon un ejemplo de uso de en la clase `Soldado` para que su nombre sea protegido y pueda usarse en el método de poner bombas del `Zapador`.
El acceso **protegido** (`protected`) es un nivel de visibilidad intermedio entre `private` y `public`. Un miembro protegido no es accesible desde cualquier clase, pero tampoco está completamente oculto: puede ser utilizado por las **subclases** (aunque estén en otros paquetes) y también por otras clases del mismo paquete. En términos de diseño, permite que las clases derivadas reutilicen directamente ciertos atributos o métodos de la superclase sin exponerlos completamente al exterior.

En Java, este acceso se implementa mediante la palabra clave `protected`. A diferencia de `private`, que solo permite acceso dentro de la propia clase, `protected` abre ese acceso a las subclases. Esto es útil cuando se quiere permitir que las clases derivadas trabajen con ciertos datos internos, pero sin hacerlos públicos para todo el programa. Es una forma controlada de reutilización dentro de una jerarquía de herencia.

Aplicado al ejemplo, si el atributo `nombre` de `Soldado` se declara como `protected`, entonces una subclase como `Zapador` puede acceder directamente a él. Esto permite usar ese atributo dentro de métodos propios del subtipo, como uno que simule la acción de poner bombas, sin necesidad de un getter público:

```java
class Soldado {
    protected String nombre;

    public Soldado(String nombre) {
        this.nombre = nombre;
    }

    public void saludar() {
        System.out.println("Hola, soy " + nombre);
    }
}

class Zapador extends Soldado {
    private int minas;

    public Zapador(String nombre, int minas) {
        super(nombre);
        this.minas = minas;
    }

    public void ponerMinas() {
        System.out.println(nombre + " está colocando minas (" + minas + ")");
    }
}
```
En este ejemplo, `Zapador` accede directamente al atributo `nombre` heredado, algo que no sería posible si fuese `private`. Esto muestra cómo `protected` facilita la reutilización en subclases, manteniendo al mismo tiempo cierto nivel de encapsulación, ya que el atributo sigue sin ser accesible desde clases externas no relacionadas.

## 7. En los lenguajes orientados a objetos ¿hay una **clase base** para todos los objetos? ¿Ocurre en todos los lenguajes? ¿Qué ocurre en Java?
En muchos lenguajes orientados a objetos existe el concepto de una **clase base común** para todos los objetos, aunque no es obligatorio en todos ellos. La idea es disponer de un tipo raíz del que hereden todas las clases, lo que permite tratar cualquier objeto de forma uniforme. Sin embargo, no todos los lenguajes implementan esto de la misma manera: en algunos (como C++) no es obligatorio que todas las clases hereden de una única clase base, mientras que en otros (como Java) este comportamiento está integrado en el lenguaje.

En Java, **todas las clases heredan implícitamente de la clase `Object`**, incluso si no se indica explícitamente con `extends`. Esto significa que cualquier objeto en Java es, en última instancia, un `Object`. Esta clase define comportamiento común que todos los objetos tienen, como `toString()`, `equals()` o `hashCode()`. Por ejemplo, aunque se defina `class Soldado { ... }`, internamente es como si se hubiera escrito `class Soldado extends Object`.

Esto tiene implicaciones importantes: permite manejar colecciones genéricas de objetos (`Object[]`), pasar cualquier objeto como parámetro de tipo general, o definir métodos que trabajen con cualquier tipo. Además, garantiza que todos los objetos comparten una interfaz mínima común, lo que facilita la reutilización y la interoperabilidad dentro del lenguaje.

En otros lenguajes orientados a objetos la situación varía. En C++, por ejemplo, no existe una clase base universal obligatoria; cada jerarquía de clases puede tener su propia raíz. En lenguajes como Python, sí existe un tipo base común (`object`), similar al caso de Java. Por tanto, aunque el concepto de clase base universal es frecuente, no es una característica universal en todos los lenguajes orientados a objetos.


## 8. ¿Qué es la **"herencia múltiple"**? ¿Existe en Java herencia múltiple?
La **herencia múltiple** es un mecanismo por el cual una clase puede heredar directamente de **más de una clase base**. Esto permite combinar estado y comportamiento de varias jerarquías en una sola clase. Por ejemplo, conceptualmente se podría tener una clase que herede de `Soldado` y también de otra clase distinta que aporte otra funcionalidad. El problema es que este modelo puede generar conflictos, especialmente cuando dos superclases definen métodos con el mismo nombre o atributos similares, lo que introduce ambigüedad sobre cuál debe utilizarse.

En Java **no existe herencia múltiple de clases**. Una clase solo puede extender (`extends`) de una única clase base. Esta decisión de diseño evita muchos de los problemas de ambigüedad presentes en otros lenguajes como C++, donde sí se permite herencia múltiple de clases. En Java se opta por un modelo más simple y seguro, donde la jerarquía de clases es siempre un árbol y no un grafo más complejo.

Sin embargo, Java ofrece una alternativa mediante las **interfaces**. Una clase puede implementar (`implements`) múltiples interfaces, lo que permite heredar comportamiento (especialmente desde Java 8 con métodos `default`) sin heredar estado. De esta forma se consigue cierta flexibilidad similar a la herencia múltiple, pero evitando los problemas clásicos asociados a compartir múltiples implementaciones completas.

Por tanto, en Java no se puede heredar de varias clases a la vez, pero sí combinar múltiples fuentes de comportamiento a través de interfaces. Esto mantiene el modelo más controlado y reduce conflictos, a costa de limitar la reutilización directa de estado desde múltiples clases base.

## 9. Las excepciones en los lenguajes orientados a objetos son objetos. Por tanto, se pueden crear excepciones personalizadas. Pon un ejemplo en Java de una excepción personalizada (`UsuarioNoEncontradoException`), que sea *no controlada* y que además este compuesto con un `Usuario`, para saber qué `Usuario` dio el problema. Permite además que se pueda incluir la causa, es decir, sobrecarga el constructor para tener una versión que permita añadir la causa subyacente. 

En orientación a objetos, las excepciones son clases, por lo que pueden definirse tipos propios adaptados al dominio del problema. Si se desea que una excepción sea **no controlada**, debe heredar de `RuntimeException`. Esto implica que no es obligatorio declararla con `throws` ni capturarla con `try-catch`, lo que suele reservarse para errores de programación o situaciones que no deberían ocurrir en condiciones normales.

Además, al ser objetos, las excepciones pueden **componerse con otros objetos**, incorporando información adicional sobre el error. En este caso, la excepción puede contener un objeto `Usuario` que represente el usuario que provocó el problema. Esto permite que, cuando la excepción sea capturada, se disponga de contexto suficiente para diagnosticar o tratar el error de forma más precisa.

A continuación se muestra un ejemplo de una excepción personalizada `UsuarioNoEncontradoException`, que hereda de `RuntimeException`, contiene un `Usuario` y ofrece dos constructores: uno básico y otro que permite incluir una causa subyacente:
```java
class Usuario {
    private String nombre;

    public Usuario(String nombre) {
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }
}

class UsuarioNoEncontradoException extends RuntimeException {
    private Usuario usuario;

    public UsuarioNoEncontradoException(Usuario usuario) {
        super("Usuario no encontrado: " + usuario.getNombre());
        this.usuario = usuario;
    }

    public UsuarioNoEncontradoException(Usuario usuario, Throwable causa) {
        super("Usuario no encontrado: " + usuario.getNombre(), causa);
        this.usuario = usuario;
    }

    public Usuario getUsuario() {
        return usuario;
    }
}
```

En este diseño, la excepción no solo indica que ha ocurrido un error, sino que encapsula información relevante sobre el `Usuario` implicado y, opcionalmente, la causa original del problema. Esto sigue el mismo patrón de composición visto en otros contextos, pero aplicado a excepciones, permitiendo enriquecer la información disponible durante el manejo de errores sin perder encapsulación.

## 10. Herencia vs. Composición. Se dice que no se debe emplear herencia simplemente por reutilizar código, es decir, que si quiero reutilizar código simplemente, no debo pensar en herencia como primera opción ¿por qué?
La herencia no debería utilizarse únicamente para reutilizar código porque introduce una **relación conceptual fuerte** entre clases: “A es-un B”. Esta relación no es solo técnica, sino también semántica. Si se usa herencia sin que exista realmente esa relación, el diseño se vuelve incorrecto y confuso. Por ejemplo, hacer que una clase herede de otra solo para aprovechar un método implica afirmar que una es un tipo de la otra, aunque no tenga sentido en el dominio del problema.

Además, la herencia genera un **acoplamiento rígido** entre la superclase y la subclase. La subclase depende de los detalles internos de la superclase, y cualquier cambio en esta puede afectar a todas sus derivadas. Esto dificulta el mantenimiento y la evolución del código. En cambio, la composición (tener un objeto dentro de otro) permite reutilizar comportamiento sin establecer una dependencia tan fuerte, ya que los componentes pueden cambiarse o modificarse con mayor facilidad.

Otro problema es que la herencia **expone demasiado** de la implementación interna. Una subclase hereda no solo lo que necesita, sino todo lo que la superclase define (estado y comportamiento), incluso aquello que no debería utilizar. Esto puede romper invariantes o permitir usos incorrectos. Con composición, en cambio, se controla mejor qué funcionalidad se expone, delegando solo lo necesario mediante métodos.

Por tanto, la herencia debe emplearse cuando existe una relación clara de especialización (“es-un”) y no solo como mecanismo de reutilización. Si el objetivo es reutilizar código sin implicar una jerarquía conceptual, la composición suele ser una opción más flexible, segura y mantenible.

## 11. Herencia vs. Composición. Se dice que se debe *"favorecer la composición frente a la herencia"*, ¿por qué?
Se recomienda **favorecer la composición frente a la herencia** porque la composición produce diseños más **flexibles y desacoplados**. En lugar de establecer una relación rígida de “es-un”, la composición modela relaciones del tipo “tiene-un”, donde una clase utiliza otra como componente interno. Esto permite cambiar fácilmente la implementación de ese componente sin afectar al resto del sistema, algo mucho más complicado cuando existe una jerarquía de herencia.

La herencia, por el contrario, introduce un **acoplamiento fuerte** entre la superclase y la subclase. La subclase depende de la implementación interna de la superclase, no solo de su interfaz pública. Si la superclase cambia (por ejemplo, modifica su comportamiento interno o sus invariantes), puede afectar a todas las subclases, incluso aunque estas no se modifiquen directamente. Esto hace que el código sea más frágil y difícil de mantener a largo plazo.

Con composición se consigue además una mayor **modularidad y reutilización controlada**. En lugar de heredar todo el comportamiento, una clase puede delegar solo lo que necesita a otros objetos. Esto permite combinar comportamientos de forma más dinámica y evita problemas típicos de la herencia, como la necesidad de forzar jerarquías artificiales o la imposibilidad de heredar de varias clases (limitación en Java).

Por tanto, la composición se considera una opción más segura y adaptable para reutilizar código y construir sistemas complejos. La herencia sigue siendo útil, pero debe reservarse para casos donde exista una relación clara de especialización (“es-un”). En cambio, cuando el objetivo es combinar funcionalidades o reutilizar comportamiento sin imponer una jerarquía rígida, la composición resulta generalmente una mejor elección.

## 12. Herencia vs. Composición. Se dice que la *"herencia rompe la encapsulación"*, ¿a qué se refiere esto?
Cuando se afirma que la **herencia rompe la encapsulación**, se hace referencia a que una subclase puede llegar a depender de detalles internos de la superclase que, idealmente, deberían permanecer ocultos. La encapsulación busca que una clase exponga solo su interfaz pública y oculte su implementación interna. Sin embargo, al heredar, la subclase no solo utiliza la interfaz pública, sino que también queda ligada al comportamiento interno heredado, lo que debilita ese aislamiento.

En la práctica, la subclase puede verse afectada por cambios en la implementación de la superclase, aunque su interfaz pública no haya cambiado. Por ejemplo, si `Soldado` modifica cómo gestiona internamente el nombre o el comportamiento de `saludar()`, una subclase como `Zapador` podría comportarse de forma distinta sin haber cambiado su propio código. Esto implica que la subclase depende no solo de lo que la superclase promete (interfaz), sino también de cómo lo implementa.

Además, si la superclase expone miembros como `protected`, las subclases pueden acceder directamente a esos atributos o métodos, lo que rompe aún más la encapsulación. En lugar de interactuar mediante métodos controlados, la subclase puede manipular directamente el estado interno, aumentando el riesgo de violar invariantes o introducir errores difíciles de detectar.

Por tanto, decir que la herencia rompe la encapsulación significa que reduce la separación entre interfaz e implementación. La subclase queda más acoplada a los detalles internos de la superclase, lo que dificulta el mantenimiento y la evolución del código. Este es uno de los motivos principales por los que se recomienda usar la herencia con cuidado y preferir la composición cuando no exista una relación clara de especialización.

## 13. Pongamos un ejemplo de dos alternativas para lo mismo. Tenemos un `Estudiante` y un `Trabajador`, ambos tienen datos en común: el DNI y el nombre. Modelemos esto de dos formas: uno por herencia, con una superclase `Persona`, y otro con composición, con una clase `DatosPersonales`. Se debe recibir una instancia de `DatosPersonales` en el constructor de la clase `Estudiante` y `Trabajador`.
Una primera forma de modelar el problema es mediante **herencia**, extrayendo los datos comunes (`dni` y `nombre`) a una superclase `Persona`. Tanto `Estudiante` como `Trabajador` se definen como subclases que heredan esos atributos y pueden añadir comportamiento propio. Este enfoque implica una relación “es-un”: un `Estudiante` es una `Persona` y un `Trabajador` también lo es. Se reutiliza directamente el estado y, potencialmente, métodos comunes definidos en la superclase.

```java
class Persona {
    protected String dni;
    protected String nombre;

    public Persona(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }
}

class Estudiante extends Persona {
    public Estudiante(String dni, String nombre) {
        super(dni, nombre);
    }
}

class Trabajador extends Persona {
    public Trabajador(String dni, String nombre) {
        super(dni, nombre);
    }
}
```
La segunda forma es mediante **composición**, donde los datos comunes se encapsulan en una clase independiente `DatosPersonales`. En este caso, `Estudiante` y `Trabajador` no heredan de una clase base, sino que contienen un objeto `DatosPersonales`. La relación aquí es “tiene-un”: un estudiante tiene datos personales, en lugar de ser una persona en la jerarquía de clases.

```java
class DatosPersonales {
    private String dni;
    private String nombre;

    public DatosPersonales(String dni, String nombre) {
        this.dni = dni;
        this.nombre = nombre;
    }

    public String getDni() {
        return dni;
    }

    public String getNombre() {
        return nombre;
    }
}

class Estudiante {
    private DatosPersonales datos;

    public Estudiante(DatosPersonales datos) {
        this.datos = datos;
    }
}

class Trabajador {
    private DatosPersonales datos;

    public Trabajador(DatosPersonales datos) {
        this.datos = datos;
    }
}
```
Ambos enfoques resuelven el mismo problema, pero con implicaciones distintas. La herencia reutiliza directamente estructura y comportamiento dentro de una jerarquía, mientras que la composición separa responsabilidades y permite reutilizar los datos sin establecer una relación rígida entre las clases. En el segundo caso, además, se cumple explícitamente el requisito de recibir una instancia de `DatosPersonales` en el constructor, lo que refuerza la idea de composición.

