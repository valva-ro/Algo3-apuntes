# Índice

[TOC]

# Design Principles Behind Smalltalk (Ingalls)

* **Dominio personal:** si el sistema es para servir al espíritu creativo debe ser completamente entendible para un individuo solitario
* **Buen diseño:** un sistema debería ser construido con un mínimo conjunto de partes no modificables que deberían ser tan generales como sea posible y estar mantenidas en un esquema uniforme
* **Propósito:** proveer un esquema para la comunicación
* **Alcance:** el diseño de un lenguaje para usar computadoras debe tratar con modelos internos, medios externos, y con la interacción entre ellos tanto en el humano como en la computadora.
* **Objetos:** un lenguaje debe soportar el concepto de objeto y proveer una manera uniforme de referirse a los objetos de universo
* **Administracion del almacenamiento:** para ser auténticamente orientado a objetos un sistema debe proveer administración automática del almacenamiento
* **Mensajes:** la computación debería ser vista como una capacidad intrínseca de los objetos que pueden ser invocados uniformemente enviándoles mensajes
* **Modularidad:** ningún componente en un sistema complejo debería depender de los detalles internos de ningún otro componente
* **Clasificación:** un lenguaje debe proveer un medio para clasificar objetos similares, y para agregar nuevas clases de objetos en pie de igualdad con las clases centrales del sistema
* **Polimorfismo:** un programa solo debería especificar el comportamiento esperado de los objetos, no su representación
* **Factorización:** cada componente independiente de un sistema sólo debería aparecer en un sólo lugar. Esto ahorra tiempo, esfuerzo y espacio si los agregados al sistema sólo necesitan hacerse en un lugar. Además los usuarios pueden encontrar más fácilmente un componente que satisfaga una necesidad.
* **Reaprovechamiento:** cuando un sistema está bien factorizado, un gran reaprovechamiento esa disponible tanto para los usuarios como para los implementadores
* **Máquina virtual:** una especificación de máquina virtual establece un marco para la aplicación de tecnología. Smalltalk establece un modelo orientado a objetos para el almacenamiento, un modelo orientado a mensajes para el procesamiento y un modelo de bitmap para el despliegue visual de información.
* **Sistema operativo:** un SO es una colección de cosas que no encajan dentro de un lenguaje, no debería existir. Smalltalk no tiene un "SO" propiamente dicho, las operaciones primitivas necesarias son incorporadas como métodos primitivos en respuesta a mensajes Smalltalk normales.
* **Selección natural:** los lenguajes y sistemas que son de buen diseño persistirán sólo para ser reemplazados por otros mejores

# Unit Testing Guidelines

1. Cortos y rápidos
2. Totalmente autimatizados y no-interactivos
3. Sencillos de correr
4.  Medir cobertura
5. Arreglar inmediatamente tests que no pasen
6. Mantenerlos a nivel unitario
7. Empezar con algo simple, es mejor un test simple que ningún test.
8. Los tests deben ser independientes para que sean fáciles de mantener
9. Los tests deberían estar _cerca_ de las clases que testean. Si la clase se llama Clase, el test debería llamarse ClaseTest y estar en el mismo directorio que Clase
10. Elegir los nombres de los tests adecuadamente
11. Testear a través de los APIs públicos
12. Pensamiento "caja-negra": los tests tienen que comprobar si la clase cumple los requisitos
13. Pensamiento "caja-blanca": como el programador también escribió la clase siendo testeada, debería testear también las cosas de lógica más compleja
14. Testear casos triviales
15. Cubrir casos border
16. Proveer un generador random
17. Testear cada característica una única vez
18. Usar asserts explícitos, es preferible `assertEquals(a, b)` que `assertTrue( a == b )` 
19. Proveer tests "negativos" (para las excepciones)
20. Diseñar el código con los tests en mente
21. No vincular los tests con fuentes externas, los tests deberían escribirse sin explicitar el entorno/contexto en el que se ejecutarán. De esta forma se puede correrlos en cualquier lugar y momento
22. Balancear el costo de testear, en general el porcentaje de cobertura de los tests es del 80%
23. Preparar los tests para que no se interrumpa el flujo del programa si el código falla
24. Escribir código para reproducir bugs
25. Conocer las limitaciones, cuando un test unitario falla hay un error en el código, pero cuando pasa no prueba nada

# What's the point of TDD

Hay dos bases importantes a tener en cuenta cuando queremos crear un sistema confiable y robusto. La primera es testear constantemente para atrapar los errores de "regresión", de esta forma si agregamos una nueva característica al programa con los tests tenemos cómo verificar si lo que habíamos hecho antes sigue funcionando o no.

La segunda es mantener el código lo más simple posible para que sea fácil de entender y modificar. Hay que pensar que como desarrolladores pasamos mucho más tiempo leyendo código que escribiendolo así que es algo que tendríamos que optimizar.

TDD consiste en escribir primero los tests y después el código y transforma el testeo en una actividad de diseño/modelado. Usamos los tests para clarificar las ideas que tenemos sobre el *qué* queremos que haga el código. El ciclo fundamental de TDD sería:

![Ciclo-TDD](https://i.loli.net/2020/11/30/u5lvChrsLwcfZx1.png)

Donde el ciclo más grande nos sirve para medir el progreso de todo el proyecto, y el más chico para mejorar nuestro código.

Los pasos un poco más detallados:

1. Escribir el test (que no va a compilar)
2. Escribir el código mínimo y necesario para que compile
3. Correr el test para verificar que falla
4. Agregar el código mínimo posible para que el test pase
5. Correr el test para verificar que pase
6. Refactorizar (mantener la firma y funcionalidad mejorando el código para que quede más cheto)
7. Agregar más pruebas

Es importante que al refactorizar se modifique únicamente la estructura interna de un bloque de código existente sin cambiar su comportamiento. La refactorización tiene que mejorar el código para que sea más mantenible en el tiempo.

Hay varios niveles del testeo que se corresponden con la imagen anterior:

1. Aceptación: el sistema completo funciona?
2. Integración: nuestro código funciona frente a código que no podemos cambiar?
3. Unitario: nuestros objetos hacen lo que tienen que hacer? Son convenientes?

*Nota: la terminología utilizada para referirse a los tests de aceptación varía en las diferentes bibliografías, y a veces se los suele llamar tests funcionales, tests de clientes, tests de sistema*

## Acoplamiento y cohesión

Son métricas que nos ayudan a describir que tan fácil o difícil va a ser cambiar el comportamiento de cierto código.

Los elementos están **acoplados** si hacer un cambio en uno implica cambiar otro. Por ejemplo si tenemos dos clases que heredan de una clase madre y modificamos alguna, es probable que tengamos que cambiar algo en la otra. Cuanto menos acoplado este nuestro modelo, mejor.

La **cohesión** de un elemento mide si sus responsabilidades forman una unidad que tiene coherencia. Por ejemplo si tuvieramos una máquina que lava ropa y platos, es extremadamente probable que no termine de hacer ninguna bien, en este caso diríamos que no es coherente porque no representa un concepto en sí mismo. Cuanto más coherente sea nuestro modelo, mejor.

# 8 Principles of Better Unit Testing (Dror Helper)

¿Qué hace bueno a un test unitario? Tiene que ser corto, rápido y automatizado y debe garantizar que una parte *específica* del programa funcione. Sólo debe fallar cuando aparece un nuevo bug en el programa y cuando eso sucede tiene que ser fácil entender qué falló.

Los 8 principios son:

1. **Saber lo que estas testeando:** cuando hacés un test sin tener claro el objetivo en mente es muy fácil que quede algo largo, complicado y difícil de entender.
2. **Debe ser autosuficiente:** tiene que estar aislado, evitar dependencias.
3. **Debe ser consistente:** el test no puede pasar a veces sí y a veces no, o pasa siempre o falla siempre (hasta que se arregle el bug). Para esto se recomienda (entre otras cosas) no utilizar valores random (porque puede fallar sin que sepamos por qué).
4. **Convención para los nombres:** para saber por qué falló un test tenemos que ser capaces de entender lo que se testea simplemente con mirarlo. Como lo primero que vemos es el nombre del test, es importante que sea lo más descriptivo y preciso posible.
5. **Repetir:** en general cuando producimos código está **mal** copiar-pegar código, pero a la hora de testearlo la cosa cambia porque es muy importante la legibilidad. Entonces es preferible tener 5 tests similares pero fáciles de leer a uno que no tiene nada duplicado pero que cuando falla no se entiende por qué.
6. **Testear resultados, no implementación:** como somos nosotros quienes programamos el código y las pruebas, puede pasar que muchas veces nos pongamos a testear cosas relacionadas a la implementación y no al resultado esperado. Algo relacionado con esto es testear métodos privados: tenemos que tener una muuuy buena razón para hacer esto, recordemos que los métodos privados *no están hechos para ser visibles desde afuera* por lo que no deberíamos testearlos.
7. **Evitar la sobre-especificación:**  a veces es tentador crear tests estrictos, bien definidos y controlados que testeen el flujo exacto del proceso. El probelma es que esto nos "bloquea" el escenario que se está testeando y en el futuro una modificación podría afectar el resultado de un test tan estricto. Deberíamos evitar, por ejemplo, escribir un test que espera llamar a un método exactamente tres veces.
8. **Usar algún framework de aislación (o mocking):** escribir buenos tests es difícil cuando una clase tiene muchas dependencias. Para evitar esto tenemos que mockear.

# The Art of Enbugging (Hunt & Thomas)

Los comunmente conocidos como "bugs" son errores que nosotros mismos, programando, metimos en nuestro código. Es probable que en un mal día hasta sintamos que estamos enbuggeando (llenando de bugs) en vez de programando. Desde ya que no es nuestra intención hacer esto, y una de las mejores formas de evitarlo es mantener una separacion de responsabilidades pertinente. esto es: modelar el código de manera tal que las clases y módulos tengan responsabilidades aisladas y bien definidas (sobra aclarar que esto es mucho más fácil en la teoría que en la práctica).

Una de las cosas que nos puede ayudar a llevar a la práctica esto es aplicar la ley de demeter para funciones. La idea es que cuantas más relaciones entre objetos tengamos más probabilidades hay de que se rompa algo cuando un objeto cambie (acoplamiento). Esta "ley" nos sugiere que un objeto sólo debería llamar a:

* Sí mismo
* Alguno de los parámetros que recibió el método
* Algún objeto que haya creado
* Algún componente directamente relacionado con un objeto

# Getter Eradicator (Fowler)

La justificación general para sacar los getters es que violan el encapsulamiento, pero es importante entender que muchas veces vamos a tener objetos que van a necesitar colaborar con otros y ahí hay una necesidad genuina de getters.

Si lo que buscamos es una regla simple podemos tener presente lo que comentó Kent Beck: revisar las partes del código donde se invoque más de un método del mismo objeto. Otro tip es revisar las clases anémicas (que sólo tengan getters y setters pero ningún comportamiento).

# Replace Conditional With Polymorphism

**Problema:** tenés un condicional que realiza ciertas acciones dependiendo de las propiedades que tenga un objeto

**Solución:** crear subclases que se relacionen con las ramas del condicional. En ellas crear un método compartido (polimorfismo) y mover el código que corresponda de la rama del condicional a ese método. 

Si por ejemplo tenemos esto:

```java
class Bird {
  // ...
  double getSpeed() {
    switch (type) {
      case EUROPEAN:
        return getBaseSpeed();
      case AFRICAN:
        return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
      case NORWEGIAN_BLUE:
        return (isNailed) ? 0 : getBaseSpeed(voltage);
    }
    throw new RuntimeException("Should be unreachable");
  }
}
```

Lo cambiaríamos a esto:

```java
abstract class Bird {
  // ...
  abstract double getSpeed();
}

class European extends Bird {
  double getSpeed() {
    return getBaseSpeed();
  }
}
class African extends Bird {
  double getSpeed() {
    return getBaseSpeed() - getLoadFactor() * numberOfCoconuts;
  }
}
class NorwegianBlue extends Bird {
  double getSpeed() {
    return (isNailed) ? 0 : getBaseSpeed(voltage);
  }
}

// Somewhere in client code
speed = bird.getSpeed();
```



Ahora bien ¿por qué hacer esta refactorización? Porque en vez de preguntarle al objeto su estado para luego hacer algo, simplemente le decimos al objeto lo que tiene que hacer para que decida por sí mismo cómo hacerlo. Además evitamos código duplicado (varios `if/else` similares) y si en el futuro tenemos que agregar un caso nuevo, es tan sencillo como crear una nueva clase que tenga ese método con el comportamiento que querramos.

# What's a Model For (Fowler)

Es importante reconocer que crear un diagrama UML tiene un costo y es algo que no es fundamentalmente importante para un cliente. Al cliente no le importa saber cómo funciona el software o que tan lindos sean los diagramas, sólo quieren un software que funcione. Sin embargo el valor del modelado esta ligado al efecto que tiene en el software que producimos. Si el modelo incrementa la calidad de nuestro software entonces tiene un valor. El modelo por sí mismo no tiene valor alguno.

Cuando tenemos un modelo súper detallado el costo es mayor, y debemos preguntarnos qué tanto ganamos agregando todos los detalles. A la hora de hacer un diagrama de clase Fowler cree que es clave resaltar los detalles realmente importantes porque no todos los problemas son igual de relevantes. Plantea que en general un modelo esquelético es mejor que un fully-fleshed porque en este último hay demasiada información que revisar. Otra ventaja que menciona de los skeletal es la facilidad para mantenerlos porque los detalles importantes suelen cambiar con menos frecuencia.

# Pruebas de Software (Fontela)

## Qué probamos

La **verificación** tiene que ver con controlar que hayamos construido el producto tal como pretendíamos. La **validación** por otro lado controla que hayamos construido el producto que nuestro cliente quería.

Supongamos que un cliente nos pide que implementemos un juego de TaTeTi. Nosotros nos ponemos a diseñar, codificar y probar nuestro programa y funciona todo bárbaro, pero cuando se lo presentamos el cliente nos dice que no es lo que pidió. Él quería un TaTeTi para dos jugadores online, y nosotros hicimos uno para jugar contra la máquina.

Acá hay dos problemas: la expectativa no cubierta del cliente (juego online) y que el sistema provee una característica que el cliente ni deseaba ni está dispuesto a pagar (la inteligencia que le permite al usuario jugar contra la máquina).

En este caso la verificación fue exitosa, porque el software funcionaba como nosotros pretendíamos, pero falló la validación porque lo que hicimos no era lo que quería el usuario.

## Alcance de las pruebas

### Pruebas de verificación (técnicas)

Las pruebas **unitarias** verifican pequeñas porciones de código.

Las pruebas de **integración** verifican que varias porciones del código, trabajando en conjunto, hagan lo que pretendíamos.

Las pruebas pueden ser de **caja negra o blanca**. Son de caja negra cuando la ejecutamos sin ver el código que estamos probando, y de caja blanca cuando analizamos el código durante la prueba. En general son preferibles las pruebas de caja negra porque precisamente se desea probar el funcionamiento y no verificar la calidad del código. Se recurre a la técnica de caja blanca cuando algún error se resiste a ser encontrado y tenemos que examinar el código en detalle (el debuggin es una técnica de caja blanca).

### Pruebas de validación (clientes)

Cuando un sistema debe salir a producción y tiene cierta criticidad, se suele poner a disposición de usuarios reales para que ellos mismos ejecuten las pruebas. Si las pruebas las hacemos en un entorno controlado por el equipo de desarrollo, las llamamos **pruebas alfa**. Si, en cambio, el producto se deja a disposición del cliente para que lo pruebe en su entorno, las llamamos **pruebas beta**. En ambas se ejecuta el sistema completo con su interfaz, medios de almacenamiento, conexiones, etc.

Otro tipo de pruebas son las de **comportamiento** que no involucran la interfaz de usuario.

### Pirámide de pruebas

La figura a continuación nos muestra que haya distintos niveles de prueba:

![image-20201130020840666](https://i.loli.net/2020/11/30/yUjYduK3tILvcwO.png)



Las que más deben abundar son las unitarias, y las que menos las de acptación a través de la interfaz de usuario. La razón de la pirámide es económica: cuanto más abajo en la pirámide más rápido se ejecutan las pruebas. Por otro lado las pruebas de aceptación son más frágiles porque dependen de la interfaz de usuario que suele ser cambiante, y a mayor fragilidad mayor tiempo gastado en mantenimiento de las pruebas. Cuanto más bajamos, más disminuye la fragilidad (porque hay menos acoplamiento).

## Roles del desarrollo de las pruebas

Las pruebas unitarias y de integración técnica son responsabilidad del programador.

Las pruebas de comportamiento están a mitad de camino entre los programadores y los testers, así que cualquiera puede desarrollarlas.

Las UAT deberían ser escritas por los usuarios o analistas de negocio, en principio, peri si esto no se consiguiera las podrían escribir los testers.

*Nota: no siempre se distingue el rol de programador y el de tester*

## Pruebas y desarrollo

### Ventajas de automatizar las pruebas

1. Nos independizamos del factor humano (y su subjetividad)
2. Es más fácil repetir las mismas preubas, con un costo ínfimo comparado con las pruebas realizadas por una persona
3. Las pruebas en código (bien hechas) sirven como erramienta de comunicación minimizando las ambiguedades

### Tipos de pruebas y automatización

![image-20201130173922570](https://i.loli.net/2020/12/01/SDXtB4iIO9m2bxl.png)

### TDD

Fue la primera práctica basada en la automatización de pruebas, presentada por Kent Beck en el marco de Extreme Programming.

![image-20201130174128025](https://i.loli.net/2020/12/01/tvDMKoz2wcg6bOH.png)

## Cobertura

La cobertura es el grado en que los casos de pruebas de un programa llegan a recorrer dicho programa al ejecutar las pruebas. Se usan como una (de varias) medida de la calidad de las pruebas: a mayor cobertura las pruebas del programa son más exhaustivas y por lo tanto exiten menos situaciones que no están siendo probadas.

Algo importante es no confundir la cobertura de las pruebas con la calidad del programa. Aun teniendo una cobertura del 100% no podemos garantizar que estemos analizando todos los casos posibles. Por lo tanto el nivel de cobertura no debe guiar el desarrollo.

Analizando los costos y beneficios de tener una cobertura del 100% se llega a que un buen objetivo es tener entre un 80%-90%.

# Referencias

1. "**[Design Principles Behind Smalltalk](https://www.cs.virginia.edu/~evans/cs655/readings/smalltalk.html)**", [Dan Ingalls](https://en.wikipedia.org/wiki/Dan_Ingalls)
2. "**[Unit Testing Guidelines](https://petroware.no/unittesting.html)**", Petroware SA
3. [**Chapter 1 - What Is the Point of Test-Driven Development?**](https://github.com/gg-daddy/ebooks/blob/master/Growing Object-Oriented Software%2C Guided by Tests.pdf)[1], Addison-Wesley - 2011
4. "**[8 Principles of Better Unit Testing](http://esj.com/Articles/2012/09/24/Better-Unit-Testing.aspx?p=1)**", Dror Helper
5. "**[The Art of Enbugging](https://media.pragprog.com/articles/jan_03_enbug.pdf)**", [Andy Hunt](https://en.wikipedia.org/wiki/Andy_Hunt_(author)) & [Dave Thomas](https://en.wikipedia.org/wiki/Dave_Thomas_(programmer))
6. "**[GetterEradicator](https://martinfowler.com/bliki/GetterEradicator.html)**", [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler)
7. "**[Replace Conditional With Polymorphism](https://sourcemaking.com/refactoring/replace-conditional-with-polymorphism)**", [SourceMaking](https://sourcemaking.com/)
8. "**[What's a Model For?](http://martinfowler.com/distributedComputing/purpose.pdf)**", [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler) 
9. "**[Pruebas de software](https://campus.fi.uba.ar/pluginfile.php/230628/mod_page/content/10/Pruebas2018.pdf)**", [Carlos Fontela](https://limetfiuba.github.io/carlosfontela/) 
10. "**[Continuous Integration](https://www.martinfowler.com/articles/continuousIntegration.html)**", [Martin Fowler](https://en.wikipedia.org/wiki/Martin_Fowler) 
11. "**[Estado del arte y tendencias en Test-Driven Development](http://sedici.unlp.edu.ar/bitstream/handle/10915/4216/Documento_completo.pdf?sequence=1&isAllowed=y)**", [Carlos Fontela](https://limetfiuba.github.io/carlosfontela/) 
12. "**[Using Java Reflection](http://www.oracle.com/technetwork/articles/java/javareflection-1536171.html)**", Glen McCluskey