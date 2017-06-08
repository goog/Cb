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

## Initialized variables
**data segment**
```
char *p = "of human bondage";
int main()
{
}
```
this makes string literal to be stored in initialized read-only area and the character pointer variable in initialized area.

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
## Pointer

```c
#include <stdio.h>

//array of pointers
char *names[] =
{
"tom",
"cat",
"eac"
};


// array of function pointers
int (*idle_state[3])();

int main()
{
    int i;

    for(i=0;i<3;i++)
    {
        printf("name%d is %s.\n", i + 1, names[i]);
    }

    char test[] = "06077";
    const char *a = test; //pointer to constant: usual use, restrict the modification to string buffer
    char const *b = test; // same to above

    //only the c is a constant pointer becase const is nearby c
    char * const c = test;
    c = &test + 1; // error demo
}
```

## sizeof

```c
#include <stdio.h>


#define my_sizeof(type) (char *)(&type + 1) - (char *)(&type)

typedef struct foo
{
int a;
char b[4];
} bar;

int main()
{
    int array[100];
    bar bbb;

    printf("size of the array is %ld \n", my_sizeof(array));
    printf("size of the structure is %ld \n", my_sizeof(bbb));

    //printf("size of the int is %ld \n", my_sizeof(int));
}
```

# Functions from libc

## String

asprintf, strtok_r, getline  

```c
#include <stdio.h>
#include <assert.h>

#undef strlen


size_t strlen(const char *str)
{
    const char *s;

    for(s=str;*s != '\0';s++) ;

    return (size_t)(s - str);
}


char *strcat(char *dest, const char *src)
{
    char *rdest = dest;

    for(;*dest;dest++) ;

    while(*dest++ = *src++);

    return rdest;
}


char *strcpy(char *dest, const char *src)
{
    assert(dest && src);

    char *rdest = dest;

    while(*dest++ = *src++);

    return rdest;
}


int strcmp(const char *s1, const char *s2)
{
    while(*s1)
    {
        if(*s1 != *s2)
        {
            return *(unsigned char *)s1 - *(unsigned char *)s2;
        }
        else
        {
            s1++, s2++;
        }
    }

    return *(unsigned char *)s1 - *(unsigned char *)s2;

}

```

# Advanced cases

## function return type
return type defaults to ‘int’  
```c
#include <stdio.h>
#include <limits.h>

foo()
{
    return UINT_MAX;
}

int main()
{
    unsigned long int a = foo();
    printf("a %ld.\n", a);

}

```
# Drawbacks of C



[1]: https://en.wikipedia.org/wiki/Static_(keyword) "c static"
