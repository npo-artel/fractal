---
title: Грамматика языка fractal (v1.0)
---


!!! Язык ***конструирования контекстно связанных рекурсивных вычислений (context-compute-construct-recursive)*** для трансляции иерархических (или сводимых к иерархическим) структур данных в RDF представление

## Пример

* [реестр предметов](реестр-предметов.json)
* [расписание](расписание.json)

*Легенда:*
    - **::=** - эквивалентно
    - **(** ... **)** - конструкция встречается один раз
    - **(** ... **)?** - конструкция встречается ноль или один раз
    - **(** ... **)** * - конструкция может встречаться ноль или несколько раз
    - **|**  - "или" одна из перечисленных конструкций

## 
1. Базовые конструкции:
    1. `fractal` ::= { "description" : `Description` } 
    1. `Description` ::= { **(** `Context` **)?** **(** ,`About` **)?** **(**, `Properties` **)?** }
    1. `Properties` ::= "properties" : [ `Property` **(** , `Property` **)** * ]
    1. `Property` ::= { `QName` **(**, `Context` **)?**  **(** , `URI` | `Literal` | `Description` **)** }

1. Частные конструкции:
    1. `URI` ::= "URI" : [URIExpr](https://www.ietf.org/rfc/rfc3986.txt) | { `computedExpression` }
    1. `Context` ::= { **(** `PredicatesList` **)** **(** , `PseudoVariable` **)** * }
    1. `pseudoVariable` ::= `Name` : **(** `Literal` | `computedExpression` **)**
    1. `About` ::= "about" : **(** `Literal` | `computedExpression` **)**
    1. `QName` ::= "QName" : { { "nameSpace" : [URIExpr](https://www.ietf.org/rfc/rfc3986.txt) } , { "PrefixedName" : [PrefixedName](https://www.w3.org/2009/sparql/docs/query-1.1/rq25-pub-2011-05.html#rPrefixedName) } }
    1. `PredicatesList` ::= "predicates" : [ `LiteralExpr` **(**, `LiteralExpr` **)** * ] | { `computedExpression` } - задает перечесление предикатов, к которым итеративно будет применяться инструктуция
    1. `Literal` ::= "literal" : [LiteralExpr](https://www.w3.org/TR/xquery-31/#prod-xquery31-Literal) | { `computedExpression` }
    1. `computedExpression` ::= "compute" : { `xquery` | `sparql` | `include` }
    1. `xquery` ::= "xquery" : [XQueryExpr](https://www.w3.org/TR/xquery-31/) | [ `XQueryExpr` **(** , `XQueryExpr` **)** ] | { `include` } 
    1. `sparql` ::= "sparql" : [SPARQLExpr](https://www.w3.org/TR/2013/REC-sparql11-query-20130321/) | { **(** { `Context` }, **)** `include` }
    1. `include` ::= "include" : { `URI` }