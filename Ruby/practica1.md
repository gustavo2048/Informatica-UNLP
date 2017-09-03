# Ruby: sintaxis y tipos básicos

### 1. Investigá y probá en un intérprete de Ruby cómo crear objetos de los siguientes tipos básicos usando literales y usando el constructor new (cuando sea posible):

#### Arrays

* `array = [12, "hola", 44] => [12, "hola", 44]`
* `array = Array.new(4, :sel) => el, :sel, :sel, :sel]`
* `array = Array.new(4) { Hash.new } => [{}, {}, {}, {}]`
* `Array({:a => "a", :b => "b"}) => [[:a, "a"], [:b, "b"]]`

#### Hashes

* `hash = { "pedro" => 44, "juan" => "hola" } => {"pedro"=>44, "juan"=>"hola"}`
* `hash = { pedro: 44, juan: "hola" } => {:pedro=>44, :juan=>"hola"}`

#### Strings

* `string = "esto es un string" => "esto es un string"`
* `string = 'esto es un string' => "esto es un string"`
* `string = String.new("esto es un string") => "esto es un string"`

#### Symbols

* `symbol = :symbol => :symbol`
* `symbol = :"symbol" => :symbol`
* `symbol = "symbol".to_sym => :symbol`

---

### 2. ¿Qué devuelve la siguiente comparación? ¿Por qué?

`'TTPS Ruby'.object_id == 'TTPS Ruby'.object_id`

Devuelve falso debido a que estamos creando dos Strings que por mas de que tienen el mismo contenido, son instancias distintas, que por ende poseen un object id distinto.

---

### 3. Escribí una función llamada reemplazar que reciba un String y que busque y reemplace en el mismo cualquier ocurrencia de { por do\n y cualquier ocurrencia de } por \nend, de modo que convierta los bloques escritos con llaves por bloques multilínea con do y end.

```
def reemplazar(string)
    string.gsub! "{", "do\n"
    string.gsub! "}", "\nend"	
end
```

---

### 4. Escribí una función que convierta a palabras la hora actual, dividiendo en los siguientes rangos los minutos:

* Si el minuto está entre 0 y 10, debe decir "en punto"
* Si el minuto está entre 11 y 20, debe decir "y cuarto"
* Si el minuto está entre 21 y 34, debe decir "y media"
* Si el minuto está entre 35 y 44, debe decir "menos veinticinco" (de la hora siguiente)
* Si el minuto está entre 45 y 55, debe decir "menos cuarto" (de la hora siguiente)
* Si el minuto está entre 56 y 59, debe decir "casi las" (y la hora siguiente)

```
def en_palabras(time)
    case time.min
        when 0..10
            puts "Son las #{time.hour} en punto"
        when 11..20
            puts "Son las #{time.hour} y cuarto"
        when 21..34
            puts "Son las #{time.hour} y media"
        when 35..44
            puts "Son las #{time.hour + 1} menos vinticinco"
        when 45..55 
            puts "Son las #{time.hour + 1} menos cuarto"
        else
            puts "Casi las #{time.hour + 1}"
    end
end
```

---

### 5. Escribí una función llamada contar que reciba como parámetro dos string y que retorne la cantidad de veces que aparece el segundo string en el primero, sin importar mayúsculas y minúsculas.

```
def contar(string1, string2)
    string1.upcase.count string2.upcase
end
```

---

### 6. Modificá la función anterior para que sólo considere como aparición del segundo string cuando se trate de palabras completas.

```
def contar_palabras universe, target
    universe.scan(/\b#{target}\b/i).size
end
```

---

### 7. Dada una cadena cualquiera, y utilizando los métodos que provee la clase String, realizá las siguientes operaciones sobre el string:

#### Imprimilo con sus caracteres en orden inverso.

`"esto es un string".reverse => rts nu se otse"`

#### Eliminá los espacios en blanco que contenga.

`"esto es un string".delete ' ' => "estoesunstring"`

#### Convertí cada uno de sus caracteres por su correspondiente valor ASCII.

```
"esto es un string".each_byte do |c|
    puts c
end
```

#### Cambiá las vocales por números (a por 4, e por 3, i por 1, o por 0, u por 6).

`puts "esto es un string".gsub /[aeiou]/, /[aA]/ => 4, "e" => 3, "E" => 3, "i" => 1, "I" => 1, "o" => 0, "O" => 0, "u" => 6, "U" => 6`

---

### 8. ¿Qué hace el siguiente código?

```
[:upcase, :downcase, :capitalize, :swapcase].map do | meth|
    "TTPS Ruby".send(meth)
end
```

Este código lo que hace es enviar el mensaje map al arreglo de simbolos, lo cual iterará por todos los simbolos creando una nueva coleccion, enviando por cada elemento del arreglo el mensaje send al string "TTPS Ruby", pasando como parámetro el simbolo por el cual se está iterando.El mensaje send lo que hace es enviar el mensaje que se pasa como parámetro. Por ende la salida en este caso sería:

`=> ["TTPS RUBY", "ttps ruby", "Ttps ruby", "ttps rUBY"]`

---

### 9. Escribí una función que dado un arreglo que contenga varios string cualesquiera, retorne un nuevo arreglo donde cada elemento es la longitud del string que se encuentra en la misma posición del arreglo recibido como parámetro.

```
def longitud array
    array.map { |elem| elem.length }
end
```

---

### 10. Escribí una función llamada `a_ul` que reciba un Hash y retorne un String con los pares de clave/valor del hash formateados en una lista HTML `<ul>`

```
a_ul hash
    "<ul>\
    #{hash.collect {|key, value| "<li>#{key}: #{value}</li>"}.join}\
    </ul>"
end
```

---

### 11. Escribí una función llamada rot13 que encripte un string recibido como parámetro utilizando el algoritmo ROT13.

```
def rot13(string)
    string.tr("a-zA-Z", "n-za-mN-ZA-M")
end
```

---

### 12. Escribí una función más genérica, parecida a la del ejercicio anterior, que reciba como parámetro un string y un número n, y que realice una rotación de n lugares de las letras del string y retorne el resultado.

```
def rot string, num
    range = "n-za-mN-ZA-M"
    alphabet = ('a'..'z').to_a
    first = alphabet[num % alphabet.length]
    last = alphabet[(alphabet.index("z") + num) % alphabet.length]
    range.tr!("nNmM", first << first.upcase + last << last.upcase)
    string.tr("a-zA-Z", range)
end
```

---

### 13. Escribí un script en Ruby que le pida al usuario su nombre y lo utilice para saludarlo imprimiendo en pantalla ¡Hola, "nombre"!.

```
puts "Por favor, ingresa tu nombre: "
name = gets.chomp
puts "¡Hola, #{name}!"
```

---

### 14.Dado un color expresado en notación RGB (https://es.wikipedia.org/wiki/RGB) , debés calcular su representación entera y hexadecimal, donde la notación entera se define como red + green.256 + blue.256.256 y la hexadecimal como el resultado de expresar en hexadecimal el valor de cada color y concatenarlos en orden. 

```
def notacion_hexadecimal array
    array.reduce("#") { |hex, int| hex << int.to_s(16).rjust 2, '0' }
end

def notacion_entera array
    array.each_with_index.reduce(0) { |sum, (value, index)| sum + value * 256 ** index }
end
```

---

### 15. Investigá qué métodos provee Ruby para:

#### Conocer la lista de métodos de una clase.

`Class.methods`

#### Conocer la lista de métodos de instancia de una clase.

`Class.instance_methods`

#### Conocer las variables de instancia de una clase.

`object.instance_variables`

#### Obtener la lista de ancestros (superclases) de una clase.

`Class.ancestors`

---


