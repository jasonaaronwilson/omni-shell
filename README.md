# Omni Shell

Omni shell is my design for a command line shell that is easier to program than bash while retaining a the simplicity of being suitable as a "command repl". It's meant to fill the ground between bash and languages like Ruby, Python, and Perl.

## Syntax

Tokens are either delimited by white-space or certain delimiters.

The pound sign begins a comment that continues to the end of the document.

Commands fit on one line unless continued with \ at the very end of the line.

## Simple Commands

The most basic thing a shell can do is launch external commands/programs:

```
   <program-name> <program-arguments>*
   ls
   ls -1
```

## Redirection

Probably the same syntax as bash. >, <, and >> to redirect a file or file handle.

## Pipelines

Naturally | is still used to create pipe-lines of commands.

```
   ls -1 | xargs head
```

## Variable Substitution

Variable substitution is the familar syntax ${var-name} or $var-name. This will look up a lexical variable and possibly turn it into a string if it isn't one already. If a lexical variable isn't found with that name than we use the environment variables.

## Special Variables

* $*
* $ARGV
* $COMMAND
* $TRUE
* $FALSE

## Variable Assignment

Variable assignment is accomplished with set (or setenv).

```
   set name <expression>
   setenv name <expression>
```

## Function Definitions

Functions can have named arguments. $* refers to a list of "left-over" arguments.

```
   function name (a b c) {
   }
```

To return one or more a value, use a return statement:

```
   return x
```

## Lambda Expressions

```
   lambda (x y z) {
   }
```

For a lambda to return a value, you need to use return.

## Control Flow

```
   if (<expression>) {
      <statement/command>
      ...
   } else {
      <statement/command>
      ...
   }
```

```
   while (<expression>) {
      <statement/command>
      ...
   }
```

There is no for statement since this can be emulated with while. There are ways to iterate over a list (or an iterator).

```
   foreach <expression> <lambda-expression/function-name>
   map <expression> <lambda-expression/function-name>
   foreach (range 0 99) lambda (x) {
     echo x
   }
```

## Strucutures

```
   structure color (red green blue)
   set b (make-color 0 0 255)
   echo $b
   echo $b.blue
   echo
```

## Lists

```
   set l (list a b c)
   echo (list-ref l 0)
   set l (cons x l)
   # car, cdr
```
   
