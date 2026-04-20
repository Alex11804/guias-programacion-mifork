<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Polimorfismo". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación, Excepciones, Composición y Herencia.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 5. Polimorfismo

## 1. Brevemente, ¿qué es el **"polimorfismo"** y para qué sirve en programación orientada a objetos? ¿qué es la **"sobreescritura"** de métodos?

El **polimorfismo** se define como la capacidad de un objeto de tomar múltiples formas. En el contexto de Java, esto permite que una única variable de referencia de un tipo superior (superclase o interfaz) pueda apuntar a objetos de diferentes subclases. La utilidad principal de este concepto radica en la flexibilidad y la extensibilidad del código, ya que permite escribir algoritmos genéricos que operan sobre la superclase sin necesidad de conocer los detalles específicos de las clases derivadas hasta el momento de la ejecución.

Al emplear polimorfismo, se consigue reducir drásticamente el acoplamiento del software. En lugar de programar una lógica distinta para cada tipo de dato —lo que en C requeriría múltiples estructuras `if` o `switch` basadas en un enumerado de tipo—, se programa para una interfaz común. Esto facilita la incorporación de nuevas funcionalidades o clases al sistema sin tener que modificar el código existente, cumpliendo así con el principio de diseño de "abierto para la extensión, cerrado para la modificación".

Por otro lado, la **sobrescritura** de métodos (o *method overriding*) es el mecanismo técnico que hace posible el polimorfismo dinámico. Consiste en redefinir en una subclase un método que ya existe en su superclase, manteniendo exactamente la misma firma (nombre, parámetros y tipo de retorno). Mediante el uso de la anotación `@Override`, se indica al compilador la intención de proporcionar una implementación específica que reemplace a la del padre cuando el objeto sea de la clase hija.

Este proceso está estrechamente ligado al concepto de **enlace dinámico**. Cuando se invoca un método sobre una referencia, la Máquina Virtual de Java (JVM) no decide qué código ejecutar basándose en el tipo de la variable (el contenedor), sino en el tipo real del objeto que reside en la memoria (el contenido). De este modo, la sobrescritura permite que cada subclase responda a un mismo mensaje de la manera más adecuada según su propia naturaleza.

## 2. ¿En qué consiste la **"ligadura dinámica"** o **"enlace tardío"**? ¿qué relación tiene con el polimorfismo? ¿hay que indicarlos explícitamente al programar o depende esto del lenguaje? Compara C++ y Java. Indicalo después también para Python.

La **ligadura dinámica** o enlace tardío es el mecanismo por el cual la asociación entre la llamada a un método y su implementación real se pospone hasta el tiempo de ejecución. En lugar de que el compilador decida qué dirección de memoria ejecutar basándose en el tipo de la variable (como ocurre en la ligadura estática de C), la decisión se toma durante la marcha del programa consultando el tipo real del objeto. Este proceso permite que un programa sea flexible y pueda manejar objetos de clases desconocidas en el momento de la compilación, siempre que respeten la jerarquía de herencia.

La relación con el **polimorfismo** es de absoluta dependencia: la ligadura dinámica es el motor técnico que permite que el polimorfismo ocurra. Sin ella, una variable de la superclase siempre ejecutaría los métodos de la clase base, ignorando las especializaciones de las subclases. Gracias al enlace tardío, cuando se invoca un método sobre una referencia genérica, el sistema "busca" en la tabla de métodos del objeto concreto la versión más específica disponible, permitiendo que el comportamiento polimórfico se manifieste de forma automática.

En cuanto a su indicación explícita, la implementación varía significativamente según el lenguaje. En **Java**, la ligadura dinámica es el comportamiento por defecto para todos los métodos no estáticos, no finales y no privados; el programador no necesita realizar ninguna acción especial más allá de la sobrescritura. En **C++**, por el contrario, el programador debe optar explícitamente por este comportamiento declarando los métodos como `virtual` en la clase base. Si en C++ se omite esta palabra clave, el lenguaje aplica ligadura estática por defecto para ganar eficiencia, lo que impediría el comportamiento polimórfico esperado en una jerarquía.

En el caso de **Python**, el concepto se lleva al extremo debido a su naturaleza dinámica. Al ser un lenguaje de tipado dinámico, no es necesario heredar de una clase base común para lograr un comportamiento similar (lo que se conoce como *Duck Typing*). En Python, todas las llamadas a métodos se resuelven en tiempo de ejecución mediante la inspección del objeto, por lo que la ligadura dinámica es intrínseca y omnipresente sin necesidad de palabras clave como `virtual` ni declaraciones de tipo previas.

## 3. Pon un ejemplo sencillo en Java, de un `Soldado`, con un método `saluda`, con dos subclases: `Zapador` y `Artillero`, donde `Zapador` sobreescribe el método `saludar`, sustituyendo por completo su comportamiento. Ilustra el funcionamiento del polimorfismo creando un array de `Soldados` de dos tipos y luego recorriéndolo empleando referencias de tipo `Soldado` y llamando a `saludar`.

Para ilustrar el concepto de polimorfismo, se define en primer lugar una clase base denominada `Soldado`. Esta clase contiene un método `saludar` que proporciona un comportamiento genérico. Al trabajar con Java, no es necesario marcar el método como "virtual" como ocurriría en C++, ya que por defecto todos los métodos de instancia permiten la ligadura dinámica.

Posteriormente, se crean las subclases `Zapador` y `Artillero`. En el caso del `Zapador`, se realiza una sobrescritura completa del método utilizando la anotación `@Override`, modificando el mensaje que se muestra por pantalla. La clase `Artillero`, al no sobrescribir el método, heredará el comportamiento estándar de la clase padre. Esto permite observar cómo una misma llamada se resuelve de forma distinta según el tipo real del objeto.

El siguiente código muestra la implementación de las clases y un método principal que gestiona un array de tipo `Soldado`. Al recorrer el array con un bucle, se emplea una referencia de la superclase para invocar el método `saludar`. Es en este punto donde actúa el polimorfismo: aunque la variable sea de tipo `Soldado`, el programa ejecutará la versión del método que corresponda al objeto concreto almacenado en cada posición de la memoria.

```java
// Clase base
class Soldado {
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }
}

// Subclase que sobrescribe el comportamiento
class Zapador extends Soldado {
    @Override
    public void saludar() {
        System.out.println("Zapador listo para colocar explosivos.");
    }
}

// Subclase que hereda el comportamiento base
class Artillero extends Soldado {
    // No sobrescribe saludar(), usa el de Soldado
}

public class Principal {
    public static void main(String[] args) {
        // Creación de un array de la superclase conteniendo diferentes tipos
        Soldado[] peloton = new Soldado[2];
        peloton[0] = new Zapador();
        peloton[1] = new Artillero();

        // Recorrido polimórfico
        for (Soldado s : peloton) {
            // El enlace dinámico decide qué método ejecutar en tiempo de ejecución
            s.saludar();
        }
    }
}
```
Al ejecutar este programa, la salida mostrará mensajes diferentes para cada elemento del array, a pesar de que ambos son tratados bajo la referencia común `Soldado`. Este diseño facilita la gestión de grandes grupos de objetos diversos mediante una interfaz única, eliminando la necesidad de realizar comprobaciones manuales de tipo sobre cada elemento del peloton.


## 4. Si sobreescribo un método, ¿puedo invocar el método base para trabajar a partir de su resultado? Haz que zapador cambie ligeramente la forma de saludar, que salude de forma normal, tal cual hace el soldado base, pero que además añada un "ZAPADOR A SUS ORDENES" ¿qué palabra clave del lenguaje has usado para invocar al método de la clase base?

Sí, es totalmente posible invocar la implementación de la clase base desde un método que ha sido sobrescrito. Esta práctica es una técnica de diseño fundamental cuando se desea extender o refinar el comportamiento original en lugar de anularlo y sustituirlo por completo. Esto favorece enormemente la reutilización del código, un principio crítico para mantener sistemas complejos, evitando copiar y pegar la lógica que ya ha sido implementada y probada en la superclase.

Para lograr este objetivo en Java, se emplea la palabra clave **`super`**. Esta palabra reservada funciona como una referencia explícita a la clase padre inmediata del objeto actual en el contexto de la herencia. Al escribir `super.nombreDelMetodo()`, se fuerza al entorno de ejecución a ignorar temporalmente la versión sobrescrita local y buscar la implementación definida un nivel más arriba en la jerarquía de clases. Es el equivalente conceptual a usar `this` para referenciar a la propia instancia, pero dirigido hacia la estructura heredada.

A continuación, se detalla cómo se modificaría la clase `Zapador` para cumplir con este nuevo requisito, integrando el saludo base con su mensaje específico:

```java
// Subclase que amplía el comportamiento usando 'super'
class Zapador extends Soldado {
    @Override
    public void saludar() {
        // Se invoca primero el comportamiento de la superclase
        super.saludar(); 
        
        // Se añade el comportamiento específico de la subclase
        System.out.println("ZAPADOR A SUS ORDENES");
    }
}
```
En este escenario, al procesar polimórficamente un objeto de tipo `Zapador`, el flujo de ejecución ingresará a su método sobrescrito. La instrucción `super.saludar()` delegará el control temporalmente a la clase `Soldado`, la cual imprimirá "Soldado presentándose.". Inmediatamente después, el control retornará a la subclase para ejecutar la siguiente línea, imprimiendo el texto adicional "ZAPADOR A SUS ORDENES". Esto demuestra cómo las clases hijas pueden aprovechar y construir sobre el trabajo de sus progenitoras.

## 5. Al sobreescribir un método en Java, ¿qué restricciones existen sobre los tipos de los parámetros y el tipo de retorno? ¿Qué diferencia hay entre sobreescritura (*overriding*) y sobrecarga (*overloading*)? ¿Para qué sirve la anotación `@Override` y por qué es recomendable usarla siempre?

Al sobrescribir un método en Java, existen reglas estrictas para garantizar que el contrato de la clase base se mantenga intacto y el polimorfismo funcione correctamente. La restricción fundamental es que la firma del método (su nombre y el orden y tipo exacto de sus parámetros) debe ser idéntica; cualquier variación en los parámetros invalida la sobrescritura. En cuanto al tipo de retorno, Java permite la **covarianza**, lo que significa que el método en la subclase puede devolver el mismo tipo que el original o, alternativamente, un tipo derivado más específico (por ejemplo, si el método padre devuelve un `Soldado`, la subclase puede devolver un `Zapador`). Adicionalmente, la visibilidad del método no puede reducirse (no se puede pasar de `public` a `protected`, por ejemplo) y no se pueden declarar nuevas excepciones comprobadas que sean más amplias que las originales.

Es crucial no confundir la **sobrescritura** (*overriding*) con la **sobrecarga** (*overloading*). La sobrescritura requiere una jerarquía de herencia, mantiene la firma exacta y sirve para redefinir el comportamiento de un método mediante **enlace dinámico** durante la ejecución del programa. Por el contrario, la sobrecarga ocurre cuando se definen múltiples métodos con el mismo nombre dentro de la misma clase (o heredados), pero variando obligatoriamente su lista de parámetros (en cantidad o tipo). La sobrecarga se resuelve en tiempo de compilación mediante **enlace estático**, siendo un concepto idéntico a la sobrecarga de funciones existente en C++, cuyo objetivo es ofrecer diferentes formas de inicializar o procesar datos bajo un mismo identificador semántico.

Finalmente, la anotación `@Override` es una herramienta del lenguaje diseñada para prevenir errores humanos. Aunque su inclusión no es obligatoria para que el polimorfismo opere, su uso se considera una buena práctica universal. Al colocar esta anotación justo antes de la declaración de un método, se le indica explícitamente al compilador la intención de sobrescribir. Si se comete un error tipográfico en el nombre del método o una equivocación en los tipos de los parámetros, el compilador lanzará un error inmediato. Sin esta anotación, el compilador asumiría silenciosamente que se está creando un método nuevo o sobrecargando uno existente, provocando que la ejecución polimórfica falle sin previo aviso, lo cual resulta en errores lógicos complejos de rastrear.

## 6. Entonces, cuando se estudia Java, ¿se emplea el polimorfismo desde el principio? Por ejemplo, sobreescribiendo `toString` o sobreescribiendo `equals`, ¿ya estoy usando polimorfismo?

Sí, el polimorfismo se emplea desde las primeras etapas del aprendizaje en Java, a menudo de forma implícita. Esto se debe a la arquitectura fundamental del lenguaje, donde todas las clases heredan, directa o indirectamente, de una superclase universal llamada `java.lang.Object`. Esta jerarquía garantiza que cualquier instancia creada sea, por definición, un tipo de `Object`, permitiendo aplicar los conceptos de *upcasting* y enlace dinámico desde el primer momento en que se interactúa con clases personalizadas.

El caso de la sobrescritura de `toString()` es el ejemplo paradigmático de esta dinámica. Cuando se pasa una variable a funciones de la biblioteca estándar, como al utilizar `System.out.println()`, internamente ese método está diseñado para aceptar una referencia genérica de tipo `Object`. Sin embargo, gracias a la ligadura dinámica explicada anteriormente, la Máquina Virtual de Java no ejecuta el comportamiento genérico de la clase base, sino que invoca la versión de `toString()` que haya sido sobrescrita en la clase específica del objeto suministrado. Es polimorfismo en su estado más puro.

De manera análoga, la sobrescritura del método `equals(Object obj)` constituye otra aplicación cotidiana de este mecanismo. Las colecciones en Java, así como numerosos algoritmos de búsqueda, utilizan este método genérico para comparar elementos sin importar de qué clase sean. Al recibir un parámetro de la superclase `Object`, la implementación sobrescrita de `equals` en una subclase requiere aplicar un *downcasting* seguro (comprobando previamente con `instanceof`) para evaluar la igualdad de los atributos concretos. 

Por lo tanto, al redefinir estos métodos heredados de `Object`, se está participando activamente en el diseño polimórfico del lenguaje. Operaciones aparentemente simples, como imprimir el estado de un objeto por consola o comparar dos instancias para verificar si contienen la misma información, están cimentadas exactamente en la misma maquinaria de herencia y enlace tardío que rige arquitecturas de software mucho más complejas.


## 7. ¿Qué es una **"clase abstracta"**? ¿Qué es un **"método abstracto"**? ¿Puedo crear instancias de una clase abstracta? Pongamos un ejemplo en Java: Redefinamos `Soldado`, hagamos que, además del método `saluda` que ya tenía, tenga un método `atacar`, que sea abstracto y que cada tipo de soldado haga su acción cuando se le pida atacar. ¿Donde debemos poner `abstract`?

Una **clase abstracta** es una entidad fundamental en el diseño de jerarquías que no puede ser instanciada de manera directa; es decir, el compilador de Java prohibirá estrictamente la creación de objetos de este tipo mediante el uso del operador `new`. Su función exclusiva es actuar como un molde o clase base genérica a partir de la cual otras clases más específicas heredarán características (atributos y métodos comunes). Se utiliza cuando se identifica un concepto general que agrupa a varias entidades, pero el concepto en sí mismo es demasiado genérico para existir por sí solo como un objeto en memoria.

Por su parte, un **método abstracto** es una función que se declara en la clase base definiendo su firma (nombre, parámetros y tipo de retorno), pero que carece completamente de cuerpo o implementación (no tiene bloque de código entre llaves `{}`). Este tipo de método actúa como un contrato estricto: obliga a cualquier subclase a sobrescribirlo obligatoriamente y proporcionar la lógica real de ejecución. Es una regla estricta del lenguaje que, si una clase posee al menos un método abstracto, la clase al completo debe marcarse también como abstracta para evitar que se intente crear un objeto con un comportamiento incompleto.

En cuanto a la sintaxis en Java, la palabra clave `abstract` debe figurar como un modificador tanto en la declaración de la clase principal como en la firma de aquellos métodos que carezcan de implementación. Retomando la jerarquía militar, esto permite que el método `saludar` mantenga su comportamiento base programado, mientras que la responsabilidad de definir la acción de `atacar` queda delegada a la especialización de cada tropa.

```java
// La clase se marca como abstracta
abstract class Soldado {
    
    // Método concreto (con implementación)
    public void saludar() {
        System.out.println("Soldado presentándose.");
    }
    
    // Método abstracto (sin implementación, termina en punto y coma)
    public abstract void atacar();
}

class Zapador extends Soldado {
    // Es obligatorio sobrescribir el método abstracto
    @Override
    public void atacar() {
        System.out.println("Zapador: Colocando cargas explosivas en el objetivo.");
    }
}

class Artillero extends Soldado {
    // Es obligatorio sobrescribir el método abstracto
    @Override
    public void atacar() {
        System.out.println("Artillero: Disparando obús de 155mm.");
    }
}
```
Mediante este diseño estructural, se obtiene un control riguroso sobre el modelo de datos. Se garantiza que en el sistema nunca existirá un objeto instanciado que sea simplemente un "Soldado" ambiguo, sino que forzosamente será un especialista de tipo `Zapador` o `Artillero`. A pesar de esta restricción en la creación de objetos, se conserva intacta la capacidad de emplear el polimorfismo: se pueden definir arrays o listas de referencias del tipo genérico `Soldado` para agrupar diferentes tropas, invocando el método `atacar` sobre la referencia base con la certeza de que el enlace dinámico ejecutará el asalto correcto para cada unidad.

## 8. ¿Qué efecto tiene la palabra clave `final` sobre métodos y clases en Java? ¿Cómo se relaciona con el polimorfismo? ¿Conoces algún ejemplo de clase `final` en la propia API estándar de Java?

La palabra clave `final` en Java actúa como un mecanismo de restricción estricta sobre la herencia y la modificación de comportamientos. Cuando se aplica en la declaración de una **clase**, impide rotundamente que esta pueda ser extendida; es decir, el compilador prohibirá que cualquier otra clase intente heredar de ella utilizando `extends`. Por otro lado, si se aplica a un **método** específico dentro de una clase normal, el efecto es que ninguna subclase podrá sobrescribirlo. Desde la perspectiva de lenguajes como C o C++, se puede asimilar a un nivel de protección estructural que garantiza que la implementación original permanezca inalterada e inviolable.

La relación de `final` con el polimorfismo es de contención: sirve precisamente para **limitar o anular el comportamiento polimórfico** cuando el diseño del sistema así lo requiere. Dado que el polimorfismo dinámico depende intrínsecamente de la sobrescritura de métodos y del enlace tardío, marcar un método como `final` garantiza el uso de enlace estático temprano. Esto asegura que siempre se ejecutará esa versión exacta del código, eliminando cualquier ambigüedad en tiempo de ejecución. Esta técnica se emplea cuando un método realiza una operación crítica (por ejemplo, validar un estado interno) que bajo ninguna circunstancia debe ser alterada u omitida en clases derivadas.

En la API estándar de Java, el ejemplo más representativo e importante de una clase `final` es `java.lang.String`. Los diseñadores del lenguaje establecieron que la clase encargada de gestionar las cadenas de texto no debía ser extensible. Esta decisión de diseño garantiza la inmutabilidad de los textos en memoria y previene vulnerabilidades de seguridad graves; si se permitiera crear una subclase de `String`, un código malicioso o erróneo podría alterar su comportamiento interno (como la longitud o la lectura de caracteres), comprometiendo operaciones fundamentales del sistema. Otras clases, como la clase utilitaria `Math` o los envoltorios de tipos primitivos (`Integer`, `Double`), también están definidas como `final`.

## 9. En Java, qué son las **"interfaces"**? ¿Son como clases abstractas? ¿Una clase puede implementar más de una interfaz?

Una **interfaz** en Java es una estructura fundamental que define un contrato o un conjunto estricto de comportamientos que las clases deben cumplir. A diferencia de una clase tradicional, que contiene tanto el estado (variables) como el comportamiento (código de los métodos), una interfaz clásica se compone casi exclusivamente de firmas de métodos y constantes. Funciona como una plantilla puramente conceptual que especifica "qué" acciones debe poder realizar un objeto, delegando la responsabilidad del "cómo" (la implementación del bloque de código) a la clase concreta que decida adoptar dicha interfaz.

Aunque guardan grandes similitudes con las **clases abstractas** —ya que ninguna de las dos estructuras puede ser instanciada directamente en memoria mediante la instrucción `new`—, existen diferencias arquitectónicas críticas dictadas por el diseño del lenguaje. Una clase abstracta puede contener constructores, variables de instancia mutables y una combinación de métodos con y sin código, diseñándose para agrupar entidades bajo una jerarquía estricta ("es un tipo de"). Por el contrario, la interfaz tradicional carece por completo de estado interno y se enfoca en dotar de capacidades transversales ("es capaz de hacer"), lo que permite que objetos pertenecientes a ramas de herencia completamente distintas compartan un mismo protocolo de comunicación.

La característica más potente y distintiva de las interfaces en Java es que constituyen el mecanismo oficial para sortear la restricción de la herencia simple. Mientras que el lenguaje prohíbe categóricamente que una clase herede de más de una superclase (para evitar ambigüedades estructurales como el infame "problema del diamante" que sí permite C++), sí **permite que una misma clase implemente múltiples interfaces** simultáneamente. Al separar los nombres de las interfaces mediante comas, la clase adquiere múltiples roles y se compromete a proporcionar el código para todas las operaciones exigidas por cada uno de esos contratos.

```java
// Definición de interfaces como "roles" o "capacidades"
interface Conductor {
    void arrancarVehiculo();
}

interface TiradorSelecto {
    void apuntarConMira();
}

// La clase hereda de UNA sola superclase, pero implementa MÚLTIPLES interfaces
class OperadorEspecial extends Soldado implements Conductor, TiradorSelecto {
    
    // Método heredado de la superclase abstracta Soldado
    @Override
    public void atacar() {
        System.out.println("Operador Especial iniciando maniobra de ataque.");
    }

    // Método exigido por la interfaz Conductor
    @Override
    public void arrancarVehiculo() {
        System.out.println("Encendiendo el motor del vehículo táctico.");
    }

    // Método exigido por la interfaz TiradorSelecto
    @Override
    public void apuntarConMira() {
        System.out.println("Ajustando elevación y deriva en la mira telescópica.");
    }
}
```

## 10. Vamos a poner un ejemplo nuevo con polimorfismo. Queremos implementar una clase `Punto`, con un método `calcularDistanciaA`, que permite calcular la distancia a otro `Punto`. Sin embargo, como queremos trabajar con puntos 2D y 3D, haz que ese método sea abstracto y haya dos implementaciones de ese cálculo de distancia. Emplea `instanceof` y *downcasting* para verificar que se recibe un punto compatible y poder calcular correctamente la distancia siempre entre puntos del mismo subtipo. Aprovecha este diseño para crear ahora una clase `Linea`, que acepta `Punto`, sin saber de qué tipo es, y es capaz de dar su longitud independientemente de las dimensiones de sus puntos (las cuales desconoce).

El diseño propuesto ilustra una aplicación avanzada del polimorfismo combinada con el paso de parámetros de tipo genérico. Al definir un método abstracto `calcularDistanciaA(Punto otro)` en la superclase, se establece un contrato universal: cualquier punto en el sistema debe saber calcular la distancia hacia otro punto. Sin embargo, la fórmula matemática concreta varía dependiendo de si el espacio es bidimensional o tridimensional. Esta estructura delega a las subclases `Punto2D` y `Punto3D` la responsabilidad de proveer la lógica matemática específica, garantizando un comportamiento coherente en toda la jerarquía.

La implementación de dicho cálculo presenta un reto técnico: el método recibe una referencia genérica de tipo `Punto`, pero para aplicar el teorema de Pitágoras es imprescindible acceder a coordenadas concretas (como `z` en 3D) que la clase base desconoce. Para resolverlo, se utiliza el operador `instanceof` seguido de un *downcasting*. Primero, se verifica dinámicamente si el punto recibido por parámetro pertenece a la misma dimensión espacial que el objeto actual. Si la comprobación es exitosa, se realiza el moldeado seguro para acceder a sus atributos; en caso contrario, se lanza una excepción. Este proceso ofrece una seguridad en tiempo de ejecución que no existe por defecto en los *castings* tradicionales de C.

Finalmente, la clase `Linea` representa el mayor beneficio del diseño polimórfico. Mediante el uso de la composición, una línea se define almacenando dos referencias abstractas de tipo `Punto`. Cuando se requiere calcular la longitud de la línea, esta simplemente invoca el método `calcularDistanciaA` sobre uno de los puntos, pasando el otro como argumento. La clase `Linea` opera en absoluta ignorancia de la dimensionalidad de sus componentes; carece de estructuras condicionales (como `if` o `switch`) para discernir tipos. El enlace dinámico se encarga de ejecutar la matemática correcta, lo que significa que si en el futuro se desarrolla un sistema para un `Punto4D`, el código de la clase `Linea` permanecerá inalterado y totalmente funcional.

```java
// Superclase abstracta
abstract class Punto {
    // Recibe una referencia genérica de la propia superclase
    public abstract double calcularDistanciaA(Punto otro);
}

// Implementación en 2D
class Punto2D extends Punto {
    private double x, y;

    public Punto2D(double x, double y) {
        this.x = x; this.y = y;
    }

    public double getX() { return x; }
    public double getY() { return y; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        // Se verifica compatibilidad espacial
        if (otro instanceof Punto2D) {
            Punto2D p2 = (Punto2D) otro; // Downcasting seguro
            double dx = this.x - p2.getX();
            double dy = this.y - p2.getY();
            return Math.sqrt(dx * dx + dy * dy);
        } else {
            throw new IllegalArgumentException("No se puede calcular distancia entre dimensiones distintas.");
        }
    }
}

// Implementación en 3D
class Punto3D extends Punto {
    private double x, y, z;

    public Punto3D(double x, double y, double z) {
        this.x = x; this.y = y; this.z = z;
    }

    public double getX() { return x; }
    public double getY() { return y; }
    public double getZ() { return z; }

    @Override
    public double calcularDistanciaA(Punto otro) {
        // Se verifica compatibilidad espacial
        if (otro instanceof Punto3D) {
            Punto3D p2 = (Punto3D) otro; // Downcasting seguro
            double dx = this.x - p2.getX();
            double dy = this.y - p2.getY();
            double dz = this.z - p2.getZ();
            return Math.sqrt(dx * dx + dy * dy + dz * dz);
        } else {
            throw new IllegalArgumentException("No se puede calcular distancia entre dimensiones distintas.");
        }
    }
}

// Clase que utiliza el polimorfismo y la composición
class Linea {
    private Punto origen;
    private Punto destino;

    // Acepta cualquier combinación de puntos (la validación ocurre en el cálculo)
    public Linea(Punto origen, Punto destino) {
        this.origen = origen;
        this.destino = destino;
    }

    public double calcularLongitud() {
        // Se delega el cálculo: el enlace dinámico decide qué código de Punto se ejecuta
        return origen.calcularDistanciaA(destino);
    }
}
```

## 11. ¿Qué es la **"herencia de interfaces"** en Java? ¿Existe **"herencia múltiple de interfaces"**? Pon un ejemplo de una interfaz `Fichero` que tenga un método para leer su contenido en forma de `String` y luego dicha interfaz sea extendida por otra que sea `FicheroEscribible` que permita enviar contenido e incluso eliminar el fichero.

La **herencia de interfaces** en Java es el mecanismo mediante el cual una interfaz puede derivar de otra u otras, adquiriendo automáticamente todas las firmas de métodos y constantes definidas en sus predecesoras. Para lograrlo, se emplea la misma palabra reservada `extends` que se utiliza en la herencia de clases. Este enfoque permite crear jerarquías de contratos modulares y escalables, donde una interfaz base define las operaciones más fundamentales y las sub-interfaces añaden capacidades más avanzadas, ampliando el protocolo de uso sin atarlo a una implementación concreta.

En cuanto a la **herencia múltiple**, aunque Java la prohíbe terminantemente para las clases, **sí existe y está plenamente soportada entre interfaces**. Una interfaz puede extender múltiples interfaces padre simultáneamente, separando sus nombres mediante comas. Esta flexibilidad es posible porque, al heredar únicamente firmas de métodos vacías y no código ejecutable ni variables de estado, se evita por completo el clásico "problema del diamante" característico de C++. El compilador simplemente fusiona todos los requisitos individuales en un único y gran contrato vinculante.

A continuación, se materializa esta estructura con el caso de uso propuesto. Se define primero una interfaz de solo lectura. Seguidamente, la interfaz de escritura hereda de ella, conformando un contrato más exigente. Cualquier clase que decida implementar la sub-interfaz estará obligada por el compilador a proporcionar el bloque de código para las tres operaciones en conjunto.

```java
// Interfaz base: define un contrato básico de lectura
interface Fichero {
    String leer();
}

// Sub-interfaz: hereda leer() y añade sus propios métodos
interface FicheroEscribible extends Fichero {
    void escribir(String contenido);
    void eliminar();
}

// Una clase concreta que implementa la interfaz derivada
class FicheroTexto implements FicheroEscribible {
    private String datos = "";

    // Obligatorio: implementar el método heredado de la interfaz base
    @Override
    public String leer() {
        return this.datos;
    }

    // Obligatorio: implementar los métodos de la sub-interfaz
    @Override
    public void escribir(String contenido) {
        this.datos += contenido;
        System.out.println("Contenido escrito exitosamente.");
    }

    @Override
    public void eliminar() {
        this.datos = "";
        System.out.println("El fichero ha sido vaciado/eliminado.");
    }
}
```
