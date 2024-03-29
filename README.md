# Omni Shell

I want a shell that scales cleanly from a command as simple as "ls" to 10,000 lines
or more of code.

After doing some initial design work on Omni Shell, an unrelated project had me take
a look at php and I beleive it is the scripting language I was looking for all these
years. It doesn't really qualify as a shell since you can't just type "ls" but it
should be able to handle my scripting needs and it's available on must major operating
systems already.

# On Hold For Now

Omni shell is my design for a command line shell that is easier to program than bash while retaining the simplicity of being suitable as a "command repl". It's meant to fill the ground between bash and languages like Ruby, Python, and Perl. Expression syntax is most similar to Scheme (though with immutable values (except closures)) while command syntax is very much like bash and other shells.

In command context, lists are automatically flattened as individual arguments (though one can still use the comma operator if desired) while in expression context one must use ",".

## Syntax

Tokens are either delimited by white-space or certain delimiters.

The pound sign begins a comment that continues to the end of the document.

Commands fit on one line unless continued with \ at the very end of the line.

Strings begin and end with " or """ and are interpolated.

## Commands Context versus Expression Context

Omni Shell starts out in command context where everything is either a command/function call or a statement. An open paren starts an expression context. In expression context, variables no longer need "$", OTOH strings must now be fully qouoted. In expression context, \ is no longer needed. Some statements or expressions have blocks inside of them (blocks begin with "{" and end with a matching "}"). Inside of a block, we reenter the command context where commands end at the end of the line unless \ is used.

## Simple Commands

The most basic thing a shell can do is launch external commands/programs:

```
   <program-name> <program-arguments>*
   ls
   ls -1
```

## Function Calls

Where ever a command is expected, if the command name matches a function name, a function call is done instead. There are a large number of built-in functions. Functions calls also happen in expression context.

Unlike traditional languages, function calls can sometimes accept "flags" which begin with "-".

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

To accept variable number of arguments, use a * at the end of the argument list and then use $* to access the arguments past the last named argument:

```
function append (a b *) {
  if (== 0 (length $*)) {
    return (cons a (cons b (make-list)))
  }
  return (append a (append b ,$*))
}
```

To accept flags, a function must declare and parse them.

```
   function split (*) {
     flag --name=delimiter --type=string --default=" "
     set left-overs parse_flags($*)
     set current ""
     set result (list)
     for-each left-overs lambda (str) {
        for-each-character str lambda (code-point) {
          if (contains? delimiter code-point) {
             set result (cons current result)
             set current ""
          }
        }
     }
     return (reverse result)
   }

   split --delimiter=, "a,b,c,d,e,f,g"
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

Structures are immutable.

```
   structure color (red green blue)
   set b (color 0 0 255)
   echo $b
   echo $b.blue
   echo (xform-color b --red 128)
```

## Lists

```
   set l (list a b c)
   echo (list-ref l 0)
   set l (cons x l)
   # car, cdr
```

## Maps

```
   set m (make-map)
   set m (put m "key" "value")
   echo (get $m "key")
```
   
