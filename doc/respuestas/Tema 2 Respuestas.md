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

### Respuesta


## 8. Responde: Los miembros de instancia privados de un objeto están ocultos para (a) otras clases o (b) otras instancias, aunque sean de la misma clase. Pon un ejemplo añadiendo un método `calcularDistanciaAPunto(Punto otro)` y explica la respuesta.

### Respuesta


## 9. ¿Qué son los métodos "getter" y "setter" en los lenguajes orientados a objetos?

### Respuesta


## 10. Cuando nos referimos a que la ocultación de información mejora la "seguridad" del programa, ¿nos referimos a que no pueda ser "hackeado"?

### Respuesta


## 11. ¿Qué diferencia hay entre **miembro de instancia** y **miembro de clase**? ¿Los miembros de clase también se pueden ocultar?

### Respuesta


## 12. Brevemente: ¿Tiene sentido que los constructores sean privados?

### Respuesta


## 13. ¿Cómo se indican los **miembros de clase** en Java? Pon un ejemplo, en la clase `Punto` definida anteriormente, para que incluya miembros de clase que permitan saber cuáles son los valores `x` e `y` máximos que se han establecido en todos los puntos que se hayan creado hasta el momento.

### Respuesta


## 14. Como sería un método factoría dentro de la clase `Punto` para construir un `Punto` a partir de dos coordenadas, pero que las redondee al entero más cercano. Escribe sólo el código del método, no toda la clase ¿Has usado `static`? 

### Respuesta


## 15. Cambia la implementación de `Punto`. En vez de dos `double`, emplea un array interno de dos posiciones, intentando no modificar la interfaz pública de la clase.

### Respuesta


## 16. Si un atributo va a tener un método "getter" y "setter" públicos, ¿no es mejor declararlo público? ¿Cuál es la convención más habitual sobre los atributos, que sean públicos o privados? ¿Tiene esto algo que ver con las "invariantes de clase"?

### Respuesta


## 17. ¿Qué significa que una clase sea **inmutable**? ¿qué es un método modificador? ¿Un método modificador es siempre un "setter"? ¿Tiene ventajas que una clase sea inmutable?

### Respuesta


## 18. ¿Es recomendable incluir métodos "setter" siempre y como convención?

### Respuesta


## 19. ¿La clase `String` en Java es mutable o inmutable? ¿Qué ocurre al concatenar dos cadenas? ¿Qué debemos hacer si vamos a hacer una operación que implique concatenar muchas veces para construir paso a paso una cadena muy larga?

### Respuesta


## 20. En POO ¿Cómo se comparan objetos de una misma clase? ¿Por su contenido o por su identidad? ¿Qué es el método equals en Java? ¿Qué hace por defecto? ¿Cómo se deben comparar dos cadenas en Java? 

### Respuesta


## 21. ¿Qué son las clases "wrapper" en un lenguaje de programación orientado a objetos? ¿Cómo se hace? ¿Es un proceso automático? ¿Qué ventajas tienen? ¿Todos los lenguajes orientados a objetos tienen tipos primitivos y necesitan wrappers? 

### Respuesta


## 22. ¿En POO qué es un **tipo de dato enumerado**? ¿En Java, un tipo de dato enumerado es una clase? ¿Qué ventajas tienen en términos de encapsulación los enumerados en Java?

### Respuesta


## 23. Crea un tipo enumerado en Java que se llame `Mes`, con doce posibles instancias y que además proporcione métodos para obtener cuántos días tiene ese mes, el ordinal de ese mes en el año (1-12), empleando atributos privados y constructores del tipo enumerado. Añade además cuatro métodos para devolver si ese mes tiene algunos días de invierno, primavera, verano u otoño, indicando con un booleano el hemisferio (norte o sur, parámetro `enHemisferioNorte`). Es decir: `esDePrimavera(boolean esHemisferioNorte)`, `esDeVerano(boolean esHemisferioNorte)`, `esDeOtoño(boolean esHemisferioNorte)`, `esDeInvierno(boolean esHemisferioNorte)`

### Respuesta
