# Escaigne

A programming language for defining integer sequences.

## Language

Each _Escaigne_ program defines exactly one integer sequence, using the following grammar:

```
«program» ::=
  Begin Head.      «header»     End Head.
  Begin Prelude.   «statements» End Prelude.
  Begin Generator. «expression» End Generator.

«header» ::=
  | Title:       «string».
  | Author:      «string».
  | Description: «string».
  | «name»:      «string»

«statement» ::=
  | Value «name» := «expression».
  | Function «name» «parameters» := «expression».
  | Operation «name» «name» «name» := «expression».
  | Symbol «name» := «expression».
  | Table «name».

«expression» ::=
  | «value»
  | («parameters» => «expression»)
  | («expression» «expression»)
  | («name» := «expression» ; «expression»)
```

The **Head** contains a set of headers which provide metadata about the file and program.

The **Prelude** contains a set of definitions that may be used elsewhere in the Prelude and also in the Generator.

The **Generator** contains a single expression that is expected to be a function that takes a single parameter, `n`, and evaluates to the `n`th term in the sequence it represents.

Note that the grammar is purposely kept very sparse. For example, simple operations are not included. That is because all strings, other than `.`, `;`, and keywords, can be used in `«name»`. Of course, many useful operations and functions will be defined in standard libraries.

## Application

Escaigne is used to provide a simple representation of integer sequences for online reference. This is useful because it allows for efficient generation of sequences on the fly, given a specification of how many entries to count out to.
