## Language 8-step plan

1. Types & typing
2. Primitive types/ops, control flow, strings
3. Methods (functions, procedures)
4. Abstraction & encapsulation
5. Idioms
6. Libraries & dependency management
7. Debugging
8. Testing facilities

## Pair Programming



## Overview & Intro to Ruby for Java programmers

![image-20221005105052632](ch2.assets/image-20221005105052632.png)

**Variables, Arrays, Hashes**

```ruby
x = 3; x = 'foo'
x = [1, 'two', :three]
x[1] == 'two'; x.length == 3
w = {'a'=>1, :b=>2}
```

**Methods**

Everything is pass-by-reference

```ruby
def foo(x, y)
  return [x, y + 1]
end

def foo(x, y=0) ; [x, y+1] ; end
```

**Basic Constructs**

![image-20221005141136062](ch2.assets/image-20221005141136062.png)

**Strings & Regular Expressions**

![image-20221005141733350](ch2.assets/image-20221005141733350.png)

## Everything is an object

```ruby
3.days.ago
50.methods
nil.respond_to?(:to_s)
```



## Classes & inheritance

```ruby
class SavingsAccount < Account
	def initialize(balance=0)
    @balance=balance
  end
  def balance
    @balance
  end
  def balance=(new_amount)
    @balance = new_amount
  end
  def deposit(amount)
    @balance += amount
  end
  @@bank_name = "MyBank.com"
  # A class method
  def self.bank_name
    @@bank_name
  end
end
```



## Poetry mode in action

![image-20221005143710292](ch2.assets/image-20221005143710292.png)



## Block

![image-20221005144157962](ch2.assets/image-20221005144157962.png)

![image-20221005144356511](ch2.assets/image-20221005144356511.png)

## Modules



## yield()

![image-20221005152015712](ch2.assets/image-20221005152015712.png)

![image-20221005152604400](ch2.assets/image-20221005152604400.png)

![image-20221006093804209](ch2.assets/image-20221006093804209.png)

## Metaprogramming & Reflection

```ruby
class Numeric
  def euros; self * 1.292; end
end
```

![image-20221006094656160](ch2.assets/image-20221006094656160.png)



## Package Management System 

![image-20221006120112906](ch2.assets/image-20221006120112906.png)
