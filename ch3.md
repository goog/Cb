# Key points of C



## Uninitialized variables

a storage class defines the scope and life-time of variables annd functions
within a C program.

* auto  
* register  
* static  
* extern  

if a variable is globel or static, it will be zeroed.
if auto , the value is indeterminate

## static

keyword <code>static</code>  
  
Static global variable:  
A variable declared as static at the top level of a source file (outside any function definitions) is only visible throughout that file  
  
Static local variables:  
Variables declared as static inside a function are statically allocated, thus keep their memory cell throughout all program execution, while having the same scope of visibility as automatic local variables (auto and register), meaning remain local to the function. Hence whatever values the function puts into its static local variables during one call will still be present when the function is called again. [if we get its pointer, then we could use the variable as a global variable in other function][1] 

Static function: file scope  

## lvalue and precedence  

lvalue:  
an expression that refers to an object in such a way that the object
may be examined and altered.  

precedence:

Tokens        |  operator            |   class |  precendence | associates  
--------------|----------------------|---------|--------------|-----------  
name,literals |  simple tokens       | primary | 16           | n/a  
a[k]          |  subscripting        | postfix | 16           | left-to-right  
f(...)        |  function call       | postfix | 16           | left-to-right  
.             |  direct select       | postfix | 16           | left-to-right
->            |                      | postfix | 16           | left-to-right
++ --         |                      | postfix | 16           | left-to-right
++ --         |                      | prefix  | 15           | right-to-left
sizeof        |  size                | unary   | 15           | right-to-left
~             |                      | unary   | 15           | right-to-left
!             |                      | unary   | 15           | right-to-left  
-,+           | math negation,plus   | unary   | 15           | right-to-left
&             |  adress of           | unary   | 15           | right-to-left
\*            |  indrection          | unary   | 15           | right-to-left
(type name)   |  casts               | unary   | 14           | right-to-left
*,/,%         |  multiplicative      | binary  | 13           | left-to-right
+,-           |  additive            | binary  | 12           | left-to-right
<< >>         |  shift               | binary  | 11           | left-to-right
<> <= >=      |  relational          | binary  | 10           | left-to-right
== !=         |                      | binary  | 9            | left-to-right
&             | bitwise and          | binary  | 8            | left-to-right
^             |                      | binary  | 7            | left-to-right
\|            |                      | binary  | 6            | left-to-right
&&            | logical and          | binary  | 5            | left-to-right
\|\|          |                      | binary  | 4            | left-to-right
? :           |  conditinal          | ternary | 3            | right-to-left
(*)=          |  assignment          | binary  | 2            | right-to-left
,             | sequential evaluation| binary  | 1            | left-to-right
    


sequence point  


# Functions from libc

# Advanced cases

# Drawbacks of C



[1]: https://en.wikipedia.org/wiki/Static_(keyword) "c static"
