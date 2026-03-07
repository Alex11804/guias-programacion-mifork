<!--
Posible prompt:
<prompt>
Tengo un cuestionario con preguntas sobre "Excepciones". Debes tener en cuenta que los conocimientos previos que tengo (y por tanto tus respuestas deben ser adaptadas), son:
- C/C++ sin orientación a objetos.
- Temas de Java previos: Clases y Objetos, Encapsulación.

Cada respuesta debe tener entre 2 - 4 párrafos de longitud (sin contar los trozos de código).

Por favor, escribe en impersonal las respuestas.

</prompt>
----
-->
# TEMA 3. Excepciones

## 1. Empecemos un tema sobre control de errores en lenguajes de programación, con algo básico. En C, donde no existen las excepciones, pongamos un ejemplo de una raíz que toma número flotante positivo. Queremos controlar el error si la función recibe un número negativo. El usuario debe ser informado pero desde fuera de la función `raiz` ¿Cómo indicamos ese error?. Enumera dos opciones diferentes de diseñar, poniendo un ejemplo de código de cada una.

En C no existen excepciones como mecanismo integrado del lenguaje, por lo que el control de errores debe diseñarse manualmente. Si se define una función `raiz` que calcula la raíz cuadrada de un número flotante positivo, se necesita algún modo de indicar al código que la invoca que se ha producido un error cuando el valor recibido es negativo. El objetivo es que el mensaje al usuario se gestione fuera de la función, separando el cálculo del tratamiento del error.

Una primera opción consiste en devolver un valor especial que indique error. Por ejemplo, puede devolverse un número negativo imposible como resultado válido (si se sabe que la raíz nunca será negativa) o utilizar `NAN` de `<math.h>`. El código que llama a la función comprueba ese valor y decide cómo actuar.

```c
#include <stdio.h>
#include <math.h>

float raiz(float x) {
    if (x < 0) {
        return -1.0;  // valor especial que indica error
    }
    return sqrtf(x);
}

int main() {
    float resultado = raiz(-9.0);

    if (resultado < 0) {
        printf("Error: no se puede calcular la raíz de un número negativo.\n");
    } else {
        printf("Resultado: %.2f\n", resultado);
    }

    return 0;
}
```

Una segunda opción consiste en devolver un código de estado independiente del resultado, utilizando parámetros por referencia (punteros). La función puede devolver 0 si todo va bien y 1 si hay error, mientras que el resultado real se almacena en una variable pasada por dirección. Este diseño separa claramente el valor calculado del estado de la operación.

```c
#include <stdio.h>
#include <math.h>

int raiz(float x, float *resultado) {
    if (x < 0) {
        return 1;  // código de error
    }
    *resultado = sqrtf(x);
    return 0;      // éxito
}

int main() {
    float resultado;
    int estado = raiz(-9.0, &resultado);

    if (estado != 0) {
        printf("Error: no se puede calcular la raíz de un número negativo.\n");
    } else {
        printf("Resultado: %.2f\n", resultado);
    }

    return 0;
}
```

En ambos casos el control del error se realiza explícitamente en el código que llama a la función, lo que obliga a comprobar manualmente cada resultado. Esta forma de diseño es habitual en C, pero puede hacer que el código sea más propenso a errores si se olvida realizar la comprobación.



## 2. Brevemente ¿Qué es una **"excepción"**? ¿Con qué objetivo las usa un programador cuando implementa funciones o cuando las llama?

Una excepción es un mecanismo de control de errores que permite señalar que durante la ejecución de un programa se ha producido una situación anómala que impide continuar con el flujo normal de instrucciones. En lugar de devolver códigos de error manualmente, el sistema interrumpe la ejecución habitual y transfiere el control a un bloque específico preparado para tratar ese problema.

El objetivo de utilizar excepciones al implementar funciones es separar claramente el código que realiza la tarea principal del código que gestiona los errores. De este modo, la función puede centrarse en su responsabilidad principal y, si ocurre una condición inválida (por ejemplo, un parámetro incorrecto o un recurso no disponible), lanzar una excepción para indicar el problema sin mezclar la lógica del cálculo con la del tratamiento del error.
Cuando se llama a una función que puede generar excepciones, el programador utiliza estructuras como `try` y `catch` para capturarlas y decidir cómo actuar. Esto permite centralizar el tratamiento de errores, mejorar la claridad del código y reducir la probabilidad de ignorar fallos, algo que en lenguajes como C debe controlarse manualmente tras cada llamada a función.


## 3. Reescribe el mismo ejemplo de raiz, pero en Java, metiendo ese método en una clase `Calculadora` y llama a dicho método desde el método `main`, mostrando cómo se puede controlar desde fuera.

En Java el control de errores puede realizarse mediante excepciones. En lugar de devolver valores especiales o códigos de estado, el método puede lanzar una excepción cuando detecta una situación inválida, como recibir un número negativo. De este modo, el método se limita a calcular la raíz cuando el dato es correcto y delega el tratamiento del error al código que lo invoque.
Se define una clase `Calculadora` con un método `raiz` que comprueba si el número es negativo. Si lo es, se lanza una `IllegalArgumentException`, que es una excepción estándar de Java para indicar argumentos no válidos. Si el valor es correcto, se utiliza `Math.sqrt()` para obtener el resultado.

public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            throw new IllegalArgumentException(
                "No se puede calcular la raíz de un número negativo");
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        try {
            double resultado = Calculadora.raiz(-9);
            System.out.println("Resultado: " + resultado);
        } catch (IllegalArgumentException e) {
            System.out.println("Error: " + e.getMessage());
        }
    }
}

En este diseño, el método `raiz` no imprime mensajes ni decide cómo reaccionar ante el error; simplemente lanza la excepción. El control se realiza desde el método `main` mediante un bloque `try-catch`, que captura la excepción y muestra el mensaje correspondiente. Así se consigue separar el cálculo del tratamiento del error, que es precisamente uno de los objetivos fundamentales del uso de excepciones en Java.

## 4. ¿Qué es **"lanzar"** una excepción? ¿Qué es **"controlar"** o **"capturar"** una excepción? ¿Qué es que se **"propague"** una excepción? ¿Qué le va ocurriendo a las funciones en la pila de llamadas por donde se va propagando la excepción? ¿Las funciones que no la controlan se reanudan después de alguna forma? Explica con el mismo ejemplo anterior en Java de la raíz cuadrada.

Lanzar una excepción es provocar de forma explícita (con `throw`) o implícita (por un fallo interno) un evento de error que interrumpe el flujo normal del método en el punto exacto donde ocurre.  
Capturar o Controlar una excepción es interceptarla con un `catch` (normalmente asociado a un `try`) para decidir qué hacer: informar, recuperar la ejecución con otro plan, o terminar de forma ordenada. 
Propagar una excepción significa que, si en el método donde se produce no se captura, el control “sube” al método que lo llamó, buscando algún `catch` compatible.

Cuando una excepción se propaga, Java va “desenrollando” la pila de llamadas: el método donde ocurre la excepción finaliza de inmediato sin ejecutar las líneas posteriores (salvo los `finally`), y se vuelve al llamador; si el llamador tampoco la captura, también se abandona, y así sucesivamente. En cada nivel, lo que ocurre es que se abandona el método como si fuera un retorno abrupto, pero sin devolver un valor normal: el control salta a un `catch` adecuado si existe, o continúa subiendo. Si nadie la captura, el programa termina mostrando información del error.

Las funciones (métodos) que no controlan la excepción **no se reanudan** después “como si nada”: el flujo normal no continúa desde la línea siguiente a la llamada que falló. Solo se ejecuta código de limpieza en `finally` (si existe) y, si algún nivel captura la excepción, la ejecución continúa **después del bloque `try-catch`** en ese nivel (no dentro del método que ya fue abandonado). En otras palabras: los métodos por los que pasa la propagación quedan abortados y no retoman su ejecución normal.

Aplicado al ejemplo de la raíz cuadrada: si `raiz(-9)` lanza `IllegalArgumentException` y no se captura dentro de `raiz`, la excepción sube a `main`. Si `main` tiene un `try-catch`, se captura allí y se muestra el mensaje; si `main` no la captura, el programa termina.
public class Calculadora {

    public static double raiz(double x) {
        if (x < 0) {
            // LANZAR: se interrumpe el método aquí
            throw new IllegalArgumentException("Número negativo: " + x);
        }
        return Math.sqrt(x);
    }

    public static void main(String[] args) {
        System.out.println("Antes de llamar a raiz");
        try {
            // Si raiz lanza, esta línea no completa normalmente
            double r = raiz(-9);
            System.out.println("Resultado: " + r); // no se ejecuta si hay excepción
        } catch (IllegalArgumentException e) {
            // CAPTURAR/CONTROLAR: aquí llega la propagación
            System.out.println("Error controlado en main: " + e.getMessage());
        }
        System.out.println("Después del try-catch (el programa continúa aquí)");
    }
}

## 5. ¿Qué ventajas tiene frente a C, la **"propagación natural"** de las excepciones a través de la pila (*stack*) de llamadas?

La propagación natural de excepciones permite que un error detectado en un nivel bajo del programa (por ejemplo, en un método auxiliar) ascienda automáticamente por la pila de llamadas hasta encontrar un punto adecuado donde tratarlo. En C, el programador debe comprobar manualmente cada valor de retorno tras cada llamada, lo que obliga a repetir código de comprobación en todos los niveles. En Java, si un método no captura la excepción, esta se transmite automáticamente al método que lo invocó.
Una ventaja clara es la reducción de código repetitivo. No es necesario añadir comprobaciones tras cada llamada ni diseñar códigos de error específicos que deban ser inspeccionados constantemente. Esto mejora la legibilidad y disminuye la probabilidad de olvidar una comprobación, algo frecuente en C cuando se encadenan varias llamadas a funciones.
Otra ventaja importante es la separación de responsabilidades. Los métodos pueden centrarse en su lógica principal y delegar el tratamiento del error a niveles superiores, donde se dispone de más contexto para decidir cómo actuar (reintentar, informar al usuario, finalizar el programa, etc.). Esto favorece un diseño más modular y estructurado.
Finalmente, la propagación automática mantiene coherente el estado del programa al interrumpir de inmediato la ejecución anómala. En C, si no se revisa correctamente un código de error, el programa puede continuar en un estado inválido. En Java, si no se captura la excepción, el programa termina de forma controlada mostrando el error, evitando que la ejecución continúe de manera inconsistente.

## 6. En orientación a objetos, ¿las excepciones suelen ser objetos? ¿Qué ventajas tiene esto en términos de encapsulación? ¿Podemos entonces crear excepciones personalizadas?

En programación orientada a objetos, las excepciones suelen ser objetos. En Java, todas las excepciones son instancias de clases que heredan de `Throwable` (normalmente de `Exception` o `RuntimeException`). Esto significa que una excepción no es solo un código numérico, sino una entidad que puede contener información y comportamiento propios.
Al ser objetos, las excepciones se benefician de la encapsulación. Pueden tener atributos privados que almacenen datos relevantes del error (por ejemplo, el valor incorrecto recibido, un identificador, o información adicional), y métodos públicos para acceder a esa información de forma controlada. De esta manera, el detalle interno del problema queda protegido, y el resto del programa interactúa con la excepción a través de una interfaz bien definida.

Sí, es posible crear excepciones personalizadas. Basta con definir una clase que herede de `Exception` (para excepciones comprobadas) o de `RuntimeException` (para no comprobadas). Esto permite modelar errores específicos del dominio de la aplicación y aportar mensajes o datos más precisos.

public class NumeroNegativoException extends Exception {

    private double valor;

    public NumeroNegativoException(double valor) {
        super("No se admite número negativo: " + valor);
        this.valor = valor;
    }

    public double getValor() {
        return valor;
    }
}

## 7. En relación con las ventajas de la encapsulación, comparando el ejemplo en C con Java. ¿Qué **información esencial** lleva cualquier **objeto excepción** que es muy útil tener cuando se llega a un manejador?

En Java, una excepción es un objeto que transporta información sobre el error ocurrido. Cuando se captura en un manejador (`catch`), ese objeto contiene datos que ayudan a entender qué ha sucedido y dónde. Una de las informaciones más importantes es el mensaje descriptivo del error, que explica la causa del problema. Este mensaje se puede obtener mediante el método `getMessage()` y permite comunicar de forma clara la naturaleza del fallo.

Otra información esencial es el tipo de la excepción (la clase concreta del objeto). El tipo permite distinguir entre diferentes clases de problemas, por ejemplo `IllegalArgumentException`, `IOException`, etc. Gracias a esto se pueden definir manejadores distintos para cada tipo de error, lo que facilita aplicar respuestas específicas según la situación.

También es muy útil la traza de la pila de llamadas (*stack trace*). Esta traza indica la secuencia de métodos que estaban ejecutándose cuando ocurrió la excepción y el punto exacto del programa donde se produjo. Esta información se puede mostrar con métodos como `printStackTrace()` y resulta especialmente valiosa durante la depuración.

En comparación con el enfoque típico en C, donde normalmente solo se dispone de un código de error o un valor especial de retorno, un objeto excepción encapsula simultáneamente el tipo de error, un mensaje explicativo y el contexto de ejecución. Esto proporciona mucha más información al manejador y facilita localizar y comprender el origen del problema.

## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

En Java es posible tener varios bloques `catch` asociados a un mismo bloque `try`. Cada bloque `catch` está diseñado para capturar un tipo concreto de excepción, lo que permite tratar de forma distinta diferentes errores que puedan producirse dentro del `try`. De esta forma, el programador puede especificar manejadores especializados para cada clase de problema.
Cuando ocurre una excepción dentro del bloque `try`, el sistema busca entre los bloques `catch` aquel cuyo tipo de excepción sea compatible con la excepción lanzada. Esta búsqueda se realiza en orden, desde el primer `catch` hacia abajo. El primer bloque que coincida con el tipo de la excepción será el que se ejecute.

Sin embargo, solo se ejecuta un único bloque `catch`. Una vez que una excepción ha sido capturada por un manejador, los demás bloques `catch` asociados al mismo `try` se ignoran. Después de ejecutar ese `catch`, el flujo del programa continúa en la primera instrucción situada después del conjunto `try-catch`.

try {
    double r = Calculadora.raiz(-9);
    System.out.println(r);
} catch (IllegalArgumentException e) {
    System.out.println("Argumento inválido: " + e.getMessage());
} catch (ArithmeticException e) {
    System.out.println("Error aritmético");
} catch (Exception e) {
    System.out.println("Otro tipo de error");
}

En este ejemplo se definen tres posibles manejadores. Si el método `raiz` lanza una `IllegalArgumentException`, se ejecutará únicamente el primer `catch`. Los otros bloques no se ejecutarán, aunque su tipo también pudiera ser compatible con excepciones distintas.

## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

Cuando se produce una excepción, el flujo normal del programa se interrumpe y puede comenzar su propagación por la pila de llamadas. Sin embargo, en muchos casos es necesario ejecutar siempre ciertas acciones antes de abandonar un bloque de código, como cerrar ficheros, liberar recursos o finalizar operaciones pendientes. En Java esto se consigue utilizando el bloque `finally`.

El bloque `finally` se ejecuta siempre, independientemente de que ocurra o no una excepción dentro del `try`. También se ejecuta tanto si la excepción se captura con un `catch` como si continúa propagándose hacia niveles superiores. Su finalidad es garantizar la ejecución de código de limpieza o liberación de recursos, incluso cuando la ejecución normal del programa ha sido interrumpida.

Un ejemplo con `catch` podría ser el siguiente. Aquí se intenta realizar una operación que puede fallar, se captura la excepción si ocurre, y el bloque `finally` asegura que se ejecute el código de cierre.

try {
    System.out.println("Abriendo recurso...");
    
    double r = Calculadora.raiz(-9);  // puede lanzar excepción
    System.out.println("Resultado: " + r);

} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());

} finally {
    System.out.println("Cerrando recurso (siempre se ejecuta)");
}

También es posible utilizar `finally` **sin bloque `catch`**. En este caso, si ocurre una excepción, el código del `finally` se ejecuta y después la excepción continúa propagándose al método llamador.

try {
    System.out.println("Abriendo recurso...");

    double r = Calculadora.raiz(-9);  // lanza excepción
    System.out.println("Resultado: " + r);

} finally {
    System.out.println("Cerrando recurso (siempre se ejecuta)");
}

En este segundo caso no se captura la excepción en ese punto, pero se garantiza que el código del `finally` se ejecute antes de que la excepción siga propagándose. Esto permite asegurar que los recursos se liberen correctamente incluso en situaciones de error.

## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

En Java el bloque `finally` puede utilizarse sin un bloque `catch`. La estructura mínima permitida es `try` seguido de `finally`. En este caso no se captura ninguna excepción en ese punto, pero se garantiza que el código del `finally` se ejecute antes de que el control abandone el bloque `try`, incluso si se ha producido una excepción que continuará propagándose hacia el método llamador.
El bloque `finally` se ejecuta tanto si ocurre una excepción como si no ocurre. Si el código del `try` termina normalmente, se ejecuta el `finally` antes de continuar con las instrucciones posteriores. Si ocurre una excepción, primero se ejecuta el `finally` y después la excepción sigue propagándose si no ha sido capturada.
También se ejecuta el `finally` aunque exista un `return` dentro del `try`. Cuando se alcanza el `return`, Java prepara el valor de retorno pero antes de abandonar el método ejecuta el código del bloque `finally`. Solo después de ejecutar ese bloque se produce realmente el retorno del método.


public static int ejemplo() {
    try {
        System.out.println("Dentro del try");
        return 10;
    } finally {
        System.out.println("Se ejecuta el finally antes de salir del método");
    }
}

En este ejemplo, al llamar al método se imprimirá primero el mensaje del `try` y después el del `finally`. Solo tras ejecutar el `finally` el método devolverá el valor `10`. Esto demuestra que el bloque `finally` se ejecuta incluso cuando el flujo del programa abandona el `try` mediante un `return`.

## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

En Java existen dos grandes categorías de excepciones: controladas (*checked exceptions*) y no controladas (*unchecked exceptions*). Las excepciones controladas son aquellas que el compilador obliga a manejar explícitamente. Si un método puede producir una de estas excepciones, el programador debe capturarla con `try-catch` o declararla con `throws` en la firma del método. Este mecanismo fuerza a que el programador tenga en cuenta ciertas situaciones de error previsibles.

Las excepciones no controladas son aquellas que no requieren ser declaradas ni capturadas obligatoriamente. Estas excepciones heredan de `RuntimeException`. El compilador no exige que se gestionen porque suelen representar errores de programación o situaciones que normalmente no deberían ocurrir si el código está correctamente escrito. Aunque pueden capturarse, no es obligatorio hacerlo.
La clase `RuntimeException` actúa como base para la mayoría de las excepciones no controladas. Todas las excepciones que heredan de ella se consideran excepciones que pueden producirse durante la ejecución normal del programa sin que el compilador obligue a tratarlas. Ejemplos típicos incluyen `NullPointerException` o `ArithmeticException`.

Ejemplos de **excepciones controladas** habituales (o que podría definir un programador):

* `IOException` (problemas al leer o escribir archivos).
* `FileNotFoundException` (archivo inexistente al intentar abrirlo).
* `SQLException` (errores al interactuar con una base de datos).
* `DatosInvalidosException` (excepción personalizada para indicar datos incorrectos en una aplicación).

Ejemplos de **excepciones no controladas**:

* `NullPointerException` (uso de una referencia que vale `null`).
* `ArithmeticException` (errores aritméticos como división por cero).
* `IndexOutOfBoundsException` (acceso a posiciones inválidas en arrays o listas).
* `IllegalArgumentException` (argumentos incorrectos al llamar a un método).

Situaciones donde suele preferirse una **excepción controlada**:

* Fallos al acceder a archivos o recursos externos.
* Problemas al comunicarse con bases de datos o servicios externos.
* Errores de validación en datos que provienen del usuario o de entrada externa.
* Operaciones que dependen de recursos que pueden no estar disponibles.

Situaciones donde suele preferirse una **excepción no controlada**:

* Errores de programación que indican un uso incorrecto de una API.
* Parámetros inválidos pasados a un método por parte del propio programa.
* Accesos fuera de rango en estructuras de datos.
* Estados internos del programa que no deberían ocurrir si el diseño es correcto.



## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

En Java, la palabra clave `throws` se utiliza en la declaración de un método para indicar que dicho método puede lanzar una o varias excepciones. Al incluir `throws` en la firma del método, se informa al compilador y a los programadores que utilicen ese método de que durante su ejecución podría producirse ese tipo de error. De este modo, el método no gestiona la excepción internamente, sino que declara que puede producirla.

El uso principal de `throws` aparece con las excepciones controladas (*checked exceptions*). Cuando un método puede generar una excepción de este tipo, el compilador obliga a que el programador haga una de dos cosas: capturarla con un bloque `try-catch` o declararla con `throws` para que se propague al método que realiza la llamada. Por tanto, `throws` forma parte del contrato del método y obliga al código que lo invoque a considerar esa posibilidad de error.

Se considera una alternativa a capturar la excepción porque permite no manejar el error en ese nivel del programa. En lugar de tratarlo inmediatamente, el método lo deja pasar hacia arriba en la pila de llamadas. Esto es útil cuando el método no dispone de suficiente contexto para decidir qué hacer ante el error, y es preferible que la decisión se tome en un nivel superior del programa.

Un ejemplo sencillo podría ser el siguiente:

import java.io.*;

public class Ejemplo {

    public static void leerArchivo(String nombre) throws IOException {
        FileReader f = new FileReader(nombre);
        f.close();
    }

    public static void main(String[] args) {
        try {
            leerArchivo("datos.txt");
        } catch (IOException e) {
            System.out.println("Error al leer el archivo: " + e.getMessage());
        }
    }
}
En este caso el método `leerArchivo` declara con `throws IOException` que puede producir esa excepción. El método `main`, que realiza la llamada, decide capturarla con un bloque `try-catch` y gestionar el error en ese punto.


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

En Java un método puede declarar con `throws` que una excepción controlada debe **propagarse al método llamador**. Esto se hace cuando el método no tiene suficiente contexto para decidir cómo reaccionar ante el error. Por ejemplo, un método que abre un fichero puede limitarse a realizar la operación y dejar que el método que lo llama decida qué hacer si el fichero no existe.
En este caso se utiliza `throws FileNotFoundException` (o `IOException`) en la firma del método. El método no captura la excepción con `catch`, por lo que si ocurre el problema la excepción se propagará automáticamente hacia arriba en la pila de llamadas. Sin embargo, aun así puede utilizarse un bloque `finally` para garantizar la liberación de recursos, como el cierre del fichero.

Un ejemplo simplificado podría ser el siguiente:

import java.io.*;

public class Lector {

    public static void leerPrimeraLinea(String nombreFichero) 
            throws FileNotFoundException, IOException {

        BufferedReader br = null;

        try {
            br = new BufferedReader(new FileReader(nombreFichero));
            System.out.println(br.readLine());

        } finally {
            if (br != null) {
                br.close();
            }
        }
    }
}
En este diseño el método `leerPrimeraLinea` declara las excepciones con `throws` y no las captura. Si el fichero no existe o ocurre un error de entrada/salida, la excepción se propagará al método que haya llamado a esta función. El bloque `finally` se utiliza para asegurar que el fichero se cierre correctamente en caso de que haya sido abierto.

## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

En Java es posible incluir excepciones no controladas en la cláusula `throws`, por ejemplo aquellas que heredan de `RuntimeException`. El lenguaje no lo prohíbe, aunque no es obligatorio hacerlo. A diferencia de las excepciones controladas, el compilador no exige que estas excepciones aparezcan en la firma del método ni que se capturen explícitamente.

Cuando un método declara una excepción no controlada en `throws`, el método llamador no está obligado a capturarla con un `try-catch`. El programa compilará igualmente aunque no exista ningún bloque de captura. Esto ocurre porque las excepciones derivadas de `RuntimeException` se consideran errores que pueden producirse durante la ejecución normal del programa y cuya gestión no siempre es necesaria en cada punto del código.

Incluir una excepción no controlada en `throws` tiene principalmente un valor informativo o documental. Sirve para indicar a quien utilice el método que podría producirse cierta condición de error. De esta forma se comunica mejor el comportamiento del método, aunque el compilador no obligue a manejar esa situación.
El método llamador puede decidir capturar esa excepción si tiene sentido hacerlo en ese contexto. Por ejemplo, podría capturarse una `IllegalArgumentException` para mostrar un mensaje al usuario o registrar el error. Sin embargo, en muchos casos se deja que estas excepciones se propaguen hasta un nivel superior del programa, donde exista un manejador general o donde se decida terminar la ejecución.

## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

Se recomienda utilizar excepciones controladas cuando el error representa una situación previsible y recuperable, especialmente si depende del entorno o de elementos externos al programa. Es el caso de abrir un fichero, acceder a una base de datos o comunicarse con una red: son operaciones que pueden fallar aunque el código esté bien escrito. En esas situaciones tiene sentido que el compilador obligue a tener en cuenta el posible error, porque el método llamador quizá pueda tomar una decisión razonable, como reintentar, pedir otro nombre de fichero o informar al usuario.

Se suele preferir una excepción no controlada cuando el problema indica un uso incorrecto del método o un error de programación, no una circunstancia externa. `IllegalArgumentException` es el ejemplo típico: si un método recibe un argumento inválido, lo normal es considerar que el fallo está en quien llamó al método. En ese caso no suele tener mucho sentido forzar con el compilador a capturar la excepción en todos los lugares, porque lo adecuado es corregir el código. Dicho de forma simple: si el error “puede pasar en la realidad”, suele encajar mejor una controlada; si el error “no debería pasar si el programa está bien hecho”, suele encajar mejor una no controlada.

No en todos los lenguajes existen ambas opciones de la misma forma que en Java. Hay lenguajes que distinguen claramente entre excepciones controladas y no controladas, y otros en los que todas las excepciones funcionan prácticamente como no controladas. En C, directamente no hay excepciones integradas; en C++ existen excepciones, pero no una separación como la de Java; en Python tampoco existe una obligación del compilador de declarar o capturar ciertos tipos. Por tanto, el modelo de Java no es universal.

En los lenguajes donde solo existe una opción práctica, la más habitual es el modelo parecido al de las no controladas, es decir, excepciones que pueden lanzarse y capturarse, pero sin obligación de declararlas en la firma ni tratarlas explícitamente en cada llamada. Es un modelo más flexible y menos rígido, aunque también deja más responsabilidad al programador. Java aquí fue bastante especial: quiso forzar el tratamiento de ciertos errores, y por eso introdujo las excepciones controladas.

## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

Sí, tiene sentido lanzar excepciones dentro de un `catch`. Un `catch` no está obligado a “resolver” el problema por completo; también puede usar la información de la excepción capturada para reaccionar y después lanzar otra excepción. Esto suele hacerse cuando se quiere traducir una excepción de bajo nivel a otra más adecuada al nivel de abstracción del programa. Por ejemplo, un error técnico de lectura de fichero puede convertirse en una excepción más cercana al dominio del problema, con un mensaje más útil para quien usa esa clase.

También se puede relanzar la misma excepción capturada usando `throw e;`. En ese caso no se crea una excepción nueva, sino que se deja que el error continúe propagándose después de haber hecho alguna acción previa, como registrar información, liberar recursos adicionales o dejar constancia en un log. Esto tiene sentido cuando en ese nivel no se puede solucionar el problema, pero sí interesa realizar una tarea intermedia antes de que otro método superior lo gestione.

Lanzar una nueva excepción dentro del `catch` tiene sentido cuando conviene ocultar detalles internos o presentar una interfaz más limpia. Relanzar la misma excepción tiene sentido cuando se quiere añadir contexto externo sin cambiar el error original. En ambos casos, el `catch` no “corta” necesariamente la propagación; simplemente puede modificar cómo continúa esa propagación.

Ejemplo de lanzar una nueva excepción dentro del `catch`:

import java.io.*;

public class GestorFichero {

    public static void procesarFichero(String nombre) throws Exception {
        try {
            BufferedReader br = new BufferedReader(new FileReader(nombre));
            System.out.println(br.readLine());
            br.close();
        } catch (IOException e) {
            throw new Exception("No se ha podido procesar el fichero de entrada", e);
        }
    }
}

Ejemplo de relanzar la misma excepción capturada:

import java.io.*;

public class GestorFichero {

    public static void leerFichero(String nombre) throws IOException {
        try {
            BufferedReader br = new BufferedReader(new FileReader(nombre));
            System.out.println(br.readLine());
            br.close();
        } catch (IOException e) {
            System.out.println("Se ha detectado un error de entrada/salida");
            throw e;   // se relanza la misma excepción
        }
    }
}

En el primer caso se cambia el tipo de excepción que se propaga, aportando una visión más abstracta del problema. En el segundo caso se conserva exactamente la misma excepción original, pero se aprovecha el `catch` para realizar una acción antes de que siga subiendo por la pila de llamadas.

## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

En Java se dice que una excepción es la “causa” de otra cuando una excepción original (de bajo nivel) provoca que se lance una nueva excepción de nivel más alto, conservando la referencia a la primera. Este mecanismo permite encapsular el error original dentro de otro más adecuado al nivel de abstracción del programa. Así se mantiene la información técnica del problema sin exponer directamente detalles internos a las capas superiores.

Este enfoque se utiliza cuando una clase detecta un error de bajo nivel (por ejemplo, un problema de entrada/salida) pero desea comunicar un error más significativo para el contexto de la aplicación. En lugar de perder la excepción original, se pasa como causa al constructor de la nueva excepción. De este modo se mantiene la relación entre ambas y se conserva toda la información útil para depuración.

Un ejemplo consiste en capturar una excepción `IOException` y envolverla dentro de una excepción personalizada que represente un problema lógico de la aplicación:

import java.io.*;

class ErrorProcesamientoException extends Exception {
    public ErrorProcesamientoException(String mensaje, Throwable causa) {
        super(mensaje, causa);
    }
}

public class Procesador {

    public static void procesar(String fichero) throws ErrorProcesamientoException {
        try {
            BufferedReader br = new BufferedReader(new FileReader(fichero));
            System.out.println(br.readLine());
            br.close();
        } catch (IOException e) {
            throw new ErrorProcesamientoException(
                "No se pudo procesar el fichero", e);
        }
    }
}

En este caso, `IOException` es la causa de `ErrorProcesamientoException`. Si la excepción final se muestra por pantalla (por ejemplo con `printStackTrace()`), **la causa también aparece**. La traza normalmente muestra primero la excepción principal y después una sección indicando *“Caused by”*, seguida de la excepción original y su propia traza de pila. Esto permite ver tanto el error de alto nivel como el problema técnico que lo provocó.


