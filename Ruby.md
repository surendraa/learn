# Table of contents
- String
-
# String

## How to represent strings
```ruby
s1 = 'does not have escapes, single quoted strings are just literals'
s1 = %q[This is a string]
```

```ruby
s2 = "This can be escaped \n"
s2 = %Q[This is a dbl quoted string \n]
```

```ruby
str = <<EOF
  acts like dbl quoted string
  for big chunk of text
EOF
```

```ruby
str = <<'EOF'
  acts like single quoted string
  for big chunk of text
EOF
```

```ruby
str = <<-EOF
  preserves line indents
  for big chunk of text
EOF
```

## Length of a string
```ruby
'abcdef'.length
```
## Process a line at a time
```ruby
'abc\ndef'.each_line { |l| puts l }
```

## Process character at a time
```ruby
'acbde'.each_char { |c| puts c }
'abcde'.scan(/./).each_char { |c| puts c }
```

## Process byte at a time
```ruby
'acbde'.each_byte { |b| puts b }
```

## Specialized string comparisions
* redefine `<=>` method inside String class
* redefine `==` method in String class

## Tokenize a string
```ruby
s_arr = str.split(/./)
s_arr = str.scan(/w+/)

require 'strscan'
ss = StringScanner.new(str)
ss.bol?; ss.eos?; ss.rest; ss.rest? ; ss.rest_size; ss.pos
ss.scan(/./); ss.captures; ss.get_byte; ss.getch; ss.scan_until; ss.skip; ss.skip_until
ss.check; ss.match?; ss.exist?; ss.peek
ss.reset; ss.terminate; ss.pos = ;
ss.matched; ss.matched? ;ss.pre_match; ss.post_match;
```

## Format a string
```ruby
sprintf("hi %d", 10)
str = "%-20s  %3d" % [name, age]
# %s, %d +d, %x #x, %o, %b, %e, %f .0f #.0f 5.2f;

s.center(20); s.ljust(13); s.rjust(20, "+")
```

## Strings as IO

## Uppercase Lowercase
downcase, upcase, capitalize, swapcase, casecmp

## Accessing and Assignment
```ruby
str[4,7]; str[4,7] = "something"
str[10, -4]
# access by range
# access by regex
# access by substr
```

## Substitution
sub, sub!, gsub, gsub!
* each of these can take a block as well

## Search in a string
index, rindex, include? and scan

## Ascii <-> chars
77.chr
"M".ord

## Implicit / Explicit conversion
to_s ; to_str

## Append to a string
s1 << s2
s1.concat s2

## Trim whitespace
```ruby
"   hello".lstrip
```

## Remove trailing whitespace
s.chomp ; s.chop

## Repeat strings
s * 5

## Interpolation
"hi #{name}"

## Strings <-> Numbers
s.to_i(base) ; Integer("0b111") -> 0b111, 0x111, 0111
str = "234 234 234"
x, y, z = str.scanf(%d, %x, %o)

## Encode / Decode
rot13

## Encryption
"abcd".crypt("asdf")
use bcrypt to store passwords

## Compression
require 'zlib'
Zlib -> Inflate, Deflate

## Counting
s1.count("set of chars like regex")
s1.count("a-d") => a to d

## Reversing a string
s1.reverse

## Removing Dups
s1.squeeze ; s1.squeeze(".")

# Removing characters
s1.delete("b")

## Printing special chars
s1.dump

## Generate successive strings
s1.succ

## 32 bit CRC
zlib
crc = crc32(“Hello”)
crc = crc32(” world!”,crc)

##  SHA-256
require 'digest'
Digest::SHA256.hexdigest(string)
Digest::SHA256.base64digest(string)
Digest::SHA256.digest(string)
* sha256, sha384, sha512, md5

## Base64
require 'base64'
Base64.encode64(str)
Base64.decode64(encoded str)

## Wrapping lines of text
active_support -> word_wrap

# Files
##

# OOP

## Multiple Constructors
* No real constructor in ruby
* `new` on a class calls `initialize`
* we can create multiple methods on the class and call new internally
```ruby
class ColouredRectangle
  def initialize(color, l, b)
  end

  def self.blue_rect(l, b)
    new("blue", l, b)
  end
end

ColouredRectangle.new("grey", 10, 20)
ColouredRectangle.blue_rect(30, 40)
```

## Instance attributes
```ruby
class Person
  def name
    @name
  end

  def name=(str)
    @name = str
  end
end
```
```ruby
class Person
  attr :name #creates an instance var @name, getter name
  attr :age, true #creates an instance var @age, getter age, setter age=
  attr_reader :color #creates only readable/getter attr
  attr_writer :weight #creates only writable/setter attr
  attr_accessor :iq #creates read/write , getter/setter
end
```
```ruby
def update(a1)
  self.a1 = a1
  # within a method the receiver self must be used while calling its own method,
  # or else a local variable will be created by that name instead of calling the method
end
```
```ruby
p.instance_variable_defined?(:age)
p.instance_variable_set("@gender", "male")
p.instance_variable_get("@height")
```

## Elaborate constructors
```ruby
# use blocks in initialize for long list of attr assignments
class Person
  attr_accessor :age, :color, :height
  def initialize
    yield self if block_given?
  end
end
Person.new do |p|
  p.age = 20
  p.color = "white"
end
```
* To assign an attribute at initialization and then make it protected
```ruby
class Person
  attr_reader :color
  def initialize(&block)
    instance_eval(&block)
  end
  protected
  attr_writer :color
end
pe = Person.new { self.color = "brown" }
pe.color = "white" #error
```

## Class attributes and methods
```ruby
class SoundPlayer
  MAX_SAMPLE = 192 # SoundPlayer::MAX_SAMPLE
  def self.detect_hardware # SoundPlayer.detect_hardware
  end
end

def SoundPlayer.count # class level method
end

@@attr # refers to class variable
```

## Inheritence
```ruby
class Student < Person
  # use super to call the parent methods when needed
  # methods and attributes can be overridden as needed
end
```

## Testing class of a method
```ruby
  p.class # Person (an object of class `Class`)
  5.instance_of?(Fixnum)
  5.kind_of? Bignum
  5.is_a? Bignum # is_a? and kind_of? are synonymous
  IO >= File # can use comparator operators to check inheritence chain
  5.respond_to? +
  5.class.superclass
```

## Testing equality of objects
* equal?
  * tests for same object id
  * should not be overwritten in classes
* ==
  * tests for the values
  * types will be coerced
* eql?
  * similar to ==
  * for testing equality between objects of same type
  * no coercing
  * used to compare values of hash keys
  * implemented in Kernel
* ===
  * called the case equality
  * case statements uses it to compare the receiver using selector === target
  * implemented in Module
* =~
  * match operator
  * used for regular expressions
* alternate forms are != and !~

## Controlling access to methods
* private
  * methods cannot be called with a receiver
  * methods can only be accessed by self
  * any method defined outside the class / module are private
* protected
  * less restricted
  * objects of same class/subclass can access the method
* public
  * public by default

## Copying an object
* dup
  * copies only the contents of the objects
  * shallow copy
* clone
  * copies the content and the methods including singleton modifications
  * shallow copy
* deep_copy
  * right way to implement deep_copy
  * a method implemented on all the classes to be deep copied
* marshal
  * Marshal.load(Marshal.dump(obj))

## Initialize copy
```ruby
class Person
  attr_accessor :age
  attr_accessor :created_at
  private :created_at

  def initialize_copy(other)
    @created_at = Time.now
  end
  # initialize_copy is called when dup is called on an object
end
```

## Allocate
p = Person.allocate
* allocate just creates the object without calling its constructor

## Modules
```ruby
module MyMod
  def included(klass)
    def klass.my_class_method
      puts "hi from module"
    end
  end
  def my_instance_method
    puts "hello"
  end
end
class Person
  include MyMod
end
p = Person.new
p.my_class_method
Person.my_class_method
```

## Converting objects
* to_s (general), to_str (expects to be close to a string, only few built-in classes call it)
* to_a , to_ary (same explanation as above)

## Structs (Data only objects)
```ruby
Address = Struct.new("Address", :street, :city, :state)
books = Struct::Address.new("23 main st", "Holmdel", "NJ")
node = Struct.new(:left: nil, :right:  nil)
```

## Freezing objects
obj.freeze
obj.frozen?
* since freeze works on reference, any method that works on reference will still works
* a = 'a'; a.freeze; a += 'b'; # a = 'ab'

## Tap
* used to intermediately do some action on the object and still return the object itself

##[Advanced Techniques]
## Call methods dynamically
```ruby
obj.send(method as symbol)
# circumvents private methods
```

## Singleton / Eigen classes
```ruby
b = '1234'
class << b
end

module Quant
end

b.extend(Quant)
class << b
  extend Quant
end
```

## Parametric classes
```ruby
# does not work
# class variables are shared through the inheritance chain
class ILife
  def ILife.home_planet
    @@home
  end

  def ILife.home_planet=(str)
    @@home = str
  end
end

class Earth << Ilife
  @@home = 'Earth'
end

class Mars << Ilife
  @@home = 'Mars'
end
Eath.home_planet # Mars (wrong)
Mars.home_planet # Mars
```
```ruby
# evaluate class variables per class
class ILife
  def ILife.home_planet
    class_eval("@@home")
  end

  def ILife.home_planet=(str)
    class_eval("@@home = #{str}")
  end
end
class Earth << Ilife
  @@home = 'Earth'
end
class Mars << Ilife
  @@home = 'Mars'
end
Eath.home_planet # Earth (wrong)
Mars.home_planet # Mars
```
```ruby
# create instance variables inside the eigen class of the Class
class Ilife
  class << self
    attr_accessor :home # class instance variable
  end
end
class Earth < Ilife
  self.home = 'Earth'
end
class Mars < ILife
  self.home = 'Mars'
end
Earth.home # Earth
Mars.home # Mars
```

## Proc and Lambda
```ruby
p = Proc.new {|a| puts a}
p.call('123')
# proc returns from the entire methods
l = lambda {|a| puts a }
# lambda returns only from the proc
-> (params) { body } # alternate way of declaring lambda
```

## Storing methods
* use object.method
```ruby
meth = str.method(:length)
meth.call

unmeth = String.instance_method(:length)
m1 = unmeth.bind("cat")
m1.call
m2 = unmeth.bind("somehting")
m2.call
```

## Convert to proc
```ruby
&:length
some_method.to_proc
```