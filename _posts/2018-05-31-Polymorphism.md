---
layout: post
title: The Many Forms of Polymorphism
excerpt_separator: <!--more-->
tags:
- polymorphism
---

Polymorphism.

<!--more-->


![The card Polymorph from the game HearthStone, it morphs an opponent's minion into a sheep](https://i.imgur.com/pUxWK6A.gif "baaaaa")

No, not like that.

It's a big fancy word that means one function can actually do a bunch of different things. In Object Oriented Programming it usually means that multiple objects have the a method with the same name and different behavior. In Functional Programming it has to work a little differently, but the same general concept exists and we'll see an example of it from Clojure here. In _Practical Object-Oriented Design in Ruby_, Sandi Metz defines Polymorphism as
>Polymorphism in OOP refers to the ability of many different objects to respond to the same message. Senders of the message need not care about the class of the receiver; receivers supply their own specific version of the behavior.

While this is a useful description of what Polymorphism is, it's only half of the information we need.

A couple paragraphs later, Metz also says
>Polymorphic methods honor an implicit bargain; they agree to be interchangeable from the sender's point of view. Any object implementing a polymorphic method can be substituted for any other; the sender of the message need not know or care about this substitution.

Although saying that polymorphism is one function name that covers many different behaviors is sufficiently descriptive of what it is, Sandi Metz's making explicit the "implicit bargain" makes the concept actually usable in one's code.

An example of polymorphism that follows the "implicit bargain" would be if you told people to "dance" and a ballerina does ballet but a tap dancer taps. In both cases the dancer is moving and performing, and while these behaviors aren't completely interchangeable, they are close enough that the "sender" doesn't need to care which one is happening. On the other hand, if you tell two people "lie" and one says "The sky is green" and the other gets in a prone position, that's a problem. In this case you have one person essentially returning a string and the other one changing state. These things are not interchangeable and can have effects later on in the program, though I guess technically might still fall under the category of polymorphic since both objects (people) respond to the same message ("lie").

In OOP you can implement polymorphism through inheritance. A Dancer class can have the function dance, and anything that inherits from Dancer can have its own specific implementation of that function.


```ruby
class Dancer
  def dance
    "moves to the music"
  end
end

class Ballerina < Dancer
  def dance
    "plié, jeté"
  end
end

class TapDancer < Dancer
  def dance
    "tappity tap tap"
  end
end

class Performance
  attr_reader :performers

  def innititalize(args)
    @performers = args[:performers]
  end

  def perform
    performers.each {|performer| performer.dance}
  end
end
```

As we can see above, I know nothing about dancing, but also we can initialize Performance with an array of Dancer objects and those objects can be a generic dancer or any of its subclasses because they all have a dance method and the show will go on!

In languages that aren't strongly typed, you can use duck typing for the same thing and not even need inheritance. A butterfly and an airplane probably shouldn't inherit from the same superclass, but they can both fly, and maybe in some point in the code, an object that can fly gets passed in and that's all that matters. You also don't need inheritance in languages that use Interfaces (like Java) and in those languages one doesn't even have to worry about the "implicit bargain" because the return value is made explicit.

In FP, since we don't have objects and classes, things have to work a little differently, but that doesn't mean that polymorphism can't still happen. For example, Clojure has a thing called multi-methods that can switch what happens based on the arguments given. For example, going back to dancers:


```clojure
(defmulti dance (fn [dancer] (:skills dancer)))

  (defmethod dance :ballet [_]
    "plié, jeté")

  (defmethod dance :tap [_]
    "tappity tap tap")

  (defmethod dance :default [_]
    "moves to the music")
```

And now we can call whichever one we want as long as we throw the right arguments in. Clojure also has a thing called protocols that work a lot more like objects. And FP languages that aren't Clojure have their own forms of polymorphism.

Essentially polymorphism is the ability to not care so much, which means that code will be a lot easier to modify when things need to change. This is an essential part of life and writing code, so it's really no wonder that every language I've encountered so far has polymorphism in some form or another.




