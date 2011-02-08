title: Currying and Partial Application
date: 2011/02/07
author: Brian J Brennan

I've been learning Haskell from the fantastic online book [Learn You a Haskell for Great Good!](http://learnyouahaskell.com/) and there's been a concept that's been hard for me to wrap my head around until now.

In Haskell, every function formally only takes one parameter. The syntactic sweetness of Haskell does its best to hide the user from this fact.

Let's define a function:
    
    addThree x y z = x + y + z

The syntax is designed to coax you into believing that this function takes three arguments. Here is how you might write this same method in JavaScript, if you didn't realize the syntactic magic going on:
    
    var addThree = function(x, y, z) { return x + y + z; }  //wrong

This doesn't actually capture what's going on underneath. Here's the real (curried) function:

    var addThree = function(x) {
      return (function(y) {
        return (function(z) {
          return x + y + z;
        })
      })
    }

To end up with an integer value, you call would call the curried version like so: `addThree(5)(8)(13) === 26 //true`.
