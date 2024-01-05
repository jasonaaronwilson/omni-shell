# Omni Shell

Omni shell is my design for a command line shell that is easier to program than bash while retaining a the simplicity of being suitable as a "command repl". It's meant to fill whatever middle ground there is between bash and languages like Ruby, Python, and Perl.

## Simple Commands

The most basic thing a shell can do is launch commands:

```
   <program-name> <program-arguments>*

   ls
   ls -1
```

## Redirection

Probably the same syntax as bash. >, <, and >> to redirect a file handle.

## Pipelines

Naturally | is still used to create pipe-lines of commands.

## Variable Substitution

Variable substitution is the familar syntax ${var-name}. This will look up a lexical variable and possibly turn it into a string if it isn't one already. If a lexical variable isn't found with that name than we use the environment variables.

## Variable Assignment

Variable assignment is accomplished with set like fish.

```
   set name <expression>
   setenv name <expression>
```

## Function Definitions

```
   function name (args...) {
   }
```

## Lambda Expressions

```
   lambda (x) {
   }
```

## Control Flow

```
   if (<expression>) {
   } else {
   }
```

```
   while (<expression>) {
   }
```
