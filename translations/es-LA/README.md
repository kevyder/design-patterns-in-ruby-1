# Patrones de diseño en Ruby

Resumen de los patrones de diseño explicados en el libro [Design Patterns in Ruby](http://designpatternsinruby.com/), donde [Russ Olsen](http://russolsen.com/)
explica y adapta a Ruby 14 de los 23 patrones de diseño originales de GoF (Gang of Four).

## Patrones de diseño

### Patrones de GoF

* [Adapter](adapter.md): ayuda a dos interfaces incompatibles a trabajar en conjunto
* [Builder](builder.md): crea objetos complejos que son difíciles de configurar
* [Command](command.md): realiza una tarea específica sin tener información del receptor de la petición
* [Composite](composite.md): construye una jerarquía de objetos de árbol e interactúa con todos ellos de la misma manera
* [Decorator](decorator.md): modifica las responsabilidades de un objeto agregando algunas características
* [Factory](factory.md): crea objetos sin tener que especificar la clase exacta del objeto que se creará
* [Interpreter](interpreter.md): proporciona un lenguaje especializado para resolver un problema bien definido en un dominio conocido
* [Iterator](iterator.md): proporciona una manera de acceder a una colección de subobjetos sin exponer la representación por debajo
* [Observer](observer.md): ayuda a construcción de un sistema altamente integrado, sostenible y evita el acoplamiento entre clases
* [Proxy](proxy.md): permite más control sobre cómo y cuándo accedemos a un determinado objeto
* [Singleton](singleton.md): tener una sola instancia de una cierta clase en toda una aplicación
* [Strategy](strategy.md): modifica parte de un algoritmo en tiempo de ejecución
* [Template Method](template_method.md): restablece ciertos pasos de un algoritmo sin cambiar la estructura del algoritmo

### Patrones que no hacen parte de los GOF: Patrones para Ruby

* [Convention Over Configuration](convention_over_configuration.md): construye un sistema extensible sin tener que cargar el peso sobre la configuración
* [Domain-Specific Language](dsl.md): construye una sintaxis conveniente para resolver problemas de un dominio específico
* [Meta-Programming](meta_programming.md): gana más flexibilidad al asignar nuevas clases y crear diseños personalizados en tiempo de ejecución

## Contribuciones

¡Las contribuciones son bienvenidas! ¿Como nos podrías ayudar?:

* Encontrar errores de escritura y gramática
* Proponer una mejor manera de explicar alguno de los estándares
* Añadir ejemplos más claros del uso de un patrón
* Agregar patrones GoF que no estén cubiertos en el libro

Pull Requests de **refactorización de los ejemplos de código no serán considerados**. Los ejemplos proporcionados por Russ Olsen en su libro son simples y autoexplicativos, no buscan tener el mejor rendimiento ni el ser los más elegantes, su propósito es meramente educativo.
