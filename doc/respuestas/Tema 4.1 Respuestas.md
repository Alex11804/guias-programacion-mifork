<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Composición". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación y Excepciones.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# Tema 4.1. Composición


## 1. En C, podemos crear estructuras mayores **componiendo** unas con otras, que suelen describirse como "A tiene-un/tiene-varios B". Pon un ejemplo, empleando `struct`, de una línea de puntos, donde puntos tienen dos coordenadas (`x` e `y`), y la línea esta hecha de dos puntos. Incluye una función para calcular la distancia entre puntos y otra para hallar la longitud de una línea.

En C, la composición se puede representar incluyendo una estructura dentro de otra. En este caso, una **línea está compuesta por dos puntos**, lo que encaja con la idea “A tiene-un B”. Cada punto contiene dos coordenadas (`x` e `y`), y la línea simplemente agrupa dos puntos como extremos. Este enfoque es natural en C, donde los `struct` permiten organizar datos relacionados, aunque sin asociar directamente funciones a esos datos (a diferencia de la POO) .

Para calcular la distancia entre dos puntos, se define una función que recibe dos estructuras `Punto` y aplica la fórmula de distancia euclídea. A partir de ahí, la longitud de una línea puede obtenerse reutilizando esa misma función, ya que una línea no es más que dos puntos. Esto muestra cómo, incluso en C, se puede construir funcionalidad reutilizable a partir de estructuras simples, siguiendo una idea similar a la composición.

```c
#include <stdio.h>
#include <math.h>

// Definición de un punto
struct Punto {
    double x;
    double y;
};

// Definición de una línea (compuesta por dos puntos)
struct Linea {
    struct Punto p1;
    struct Punto p2;
};

// Función para calcular la distancia entre dos puntos
double distancia(struct Punto a, struct Punto b) {
    double dx = a.x - b.x;
    double dy = a.y - b.y;
    return sqrt(dx * dx + dy * dy);
}

// Función para calcular la longitud de una línea
double longitud(struct Linea l) {
    return distancia(l.p1, l.p2);
}

// Ejemplo de uso
int main() {
    struct Punto p1 = {0, 0};
    struct Punto p2 = {3, 4};

    struct Linea linea = {p1, p2};

    printf("Distancia entre puntos: %.2f\n", distancia(p1, p2));
    printf("Longitud de la línea: %.2f\n", longitud(linea));

    return 0;
}
```

Este ejemplo refleja claramente la composición: la estructura `Linea` contiene dos `Punto`, y las funciones operan sobre estas estructuras pasando los datos explícitamente. A diferencia de Java, no existe un `this` implícito ni métodos asociados al `struct`, por lo que la relación entre datos y funciones debe gestionarse manualmente.


## 2. Ahora transforma ese ejemplo a orientación a objetos con Java, para tener un primer ejemplo de **composición** en orientación a objetos. Crea una clase `Punto`, y una clase `Linea`. La clase `Punto` debe tener un método para calcular distancia a otro `Punto` y `Linea` debe tener un método para calcular su longitud. Gracias a la ocultación de información, supera a C, garantizando que los puntos sean inmutables, al igual que la línea, que una vez creada, no queremos que se modifique de qué a qué puntos va dicha línea.  

En Java, la composición se expresa haciendo que una clase contenga objetos de otras clases como parte de su estado. En este caso, una `Linea` está compuesta por dos objetos `Punto`. A diferencia de C, donde los datos y las funciones están separados, en orientación a objetos cada clase agrupa tanto los datos como el comportamiento. Además, mediante encapsulación, se puede controlar el acceso a los atributos y garantizar ciertas propiedades del diseño, como la inmutabilidad .
Para que los objetos sean **inmutables**, se declaran sus atributos como `private final` y no se proporcionan métodos modificadores (setters). Esto implica que el estado del objeto queda fijado en el momento de su creación y no puede cambiar después. En `Punto`, esto asegura que sus coordenadas no se modifiquen, y en `Linea` garantiza que los puntos que la definen no cambien una vez creada. Este diseño protege las invariantes de la clase y evita efectos secundarios inesperados.

```java
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class Linea {
    private final Punto p1;
    private final Punto p2;

    public Linea(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```
En este diseño, la composición aparece claramente: `Linea` contiene dos objetos `Punto`. Gracias a la ocultación de información, no es posible modificar ni las coordenadas de los puntos ni los extremos de la línea después de su creación. Esto supone una mejora respecto a C, donde cualquier parte del programa podría alterar directamente los datos, mientras que aquí la propia clase controla completamente su estado y su comportamiento.


## 3. ¿Qué significa la **multiplicidad** en la composición? En el ejemplo anterior, ¿cuál es la multiplicidad entre `Linea` y `Punto`? Indícalo expresando la multiplicidad en ambas direcciones, de `Linea` a `Punto` y de `Punto` a `Linea`.

La **multiplicidad** en composición indica cuántas instancias de una clase pueden estar asociadas con una instancia de otra clase. Es un concepto que describe la cantidad de relaciones posibles entre objetos, y suele expresarse con rangos como `1`, `0..1`, `1..*`, `0..*`, etc. En otras palabras, responde a preguntas como: “¿cuántos objetos de tipo B puede tener un objeto de tipo A?” y también al revés. Esto permite entender mejor la estructura del modelo y las restricciones que existen entre las clases.

En el ejemplo de `Linea` y `Punto`, desde **`Linea` hacia `Punto`**, la multiplicidad es **2** (o también se puede expresar como `2..2`). Esto se debe a que cada línea está compuesta exactamente por dos puntos: uno inicial y uno final. No puede haber una línea con menos ni más puntos en este diseño, ya que su definición fija esos dos extremos.

En la dirección contraria, desde **`Punto` hacia `Linea`**, la multiplicidad es **0..***. Esto significa que un punto puede no pertenecer a ninguna línea, o puede formar parte de muchas líneas distintas al mismo tiempo. Por ejemplo, un mismo punto puede ser el extremo de varias líneas diferentes. Esta asimetría en la multiplicidad es habitual en composición, ya que la relación no tiene por qué ser equivalente en ambos sentidos.


## 4. ¿Qué significa composición **fuerte** y composición **débil**? ¿Qué consecuencia implica en relación al ciclo de vida de los objetos? Indica a cuál solemos referirnos como **"asociación o agregación"** y a cuál como **"composición"** propiamente.

La diferencia entre **composición fuerte** y **composición débil** está relacionada con el grado de dependencia entre los objetos y, sobre todo, con su **ciclo de vida**. En ambos casos existe una relación “tiene-un”, pero no todas las relaciones implican el mismo nivel de vínculo entre las partes. Este matiz es importante en diseño orientado a objetos, porque afecta a cómo se crean, usan y destruyen los objetos.

En la **composición fuerte**, los objetos contenidos dependen completamente del objeto que los contiene. Esto significa que su ciclo de vida está ligado: si el objeto principal se destruye, también lo hacen sus componentes. Además, esos componentes no suelen existir de forma independiente ni compartirse con otros objetos. Este tipo de relación es lo que se denomina **composición** propiamente dicha. Por ejemplo, en una `Linea`, los puntos que la definen pueden considerarse parte esencial de ella y no tendrían sentido sin esa línea concreta (según el diseño elegido).

En la **composición débil**, los objetos relacionados pueden existir de forma independiente. El objeto principal “usa” o “referencia” otros objetos, pero no es su dueño. Estos pueden ser compartidos por varios objetos y su ciclo de vida no depende del contenedor. A este tipo de relación se le suele llamar **asociación** o **agregación**. Por ejemplo, si varios objetos comparten un mismo `Punto`, ese punto puede seguir existiendo aunque una `Linea` desaparezca.

La consecuencia clave es, por tanto, el **control del ciclo de vida**: en composición fuerte hay propiedad y dependencia total, mientras que en agregación o asociación hay independencia. Esta distinción guía decisiones de diseño, como si un objeto debe crearse dentro de otro, si puede compartirse o si debe garantizarse que no exista fuera del contexto del objeto que lo contiene.



## 5. Cuando una clase usa a otra al recibirla o devolverla como parámetro en algún método, al hacer `new` dentro de un método, o al usarlas como variables locales, ¿hablamos de composición o de **"dependencia"**?

Cuando una clase utiliza otra únicamente de forma puntual —por ejemplo, recibiéndola como parámetro, devolviéndola como resultado, creando un objeto con `new` dentro de un método o usándola como variable local— no se está hablando de composición, sino de una **dependencia**. En este caso, la relación no forma parte de la estructura interna permanente de la clase, sino que aparece solo durante la ejecución de ciertos métodos.

La **composición** implica que una clase tiene otras como parte de su estado (normalmente como atributos), estableciendo una relación estable y duradera en el tiempo. En cambio, la **dependencia** es una relación más débil y temporal: una clase necesita a otra para realizar una operación concreta, pero no la “posee” ni la mantiene como parte de sí misma. Esta diferencia es importante porque la dependencia no define la estructura del objeto, sino solo su uso.

Desde el punto de vista del diseño, las dependencias indican acoplamiento entre clases, pero de menor intensidad que la composición. Reducir dependencias innecesarias suele mejorar la flexibilidad del sistema, mientras que la composición se utiliza cuando realmente existe una relación conceptual fuerte de “tiene-un”. Por tanto, los casos descritos (parámetros, variables locales, creación puntual con `new`) corresponden claramente a **dependencia**, no a composición.



## 6. En el ejemplo anterior de línea y punto, programa la relación entre `Linea` y `Punto` de dos formas. Una **como composición fuerte**, donde el ciclo de vida de los puntos está ligado al de Linea y otra **como composición débil**, donde no.

Se pueden modelar ambas situaciones cambiando **cómo se crean y gestionan los objetos `Punto` dentro de `Linea`**. La clave está en si `Linea` es responsable de crear y controlar los puntos (composición fuerte) o si simplemente usa puntos que existen fuera (composición débil / agregación).

En una **composición fuerte**, los puntos forman parte interna de la línea y su ciclo de vida depende completamente de ella. No se reciben desde fuera, sino que se crean dentro del constructor. Así se evita que esos puntos se compartan o existan independientemente. Además, se mantiene la inmutabilidad con `final`.

```java id="v7k3pz"
class Punto {
    private final double x;
    private final double y;

    public Punto(double x, double y) {
        this.x = x;
        this.y = y;
    }

    public double distanciaA(Punto otro) {
        double dx = this.x - otro.x;
        double dy = this.y - otro.y;
        return Math.sqrt(dx * dx + dy * dy);
    }
}

class LineaFuerte {
    private final Punto p1;
    private final Punto p2;

    // La línea crea sus propios puntos → composición fuerte
    public LineaFuerte(double x1, double y1, double x2, double y2) {
        this.p1 = new Punto(x1, y1);
        this.p2 = new Punto(x2, y2);
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```

En este diseño, los puntos **no existen fuera de la línea**. No pueden compartirse ni reutilizarse, y su vida está completamente ligada a `LineaFuerte`.
---

En una **composición débil (agregación)**, la línea recibe puntos ya creados. Estos pueden existir independientemente y ser compartidos por varias líneas. La línea no controla su ciclo de vida, solo los referencia.

```java id="e2k91m"
class LineaDebil {
    private final Punto p1;
    private final Punto p2;

    // La línea recibe puntos externos → composición débil (agregación)
    public LineaDebil(Punto p1, Punto p2) {
        this.p1 = p1;
        this.p2 = p2;
    }

    public double longitud() {
        return p1.distanciaA(p2);
    }
}
```

Aquí, los objetos `Punto` pueden ser utilizados por múltiples líneas o existir sin ninguna. Si una línea desaparece, los puntos siguen existiendo, lo que refleja la independencia de ciclo de vida.
La diferencia fundamental es el **control del ciclo de vida**: en composición fuerte, la clase contenedora crea y “posee” los objetos; en la débil, solo los usa.


## 7. En Java, en la composición fuerte, ¿cuando el contenedor destruye los objetos? No se observa que `Linea` destruya los `Punto` explícitamente, ¿Por qué?


En Java no existe destrucción explícita de objetos como en C o C++. Los objetos se crean en memoria dinámica y su liberación no depende directamente del programador, sino del mecanismo automático de **recolección de basura (garbage collector)**. Este sistema elimina objetos cuando detecta que ya no existe ninguna referencia accesible hacia ellos desde el programa .

En una **composición fuerte**, como en el ejemplo de `Linea` y `Punto`, el ciclo de vida de los puntos está ligado al de la línea a nivel lógico. Sin embargo, físicamente en memoria, los objetos `Punto` no se destruyen en el momento en que “desaparece” la línea, sino cuando dejan de ser accesibles. Es decir, si una instancia de `Linea` deja de estar referenciada (por ejemplo, porque ninguna variable apunta a ella), entonces también se pierden las referencias a sus puntos internos. En ese momento, tanto la línea como los puntos quedan candidatos a ser liberados por el recolector de basura.

Por eso no se observa ninguna instrucción explícita de destrucción en `Linea`. En Java, el programador no controla cuándo se libera la memoria ni llama a destructores como en C++. El sistema se encarga automáticamente de detectar qué objetos ya no pueden usarse y liberar su memoria en algún momento posterior. La composición fuerte, por tanto, no implica una destrucción manual, sino una **dependencia de accesibilidad**: cuando el contenedor deja de existir, sus componentes también dejan de ser accesibles y eventualmente serán eliminados.


## 8. Pon un ejemplo de composicion débil entre un departamento que tiene varios profesores. Implementa dos composiciones a la vez: entre el departamento y todos sus profesores y entre el departamento y su director, que es un profesor del departamento. Siempre debe haber un director en el departamento desde el inicio. Lanza excepciones si se viola la invariante. Emplea arrays primitivos de Java, estilo `Profesor[]`, con máximo 50, pero no rompas la encapsulación, no desveles que estás empleando un array, permite añadir un `Profesor` al final de la lista, y eliminar un profesor dada su posición. Da acceso a los profesores con un método para saber cuántos hay y otro para obtener un profesor por posición. El director se puede cambiar por otro profesor del departamento. Sin embargo, ten en cuenta esta invariante de clase: el director debe formar siempre parte de la lista de profesores, es decir, ten cuidado al cambiar el director o al eliminar un profesor.


En este caso se modela una **composición débil** porque los objetos `Profesor` existen independientemente del `Departamento`. El departamento solo mantiene referencias a profesores que podrían seguir existiendo aunque el departamento desapareciese. Aun así, el diseño debe preservar una invariante importante: **siempre debe existir un director y ese director debe estar incluido en la lista de profesores del departamento**. Para mantener esa regla, la clase no expone directamente el array interno, sino que ofrece métodos controlados para consultar, añadir, eliminar y cambiar el director .

La idea clave es que el constructor obliga a crear el departamento con un director inicial, y automáticamente ese profesor pasa a formar parte de la lista. Después, solo se permite nombrar director a un profesor que ya pertenezca al departamento. Además, no se permite eliminar al profesor que en ese momento sea director, salvo que antes se haya cambiado el director por otro profesor del departamento. Cuando alguna operación rompería la invariante o superaría la capacidad máxima, se lanza una excepción, siguiendo el uso normal de excepciones para impedir estados inválidos en los objetos .

```java
class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del profesor no puede ser nulo ni vacío");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    @Override
    public String toString() {
        return "Profesor(" + nombre + ")";
    }
}

class Departamento {
    private static final int MAX_PROFESORES = 50;

    private final Profesor[] profesores;
    private int numProfesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento debe crearse con un director");
        }

        this.profesores = new Profesor[MAX_PROFESORES];
        this.profesores[0] = directorInicial;
        this.numProfesores = 1;
        this.director = directorInicial;
    }

    public int getNumeroProfesores() {
        return numProfesores;
    }

    public Profesor getProfesor(int posicion) {
        comprobarPosicion(posicion);
        return profesores[posicion];
    }

    public Profesor getDirector() {
        return director;
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo");
        }
        if (numProfesores >= MAX_PROFESORES) {
            throw new IllegalStateException("No caben más profesores en el departamento");
        }
        if (contiene(profesor)) {
            throw new IllegalArgumentException("Ese profesor ya pertenece al departamento");
        }

        profesores[numProfesores] = profesor;
        numProfesores++;
    }

    public void eliminarProfesor(int posicion) {
        comprobarPosicion(posicion);

        if (profesores[posicion] == director) {
            throw new IllegalStateException(
                "No se puede eliminar al director; antes debe nombrarse otro director del departamento"
            );
        }

        for (int i = posicion; i < numProfesores - 1; i++) {
            profesores[i] = profesores[i + 1];
        }

        profesores[numProfesores - 1] = null;
        numProfesores--;
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        if (!contiene(nuevoDirector)) {
            throw new IllegalArgumentException(
                "El nuevo director debe ser un profesor que ya pertenezca al departamento"
            );
        }

        director = nuevoDirector;
    }

    private boolean contiene(Profesor profesor) {
        for (int i = 0; i < numProfesores; i++) {
            if (profesores[i] == profesor) {
                return true;
            }
        }
        return false;
    }

    private void comprobarPosicion(int posicion) {
        if (posicion < 0 || posicion >= numProfesores) {
            throw new IndexOutOfBoundsException("Posición de profesor no válida: " + posicion);
        }
    }
}
```
Con este diseño no se rompe la encapsulación porque desde fuera no se accede nunca al array `Profesor[]` directamente, sino solo mediante métodos públicos. Al mismo tiempo, la clase mantiene siempre su coherencia interna: hay director desde el principio, el director pertenece siempre a la lista y ninguna operación permitida deja al departamento en un estado inválido. Esa es precisamente la gracia de la encapsulación: impedir que el objeto acabe en un estado absurdo por un uso incorrecto desde fuera 


## 9. En Java, existen también `List`, cambia y muestra cómo sería el código anterior empleando `List` en vez de arrays primitivos. ¿Qué parte del código original te has ahorrado? Además, fíjate en el método `getProfesor(int pos)`: si en su lugar existiera un método que devolviera todos los profesores a la vez, ¿qué problema tendría devolver directamente la lista interna? ¿Cómo lo resolverías?

Al sustituir el array por `List<Profesor>`, la clase mantiene la misma idea de encapsulación, pero se simplifica bastante la gestión interna. Ya no hace falta llevar manualmente la cuenta de cuántos profesores hay, ni desplazar elementos al eliminar, ni comprobar si se supera una capacidad fija impuesta por el propio array. La invariante sigue siendo la misma: **siempre debe haber director y ese director debe pertenecer a la lista de profesores**. Lo que cambia es que parte del trabajo mecánico pasa a hacerlo la propia biblioteca de Java, lo que reduce código repetitivo y riesgo de errores .

En el código original con array se ahorran, sobre todo, estas partes: el atributo `numProfesores`, los bucles de desplazamiento al borrar, la gestión manual de posiciones libres y la limitación fija de tamaño 50. Con `List`, añadir al final se hace con `add`, eliminar por posición con `remove`, contar con `size` y acceder con `get`. Ahora bien, aparece una precaución importante: si existiera un método que devolviera **todos** los profesores, no convendría devolver directamente la lista interna, porque desde fuera podría modificarse y romper la encapsulación, por ejemplo eliminando al director o insertando valores no válidos. La forma habitual de resolverlo es devolver una **copia** de la lista o una **vista no modificable** de ella .

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

class Profesor {
    private final String nombre;

    public Profesor(String nombre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre del profesor no puede ser nulo ni vacío");
        }
        this.nombre = nombre;
    }

    public String getNombre() {
        return nombre;
    }

    @Override
    public String toString() {
        return "Profesor(" + nombre + ")";
    }
}

class Departamento {
    private final List<Profesor> profesores;
    private Profesor director;

    public Departamento(Profesor directorInicial) {
        if (directorInicial == null) {
            throw new IllegalArgumentException("El departamento debe crearse con un director");
        }

        this.profesores = new ArrayList<>();
        this.profesores.add(directorInicial);
        this.director = directorInicial;
    }

    public int getNumeroProfesores() {
        return profesores.size();
    }

    public Profesor getProfesor(int posicion) {
        return profesores.get(posicion);
    }

    public Profesor getDirector() {
        return director;
    }

    public void addProfesor(Profesor profesor) {
        if (profesor == null) {
            throw new IllegalArgumentException("No se puede añadir un profesor nulo");
        }
        if (profesores.contains(profesor)) {
            throw new IllegalArgumentException("Ese profesor ya pertenece al departamento");
        }

        profesores.add(profesor);
    }

    public void eliminarProfesor(int posicion) {
        Profesor profesorAEliminar = profesores.get(posicion);

        if (profesorAEliminar == director) {
            throw new IllegalStateException(
                "No se puede eliminar al director; antes debe nombrarse otro director"
            );
        }

        profesores.remove(posicion);
    }

    public void cambiarDirector(Profesor nuevoDirector) {
        if (nuevoDirector == null) {
            throw new IllegalArgumentException("El director no puede ser nulo");
        }
        if (!profesores.contains(nuevoDirector)) {
            throw new IllegalArgumentException(
                "El nuevo director debe pertenecer al departamento"
            );
        }

        director = nuevoDirector;
    }

    public List<Profesor> getProfesores() {
        return Collections.unmodifiableList(profesores);
        // Alternativa: return new ArrayList<>(profesores);
    }
}
```

Devolver `Collections.unmodifiableList(profesores)` permite consultar todos los profesores sin exponer la estructura interna a modificaciones externas. Esa es la diferencia importante: una cosa es dar acceso de lectura, y otra muy distinta regalar las llaves del objeto y esperar que nadie toque nada. Mala estrategia; gran forma de fabricar bugs. 


## 10. Al igual que ocurre con las excepciones en Java, que pueden encerrar causas (que son excepciones), de forma recursiva, suponen un tipo especial de composiciones, denominadas composiciones recursivas. Pon un ejemplo en Java de una `Persona`, que sea inmutable, y que tiene una madre, que es otra `Persona`. Haz un main con un ejemplo de uso con una familia de personas, desde el nieto hasta la abuela. Enumera algún otro ejemplo clásico de composiciones recursivas.


Una **composición recursiva** ocurre cuando una clase contiene referencias a objetos de su mismo tipo. En este caso, una `Persona` tiene una madre que también es una `Persona`. Este patrón es habitual para modelar estructuras jerárquicas o encadenadas (árboles, listas, etc.). Para mantener la inmutabilidad, los atributos se declaran `private final` y no se proporcionan métodos que modifiquen el estado una vez creado el objeto.

```java
class Persona {
    private final String nombre;
    private final Persona madre;

    public Persona(String nombre, Persona madre) {
        if (nombre == null || nombre.isBlank()) {
            throw new IllegalArgumentException("El nombre no puede ser nulo o vacío");
        }
        this.nombre = nombre;
        this.madre = madre; // puede ser null (por ejemplo, generación más antigua)
    }

    public String getNombre() {
        return nombre;
    }

    public Persona getMadre() {
        return madre;
    }

    @Override
    public String toString() {
        return nombre;
    }
}

public class Main {
    public static void main(String[] args) {
        Persona abuela = new Persona("Carmen", null);
        Persona madre = new Persona("Ana", abuela);
        Persona nieto = new Persona("Luis", madre);

        System.out.println("Nieto: " + nieto);
        System.out.println("Madre: " + nieto.getMadre());
        System.out.println("Abuela: " + nieto.getMadre().getMadre());
    }
}
```

En este ejemplo, cada objeto `Persona` es inmutable: su nombre y su madre quedan fijados en el momento de la creación. La relación es recursiva porque una persona contiene otra persona, y así sucesivamente. La última generación (la abuela) tiene `madre = null`, lo que actúa como caso base de la estructura.

Algunos ejemplos clásicos de composiciones recursivas son: estructuras de árboles (por ejemplo, nodos con hijos), listas enlazadas (cada nodo apunta al siguiente), sistemas de archivos (carpetas que contienen carpetas y archivos) o expresiones matemáticas (una expresión compuesta por subexpresiones). Estas estructuras comparten la idea de definirse en términos de sí mismas, lo que permite representar jerarquías de forma natural.


## 11. ¿Qué son las relaciones de composición "bidireccionales"? ¿Qué habría que hacer para implementar este tipo de relación en el ejemplo de `Profesor` y `Departamento`?


Una relación de composición (o asociación) **bidireccional** significa que ambas clases mantienen una referencia entre sí y pueden navegar la relación en los dos sentidos. Es decir, no solo `Departamento` conoce a sus `Profesor`, sino que cada `Profesor` también conoce a qué `Departamento` pertenece. Esto contrasta con una relación unidireccional, donde solo una de las clases tiene la referencia.

Para implementar esto en el ejemplo, habría que añadir en la clase `Profesor` un atributo que referencie al `Departamento`. Ese atributo debería mantenerse coherente con la lista de profesores del departamento. Es decir, cuando un profesor se añade a un departamento, también debe actualizarse su referencia interna, y cuando se elimina, esa referencia debe ponerse a `null`. Además, habría que controlar que un profesor no pertenezca a dos departamentos a la vez (si esa es la restricción del modelo).

```java id="n4x7pf"
class Profesor {
    private final String nombre;
    private Departamento departamento; // referencia inversa

    public Profesor(String nombre) {
        this.nombre = nombre;
    }

    void setDepartamento(Departamento d) {
        this.departamento = d;
    }

    public Departamento getDepartamento() {
        return departamento;
    }
}
```
Y en `Departamento`, habría que modificar los métodos para mantener la coherencia:

```java
public void addProfesor(Profesor p) {
    if (p == null) throw new IllegalArgumentException();

    if (p.getDepartamento() != null) {
        throw new IllegalStateException("El profesor ya pertenece a otro departamento");
    }

    profesores.add(p);
    p.setDepartamento(this); // sincronización bidireccional
}

public void eliminarProfesor(int pos) {
    Profesor p = profesores.get(pos);

    if (p == director) {
        throw new IllegalStateException("No se puede eliminar al director");
    }

    profesores.remove(pos);
    p.setDepartamento(null); // romper relación en ambos lados
}
```
La dificultad principal de las relaciones bidireccionales es mantener la **consistencia**: cualquier cambio en un lado debe reflejarse en el otro. Si no se hace correctamente, pueden aparecer estados incoherentes (por ejemplo, un profesor que apunta a un departamento que no lo contiene en su lista). Por ello, es habitual centralizar las modificaciones en una sola clase (en este caso `Departamento`) y restringir el acceso directo a los métodos que alteran la relación.

