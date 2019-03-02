## Sempre exercise solutions collection
**Sempre**: Semantic Parsing with Execution is an open source tool for customizing and building a semantic parser.

Sempre can be downloaded and all its documentation and tutorial can be found at 
https://github.com/percyliang/sempre

One can hopefully get a hand on Sempre by going through the exercises in the tutorial provided by the following link.
https://github.com/percyliang/sempre/blob/master/TUTORIAL.md

Here we give a collection of possible solutions to the exercises which is not included in the tutorial.

Use the following code to start a terminal from shell
```
./run @mode=simple -n
```
Use the following code to start a terminal and save the grammar.
```
./run @mode=simple -Grammar.inPaths data/tutorial-arithmetic.grammar
```

**1. Logical forms and denotations**

1.1. Find the first word in string (String needs to contain at least one ' ' (space character))
```
(execute (call .substring (string "what is this?") (number 0) (call .indexOf (string "what is this?") (string " ")) ) )
```
1.2. First word using lambda expressions
```
(execute ( (lambda x (call .substring (var x) (number 0) (call .indexOf (var x) (string " ")) ) )  (string "what is this?")) )
```

**2. Parsing utterances to logical forms**

2.1. String length
```
(rule $Stringlen (length of) (ConstantFn (lambda x (call .length (var x)))))
(rule $Expr ($Stringlen $PHRASE) (JoinFn forward))
(rule $ROOT ($Expr)(IdentityFn))
```

2.2. First word (parser version)
```
(rule $Firstword (first word in) (ConstantFn (lambda x (call .substring (var x) (number 0) (call .indexOf (var x) (string " ")) ) )))
(rule $String ($Firstword $PHRASE) (JoinFn forward))
(rule $ROOT ($String)(IdentityFn))
```

2.3. Complex parsing (The grammar fails to catch sentence ambiguity)
```
(rule $Operator (times) (ConstantFn (lambda y (lambda x (call * (var x) (var y))))))
(rule $Partial ($Operator $Expr) (JoinFn forward))
(rule $Expr ($Expr $Partial) (JoinFn backward))
(rule $Expr ($PHRASE) (NumberFn))
(rule $Stringlen (length of) (ConstantFn (lambda x (call .length (var x)))))
(rule $Expr ($Stringlen $PHRASE) (JoinFn forward))
(rule $ROOT ($Expr)(IdentityFn))
```

**3. Learning**

No exercise

**4. Lambda DCS and SPARQL**

4.1. Lambda DCS for utterances

"city with the largest area"
```
(execute (argmax 1 1 (fb:type.object.type fb:location.citytown) fb:location.location.area))
```
"top 5 cities by area"

```
(execute (argmax 1 5 (fb:type.object.type fb:location.citytown) fb:location.location.area))
```

4.2. Parsing (Some answers are unverified)

The first step,


"city with the largest area"  (argmax has syntax error)
```
(rule $ROOT ($N) (IdentityFn))
(rule $N (city with the largest area) (ConstantFn (argmax 1 1 (fb:type.object.type fb:location.citytown) fb:location.location.area) ))
```

"number of cities"
```
(rule $ROOT ($NUM) (IdentityFn))
(rule $NUM (number of cities) (ConstantFn (count (fb:type.object.type fb:location.citytown)) ))
```

**Acknowledgement**

Thanks to Sempre.
