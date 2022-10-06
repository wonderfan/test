# Nginx

## Request Limit

In the first part of source code, the data types and function prototypes are defined. The function implementations are declared in the following part. The functions are grouped into command list. The type variables are instantialized by literal statement.The request limit handler is created to enforce the request limit. The input parameter is http request type. The http request is standed by a struct type. 

We should think how the object are represented by data structure. The definition of data structure means one type in the digital world. The nginx has a set of type system. The memory is allocated in the unit of pool. The data types of network are defined in the network interface file. The data and its operations are defined in the same source code file. 

## Connection Limit

In computing, a compiler is a computer program that translates computer code written in one programming language (the source language) into another language (the target language). The name "compiler" is primarily used for programs that translate source code from a high-level programming language to a lower level language (e.g. assembly language, object code, or machine code) to create an executable program.

A compiler is likely to perform some or all of the following operations, often called phases: preprocessing, lexical analysis, parsing, semantic analysis (syntax-directed translation), conversion of input programs to an intermediate representation, code optimization and code generation. Compilers generally implement these phases as modular components, promoting efficient design and correctness of transformations of source input to target output. Program faults caused by incorrect compiler behavior can be very difficult to track down and work around; therefore, compiler implementers invest significant effort to ensure compiler correctness.

High-level languages are formal languages that are strictly defined by their syntax and semantics which form the high-level language architecture. Elements of these formal languages include:

Alphabet, any finite set of symbols;
String, a finite sequence of symbols;
Language, any set of strings on an alphabet.

