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

### Respuesta


## 8. En Java, sobre el bloque **"try-catch"**, ¿se pueden tener más de un bloque `catch`? ¿cuántos bloques `catch` se ejecutan?

### Respuesta


## 9. Si las excepciones producen rupturas en el código llamador, ¿cómo podemos garantizar que se ejecuta siempre finalmente un código necesario para cierre de ficheros, liberacion de recursos, antes de que continúe propagándose la excepción? Pon un ejemplo en Java con `finally`, tanto con `catch` como sin él.

### Respuesta


## 10. En Java, el bloque `finally` puede ir sin `catch`? ¿Se ejecuta siempre tanto si ocurre como si no ocurre una excepción? ¿Y si hay un `return` en medio del `try`?

### Respuesta


## 11. En Java, qué son las excepciones **"controladas"** y las **"no controladas"**? ¿Qué papel juega `RuntimeException`? Pon un ejemplo de excepciones típicas controladas y no controladas que incluso nosotros mismos podríamos usar. Haz dos listas con 3 o 4 ejemplos de situación donde se suele preferir una excepción controlada y donde se suele preferir una excepción no controlada.

### Respuesta


## 12. ¿Qué es y para qué se usa `throws`? ¿Por qué es alternativa a capturar una excepción controlada?

### Respuesta


## 13. Pon un ejemplo en Java de firma de método que incluya `throws`, de una función que abre un fichero pero que declara que no le interesa menejar la excepción de si el fichero no existe, sino que se propague hacia arriba. Eso sí, acuérdate del `finally`.

### Respuesta


## 14. ¿Podemos poner en `throws` excepciones no controladas, como `RuntimeException`? ¿Debería el método llamador entonces poner `try-catch` en ese caso? ¿Qué sentido tendría?

### Respuesta


## 15. ¿Cuándo se recomienda usar excepciones controladas, como `IOException`, y cuándo no controladas como `IllegalArgumentException`? ¿Existen en todos los lenguajes ambas opciones? En los que sólo existe una opción, ¿cuál es la más habitual?

### Respuesta


## 16. ¿Tiene sentido lanzar excepciones dentro del `catch`? ¿Se puede relanzar la misma excepción capturada? ¿Cuándo tendría sentido hacer esto último? Pon ejemplos de ambos casos.

### Respuesta


## 17. ¿En qué consiste que una excepción sea la **"causa"** de otra excepción? Pon un ejemplo en Java, donde capturemos una excepción de bajo nivel y la encapsulemos en otra personalizada de alto nivel. Cuando una excepción sale por pantalla y tiene una causa, ¿se ve?

### Respuesta

