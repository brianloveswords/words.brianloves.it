title: Currying and Partial Application
date: 2011/02/07
author: Brian J Brennan
tags: javascript haskell currying
draft: true

I've been learning Haskell from the fantastic online book [Learn You a Haskell for Great Good!](http://learnyouahaskell.com/) and there's been a concept that I'm just starting to really wrap my head around.

In Haskell, every function formally only takes one parameter. The syntactic sweetness of Haskell does its best to hide the user from this fact.

Let's define a function:
    
    addThree x y z = x + y + z

<!--
The syntax is designed to coax you into believing that this function takes three arguments. If you checkout the type declaration of the function, there's a hint there:

    addThree :: (Num a) => a -> a -> a -> a 

The book probably does a better job of explaining this than I could, but I'll give it a shot. The `(Num a) =>` part is saying “Any `a` that follows is of the type `Num`”. This part is technically irrelevant, for what I'm explaining, but it's good to know why it's there.

After that there comes a chain of `a -> a`, which is actually Haskell sugar. It should really read

    addThree :: (Num a) => a -> (a -> ( a -> a) )

-->
## Better understanding through JavaScript

Here is how you might write this same method in JavaScript, if you didn't realize the syntactic magic going on:
    
    var addThree = function(x, y, z) { return x + y + z }  //wrong

This doesn't actually capture what's going on underneath. Here's the actual equivalent function:

    var addThree = function(x) {
      return (function(y) {
        return (function(z) {
          return x + y + z 
        })
      })
    }

To end up with an integer value, you call would call the curried version like so:

    addThree(5)(8)(13) == 26 //true

But you could have a partial application of addThree which returns a function:

    var addSixteenTo = addThree(8)(8) //one function left in chain
    addSixteenTo(112) == 128 //true

Let's do another example:

    var plus = function(x) { return (function(y) { return x + y  } ) }
    
    // now we can do things like:
    var inc = plus(1),
        dec = plus(-1)
    
    [1,2,3,4,5].map(inc) // [2,3,4,5,6]




    
