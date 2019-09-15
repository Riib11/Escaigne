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
  | Symbolic Value «name».
  | Symbolic Function «name» «integer».
  | Symbolic Operation «name».
  | Table «name».

«expression» ::=
  | «value»
  | ( «parameters» => «expression» )
  | ( «expression» «arguments» )
  | ( value «name» := «expression» ; «expression» )
  | ( function «name» «parameters» := «expression» ; «expression» )
  | ( «name» [ «expression» ] )

«parameters» ::= «name» «name» ... «name»

«arguments» ::= «name» «name» ... «name»

«value» ::=
  | «integer»
  | «boolean»
  | «string»
  | «name»

```

The **Head** contains a set of headers which provide metadata about the file and program.

The **Prelude** contains a set of definitions that may be used elsewhere in the Prelude and also in the Generator.

The **Generator** contains a single expression that is expected to be a function that takes a single parameter, `n`, and evaluates to the `n`th term in the sequence it represents.

Note that the grammar is purposely kept very sparse. For example, simple operations are not included. That is because all strings, other than `.`, `;`, `(`, `)`, and keywords, can be used in `«name»`. Of course, many useful operations and functions will be defined in standard libraries.

### Statements

`Value n := e` binds `n` to `e`.

`Function f x1 ... xn := e` binds `f` to a function with parameters `x1, ..., xn` and evaluates to `e`.

`Operation a • b := e` binds `•` to a function with parameters `a, b` and evaluates to `e`. Additionally, `•` is marked as an infixed operator.

`Symbolic Value n`, `Symbolic Function f #`, and `Symbolic Operation •` bind their names to the appropriate type of value, which is marked as symbolic. A symbolic value cannot be reduced, and persists in the output. Symbolic values are automatically assumed to extend equality, but no other properties. Symbolic values are useful for forcing a certain representations - making explicit kinds of abstractions about the output sequence.

`Table t` declares a new table that can store values. Notably, these values persist during the evaluation of the generator over many entries of the sequence. This construct is useful for taking advantage of a dynamic-programming approach to computing some sequences (e.g. the Fibbonacci sequence).

## Application

Escaigne is used to provide a simple representation of integer sequences for online reference. This is useful because it allows for efficient generation of sequences on the fly, given a specification of how many entries to count out to.
