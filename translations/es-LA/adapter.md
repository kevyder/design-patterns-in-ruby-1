# Patrón Adapter

## Problema

Queremos que un objeto converse con otro, pero sus interfaces no son compatibles.

## Solución
Simplemente contenemos la **adaptación** con nuestra nueva clase **adaptadora**. Esta clase implementa una interfaz que el invocador entiende, aunque todo el trabajo sea realizado por el objeto adaptado.

## Ejemplo
Pensemos en una clase que recibe dos archivos (un lector y un escritor) y encripta un archivo.

```ruby
class Encrypter
  def initialize(key)
    @key = key
  end

  def encrypt(reader, writer)
    key_index = 0
    while not reader.eof?
      clear_char = reader.getc
      encrypted_char = clear_char ^ @key[key_index]
      writer.putc(encrypted_char)
      key_index = (key_index + 1) % @key.size
    end
  end
end
```
Pero ¿que ocurre si la información segura que queremos pasa a ser un string, en lugar de un archivo? Necesitamos un objeto que parezca un archivo, es decir, que soporte la misma interfaz que el objeto `IO` de Ruby. Para lograr esto podemos crear una clase adaptadora `StringIOAdapter`:

```ruby
class StringIOAdapter
  def initialize(string)
    @string = string
    @position = 0
  end

  def getc
    if @position >= @string.length
      raise EOFError
    end
    ch = @string[@position]
    @position += 1
    return ch
  end

  def eof?
    return @position >= @string.length
  end
end
```

Ahora podemos utilizar un `String` como su fuera un archivo (solo implementa una pequeña parte de la interfaz `IO`, básicamente lo que necesitamos)

```ruby
encrypter = Encrypter.new('XYZZY')
reader= StringIOAdapter.new('We attack at dawn')
writer=File.open('out.txt', 'w')
encrypter.encrypt(reader, writer)
```
