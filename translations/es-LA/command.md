# Patrón Command

## Problema

Queremos ejecutar una tarea específica sin saber cómo funciona el proceso o tener información sobre el receptor de la petición.

## Solución

El patrón Command desacopla el objeto que necesita ejecutar una tarea específica del objeto que sabe cómo hacerlo. Se encapsula toda la información necesaria para hacer el trabajo en su propio objeto, incluyendo: quiénes son los destinatarios, los métodos que se llamarán y sus parámetros. De esta manera, cualquier objeto que desea realizar la tarea sólo necesita conocer la interfaz del objeto de comando.

## Ejemplo

Consideremos la implementación de un botón en un framework de interfaz gráfica (GUI), el cual tiene un método que será llamado al clicar sobre el botón.

```ruby
class SlickButton

  # Lots of button drawing and management
  # code omitted...

  def on_button_push
    # Do something when the button is pushed
  end
end
```

Podríamos extender la clase del botón sobreescribiendo el método `on_button_push` para ejecutar ciertas acciones cada vez que un usuario haga clic sobre él, Por ejemplo, si el propósito del botón es guardar un documento, podríamos hacer algo como esto:

```ruby
class SaveButton < SlickButton
  def on_button_push
    # Save the current document...
  end
end
```

Sin embargo, una interfaz gráfica de usuario compleja podría tener cientos de botones, lo que significa que terminaríamos teniendo varios cientos de subclases de nuestro botón. Hay una manera más fácil. Podemos refactorizar el código que ejecuta la acción en su propio objeto, el cual implementa una interfaz simple. Entonces, podemos refactorizar la implementación de nuestro botón para recibir el objeto de comando como un parámetro y llamarlo cuando se hace clic.

```ruby
class SaveCommand
  def execute
    # Save the current document...
  end
end

class SlickButton
  attr_accessor :command

  def initialize(command)
    @command = command
  end

  def on_button_push
    @command.execute if @command
  end
end

save_button = SlickButton.new(SaveCommand.new)
```

El patrón de Comando es bastante útil si necesitamos implementar la función de **deshacer**. Todo lo que necesitamos hacer es implementar el método `unexecute` en nuestro objeto de comando. Por ejemplo, así es como implementaríamos la creación de un archivo:

```ruby
class CreateFile < Command
  def initialize(path, contents)
    super "Create file: #{path}"
    @path = path
    @contents = contents
  end

  def execute
    f = File.open(@path, "w")
    f.write(@contents)
    f.close
  end

  def unexecute
    File.delete(@path)
  end
end
```

Otra situación en la que el patrón de comando es realmente útil es en los programas de instalación. Combinándolo con el patrón **composite**, podemos almacenar una lista de tareas a realizar:

```ruby
class CompositeCommand < Command
  def initialize
    @commands = []
  end

  def add_command(cmd)
    @commands << cmd
  end

  def execute
    @commands.each {|cmd| cmd.execute}
  end
end

cmds = CompositeCommand.new
cmds.add_command(CreateFile.new('file1.txt', "hello world\n"))
cmds.add_command(CopyFile.new('file1.txt', 'file2.txt'))
cmds.add_command(DeleteFile.new('file1.txt'))
```
