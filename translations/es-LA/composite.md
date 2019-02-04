# Patrón Composite

## Problema

Necesitamos construir una jerarquía de objetos de árbol y queremos interactuar con todos de la misma manera, sin importar si se trata de un objeto hoja o no.

## Solución

Hay tres clases principales en el patrón Composite: el **component**, la **hoja** y las clases **compuestas**. El **componente** es la clase base y define una interfaz común para todos los componentes. La **hoja** es un bloque de construcción indivisible del proceso. El **compuesto** es un componente de nivel superior creado a partir de subcomponentes, por lo que cumple una doble función: es un componente y una colección de componentes. Como las clases compuestas y de hoja implementan la misma interfaz, pueden ser usadas de la misma manera.

## Ejemplo

Se nos ha pedido que construyamos un sistema que rastree la fabricación de pasteles, con el requisito de poder saber cuánto tiempo se tarda en hornearlo. Hacer un pastel es un proceso complicado, ya que involucra múltiples tareas que pueden estar compuestas por diferentes subtareas. Todo el proceso podría estar representado en el siguiente árbol:

```
|__ Manufacture Cake
    |__ Make Cake
    |   |__ Make Batter
    |   |   |__ Add Dry Ingredients
    |   |   |__ Add Liquids
    |   |   |__ Mix
    |   |__ Fill Pan
    |   |__ Bake
    |   |__ Frost
    |
    |__ Package Cake
        |__ Box
        |__ Label
```

En el patrón Composite, modelaremos cada paso en una clase separada con una interfaz común que mostrará el tiempo que tardará. Así que definiremos una clase base común, `Task`, que desempeña el papel de **componente**.

```ruby
class Task
  attr_accessor :name, :parent

  def initialize(name)
    @name = name
    @parent = nil
  end

  def get_time_required
    0.0
  end
end
```

Ahora podemos crear las clases a cargo de las tareas más basicas (clases **hoja**) como `AddDryIngredientsTask`:

```ruby
class AddDryIngredientsTask < Task
  def initialize
    super('Add dry ingredients')
  end

  def get_time_required
    1.0
  end
end
```

Ahora necesitamos un contenedor que se encargue de tareas complejas que crean internamente de según el número de subtareas, pero que se parecen a cualquier otro objecto `Task` desde afuera. Crearemos la clase **compuesta**:

```ruby
class CompositeTask < Task
  def initialize(name)
    super(name)
    @sub_tasks = []
  end

  def add_sub_task(task)
    @sub_tasks << task
    task.parent = self
  end

  def remove_sub_task(task)
    @sub_tasks.delete(task)
    task.parent = nil
  end

  def get_time_required
    @sub_tasks.inject(0.0) {|time, task| time += task.get_time_required}
  end
end
```

Con esta clase base podemos crear tareas complejas que se comportan como una simple (porque implementa la interfaz de `Task`) y también agregar subtareas con el método `add_sub_task`. Vamos a crear la `MakeBatterTask`

```ruby
class MakeBatterTask < CompositeTask
  def initialize
    super('Make batter')
    add_sub_task(AddDryIngredientsTask.new)
    add_sub_task(AddLiquidsTask.new)
    add_sub_task(MixTask.new)
  end
end
```

Debemos tener en cuenta que el árbol de objetos puede ir tan profundo como queramos. `MakeBatterTask` solo contiene objetos de hoja, pero podríamos crear una clase que contenga objetos compuestos y se comportaría exactamente igual:

```ruby
class MakeCakeTask < CompositeTask
  def initialize
    super('Make cake')
    add_sub_task(MakeBatterTask.new)
    add_sub_task(FillPanTask.new)
    add_sub_task(BakeTask.new)
    add_sub_task(FrostTask.new)
    add_sub_task(LickSpoonTask.new)
  end
end
```
