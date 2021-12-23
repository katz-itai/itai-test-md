# How To Write Unmaintainable Code

## Ensure a job for life ;-)

#### Code That Masquerades As Comments and Vice Versa

Include sections of code that are commented out but at first glance do not appear to be.

```js
for (j=0; j<array_len; j+=8) {
    total += array[j+0];
    total += array[j+1];
    total += array[j+2]; /* Main body of
    total += array[j+3]; * loop is unrolled
    total += array[j+4]; * for greater speed.
    total += array[j+5]; */
    total += array[j+6];
    total += array[j+7];
}
```

Without the colour coding would you notice that three lines of code are commented out?

#### namespaces

Struct/union and typedef struct/union are different name spaces in C (not in C++). Use the same name in both name spaces for structures or unions. Make them, if possible, nearly compatible.

```c
typedef struct {
    char* pTr;
    size_t lEn;
} snafu;

struct snafu {
    unsigned cNt
    char* pTr;
    size_t lEn;
} A;
```

#### Standards Schmandards

Whenever possible ignore the coding standards currently in use by thousands of developers in your project's target language and environment. For example, insist on STL style coding standards when writing an MFC based application.

#### Reverse the Usual True False Convention

Reverse the usual definitions of true and false. Sounds very obvious but it works great. You can hide:

```c
#define TRUE 0
#define FALSE 1
```

somewhere deep in the code so that it is dredged up from the bowels of the program from some file that no-one ever looks at anymore. Then force the program to do comparisons like:

```c
if ( var == TRUE )
if ( var != FALSE )
```

someone is bound to "correct" the apparent redundancy, and use var elsewhere in the usual way:

```c
if ( var )
```

#### Lisp

LISP is a dream language for the writer of unmaintainable code. Consider these baffling fragments:

```lisp
(lambda (*<8-]= *<8-[= ) (or *<8-]= *<8-[= ))
(defun :-] (<) (= < 2))

(defun !(!)(if(and(funcall(lambda(!)(if(and '(< 0)(< ! 2))1 nil))(1+ !))
(not(null '(lambda(!)(if(< 1 !)t nil)))))1(* !(!(1- !)))))
```

#### Visual Basic Declarations

Instead of:

```vbnet
dim Count_num as string
dim Color_var as string
dim counter as integer
```

use:

```vbnet
Dim Count_num$, Color_var$, counter%
```

#### Visual Basic Madness

If reading from a text file, read 15 characters more than you need to then embed the actual text string like so:

```vbnet
ReadChars = .ReadChars (29,0)
ReadChar = trim(left(mid(ReadChar,len(ReadChar)-15,len(ReadChar)-5),7))
If ReadChars = "alongsentancewithoutanyspaces"
Mid,14,24 = "withoutanys"
and left,5 = "without"
```

#### Visual Foxpro

This one is specific to Visual Foxpro. A variable is undefined and can't be used unless you assign a value to it. This is what happens when you check a variable's type:

```foxpro
lcx = TYPE('somevariable')
```

The value of `lcx` will be `'U'` or `undefined`. BUT if you assign scope to the variable it sort of defines it and makes it a logical `FALSE`. Neat, huh!?

```foxpro
LOCAL lcx
lcx = TYPE('somevariable')
```

The value of lcx is now `'L'` or logical. It further defined the value of `FALSE`. Just imagine the power of this in writing unmaintainable code.

```foxpro
LOCAL lc_one, lc_two, lc_three... , lc_n
IF lc_one
DO some_incredibly_complex_operation_that_will_neverbe_executed WITH
make_sure_to_pass_parameters
ENDIF

IF lc_two
DO some_incredibly_complex_operation_that_will_neverbe_executed WITH
make_sure_to_pass_parameters
ENDIF

PROCEDURE some_incredibly_complex_oper....
* put tons of code here that will never be executed
* why not cut and paste your main procedure!
ENDIF
```

#### A Real Life Example

Here's a real life example written by a master. Let's look at all the different techniques he packed into this single C function.

```c
void* Realocate(void*buf, int os, int ns)
{
    void*temp;
    temp = malloc(os);
    memcpy((void*)temp, (void*)buf, os);
    free(buf);
    buf = malloc(ns);
    memset(buf, 0, ns);
    memcpy((void*)buf, (void*)temp, ns);
    return buf;
}
```

