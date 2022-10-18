# The Code Corrector

The problem of automated program repair is quite interesting since it has pedagogical value for students who are beginning to learn programming. 

Given a program in (say the C language) that fails to compile, we want to suggest repairs to the student who wrote that code. Such a tool can lessen the load of teaching assistants and make the learning experience more enjoyable for students. However, before we can perform repair, we need to find out what sort of error is present in the program. Although the compiler often does give some hints as to what sort of error might be present, these are often insufficient. Thus, we will try to solve this problem using machine learning techniques.

## Data Points

Each data point corresponds to a single line in the code (C programming language) that contained the error (for sake of simplicity, we are only considering simple cases where the error was localized in a single line of the code – correcting that one line would make the program compile-worthy). 

A line of code is represented as a sequence of tokens 
e.g. 
`for(j = 0, j < N, j++)` which consists of 13 tokens namely `for`, `(`, `j`, `=`, `0`, `,`, `j`, `<`, `N`, `,`, `j`, `++`, and `)`, in that order.

However, since variable names (identifiers) like j, N, and constant values (literals) like 0
do not matter when correcting single line errors (they may matter when correcting long-range
errors like variable name mistakes, scoping issues etc), we abstract out lines by replacing iden-
tifiers and literals by stand-in tokens as follows :
```
for(TypeKind.INT = TokenKind.LITERAL_INT,TypeKind.INT < TypeKind.INT, TypeKind.INT++)
```
This gives us a dictionary of tokens consisting of reserved keywords e.g. `for`, stand-in tokens e.g. `TypeKind.INT` and punctuation marks and operators e.g. `,`, `++`, `(` etc. 

We then create a bag-of-words (BoW) representations for the line by counting the number of occurrences of each dictionary token in that line. 

For example, for the above line, the BoW representation would look something like the following
```
for: 1
(: 1
TypeKind.INT: 4
=: 1
TokenKind.LITERAL_INT: 1
,: 2
<: 1
++: 1
): 1
```
since the token `TypeKind.INT` appears four times in the line whereas the token `<` appears only once in the line etc. Since we have 225 dictionary tokens, each BoW vector is represented as a d = 225 dimensional vector. However, this vector is usually very sparse since most lines contain only a few unique tokens. 
