---
layout: post
title: "How to write your own compiler and executor for mathematical expressions"
page_title: "How to write your own compiler and executor for mathematical expressions"
title_image: "/assets/img/2021-04-05-expressions/title_image.png"
image:
    path: "/assets/img/2021-04-05-expressions/title_image.png"
date: 2021-04-05 16:00:00 +0300
images_folder: "/assets/img/2021-04-05-expressions"
excerpt: "Let's talk about infix, prefix and postfix notations, abstract syntax trees, and how to convert a human-readable expression into a computer-digestible from."
---
Let's talk about infix, prefix and postfix notations, abstract syntax trees, and how to convert a human-readable expression into a computer-digestible from.

Sometimes we need our programs to be able to run simple mathematical expressions entered by a user. E.g., it can be an arithmetical expression in a comand-line calculator, an expression to process a result of analytical device measurements, or it can be an expression to calculate scores for a player in a videogame. Languages for such expressions are sometimes called scripting languages or domain-specific languages.

A user enters an human-readable expression as a string, and the program needs to parse it, convert it into a form understandable by a computer, and then execute it using some data. Unfortunately, expression notations suitable for humans and for computers usually are different and require some translation.

![]({{page.images_folder}}/title_image.png)

# Infix notation

Let's start with a simple expression: "1 + 2". This is a mathematical operation that sums two numbers. The "+" sign is an operator, it show what action we need to do. "1" and "2" are operands, they are objects what our operation works with. The same applies to logical expressions, e.g., "A and B or C".

When we use arithmetical or logical expressions and if we assume that these expressions will be read by humans, we usually put operators between operands. It is called "infix notation", the operator is INside the expression.

This is the expression notation we all have got used to, but it has a couple of disadvantages. First, we need to take care of operation priorities if we have multiple operators in a single expression. E.g., multiplication is stronger than sum. In a expression "A + B * C" we calculate "A * B" first and then sum its result with A. If we want to change the order and calculate the sum first, we need to use parentheses (round brackets): "(A + B) * C".

Second, infix notation isn't the best notation if we we want to teach a computer to execute mathematical or logical expressions. Mostly because the order of execution is not the same as the order of reading.

# Prefix notation

Prefix notation, also know as Polish notation or normal Polish notation, is when we put the operator before its operands. "1 + 2" becomes "+ 1 2". "A or B" becomes "or A B". Humans are not got used to it, so it might look a bit strange or difficult. It gets even worse if we have expressions with multiple operators, e.g., "A and B or C" becomes "or and A B C".

The basic idea is that we read a token from the expression one by one. If it's an operator, then we continue reading until we have enough operands for it and execute it. But if we find another operator when we try to read an operand, then we need to calculate it and its result will be our operand.

With prefix notation we don't need to think about priorities any more, because the order of execution is written in the expression itself. "A + B * C" turns into "+ A * B C", which means we need to sum A and the result of multiplication of B and C. "(A + B) * C" turns into "* + A B C" which means we need to multiply the result of sum of A and B, and C.

Prefix notation fits really good for recursive execution. Let's say we have a function
`execute(operator, operand1, operand2)`. Then to calculate "+ A * B C" we need to run this code:

```c
execute('+', A, execute('*', B, C));
```

To calculate "* + A B C" we will run this code:

```c
execute('*', execute('+', A, B), C);
```

# Postfix notation

Postfix notation, also know as reverse Polish notation, is when we put the operator after its operands. "1 + 2" becomes "1 2 +", "A + B * C" becomes "A B C * +", "(A + B) * C" becomes "A B + C *".

Postfix notation has the same advantages as prefix notation, they just have different approaches to execution. The basic idea is that we read tokens from an expression one by one. If it's an operand, then we keep it in memory. If it's an operator, we get operands from memory, execute it, and then keep its result in the same memory.

Postfix notation is really good for stack-based execution. When I wrote that we "keep operands in memory" I didn't mention how exactly we do it. Here comes the stack.

Let's calculate an expression: "9 - 2 * 3". In postfix notation it is "9 2 3 * -". We read tokens one by one. First, we get 1. It's an operand, so we put it on the stack.

```c
| 9
```

Next, we read 2. We put it on the stack.

```c
| 9 2
```

Next, we read 3. We put it on the stack.

```c
| 9 2 3
```

Now we have "*". It's a binary operator, so we need to get 2 operands for it. Let's get 2 last operands from the stack. We have 2 and 3. If we multiply them, we will get 6. We need to put the result on the stack.

```c
| 9 6
```

Next, we read "-". It's a binary operator, we need to get 2 operands from the stack. We have 9 and 6 for it. The result is 3. We put it back on the stack.

```c
| 3
```

We done reading the expression, there are no more tokens. If the expression was correct, then the stack must have exactly one value and it is the result of the expression.

# Abstract syntax trees

Prefix and postfix notations has one disadvantage: they assume that each operator has fixed and known arity (the number of operands). In our examples it wasn't a problem because most of arithmetic operations are binary, they have two operands (+, -, *, /, etc.). But what if we have functions with variable amount of arguments. Functions are just like operators, they are basically the same.

For example, let's define operators "max" and "in".

```c
max(a, b, c, ...)
a in (b, c, ...)
```

The "max" operator returns the max value from the list of its operands. The "in" operator returns true if its first operand equals to at least one of the other operands.

Let's say, we have this expression for whatever reason: "max(a, b) in (c, d, e)". Here is the prefix notation for it:

```c
in max a b c d e
```

Let's take another example expression: "max(a, b, c) in (d, e)":

```c
in max a b c d e
```

As you might notice, they are the same in their prefix forms. There are some possible solutions for it.

### Use parentheses

```c
in ( max ( a b ) c d e )
in ( max ( a b c ) d e )
```

It solves the problem but makes implementation of the expression executor more complex. And I can feel some Lisp vibes here.

### Pass arity as the first operand

```c
in 4 max 2 a b c d e
in 3 max 3 a b c d e
```

I like this solution more but it still feels wrong. Like we lose elegance for Polish notation.

### Use non-linear data structures

Instead of using linear notation as we got used to with infix notation, we can go deeper and use another data structure. Our main issue here is that in the line "a b c d e" we cannot find what tokens belong to "max" and what token belong to "in". But what if we put tokens for different operators to different lines? We can use trees.

![]({{page.images_folder}}/1.png)

![]({{page.images_folder}}/2.png)

It's much clear now where are operands for "max" and where are operands for "in". Let's take a look at another example: "(a + b) * (c - d)". 

![]({{page.images_folder}}/3.png)

Let's do a depth-first traversal of this graph and print visited tokens. We will get this result:

```c
* + a b - c d
```

This is the prefix form of our expression. For this reason I called this tree structures "prefix trees" when I was working with them. If you search for "prefix trees" in Google you will find out that prefix trees are something different and these trees we have here are actually abstract syntax trees (AST). According to Wikipedia, they are used in compilers at one of the syntax analysis stages.

# Convert an expression into an abstract syntax tree

Internet suggests the shunting-yard algorithm to convert an infix-form expression into a postfix form or to an AST and it might be a good idea to use it. When I was working on this subject I used this algorithm as reference and created my own. Basically, the algorithm iterates over the expression's tokens and inserts them into the tree on the right position depending on the token's priority.

The algorithm keeps pointer to the current node. On the first step I create a stub "root" node to avoid nulls. Then until we reach the end of the expression we get tokens, identify their types, and then take different actions depending on the type. This is similar to the shunting-yard algorithm. The insertion actions are different though.

I identify a token's type in this order:

- if the token is "(", then it's a left parenthesis;
- if the token is ")", then it's a right parenthesis;
- if the token is ",", then it's an argument delimiter;
- if the token is on the list of known function, then it's a function;
- if the token is on the list of known operators, then it's an operator;
- else it is an operand (a variable name or a literal).

### Operand

If the token is an operand, then we just append it to the current node as a child node. By operands I mean variables and constants, e.g., x, 0, 42, 3.14, "hello", etc.

The current node points to the same node, no changes here.

![]({{page.images_folder}}/4.png)

### Operator

By operators I mean arithmetic and logical operators with at least one operand written on the left side from the operator: +, -, *, in, or, and, comparison operators, etc. We need to compare the priority of the current node's token with the priority of the new token. Here's the priority list I use (from lowest to highest):

```python
root, subtree_root, or, -, +, and, /, *, in, >=, >, <=, <, ==, operand
```

"Root" on the left means no token can be a parent node for the root node. "Operand" on the right means that no token can be a child node for an operand. I'll explain the "subtree_root" later in this text.

Then actions are different depending on the priorities of the current and the new nodes.

If the new operator's priority is higher or the same, then it must be placed lower in the tree, assuming that the root is on the top (yes, computer trees normally grow from top to bottom). We need to insert this token between the current node and its last child. The last child of the current node becomes the child of the new node and the new node becomes the child of the current node.

To understand why we insert it before the last child, we need to remember, that we parse infix notation and we have already read the first operand for this operator. E.g., let's take a look at expression: "a + b * c". The algorithm reads "b" before "*". It thinks it's the second operand for "+" and inserts it as a child for "+". Then it reads "*" and realizes that "b" must be multiplied before summing, and therefore it moves "b" to the children of "*".

Then we change the current node to the inserted one, because we are reading its operands now.

![]({{page.images_folder}}/5.png)

If the new operator's priority is lower, then we need to insert it higher in the tree than the current node. We go up in the tree until we find a node with priority lower or equal then the new token's priority. We need to do it because the current node is actually an operand for the new node, so the new node becomes the parent node for the current node.

The root node has the lowest priority and it guarantees that this upward search will stop before it if there are no other nodes with lower priority.

Then we change the current node to the inserted one, because we are reading its operands now.

![]({{page.images_folder}}/6.png)

I would say that this approach that I use for operators in this algorithm is similar to the insertion sort algorithm, but for trees.

### Function

Although I have already mentioned in this article that operators and functions are basically the same, I actually mean that they are semantically the same. Syntactically they are different, and functions are much easier for the algorithm. First, functions are always written in the prefix-form, i.e. a function name always goes before its arguments. Second, a function's arguments are always calculated before the function.

```python
max(a + b, c - d)
```

In this example we can be sure that "a + b" and "c - d" will be executed before "max(...)".

It means we don't need to use the priority sorting for functions. We can just put the function node as a child node to the current node and then set the current node pointer to it.

![]({{page.images_folder}}/7.png)

### Left parenthesis

Parentheses mean that everything inside them must be executed before the current node. They override the priority sorting. It works both for parentheses we use in mathematical expressions to change priority and for functions parentheses, so we can use the same approach for both cases.

I use a small hack: I insert a temporary "subtree_root" node with low priority to deal with parentheses. That means that if there will be priority sorting for operators in this subtree, then no node can go outside it, because no operator can have priority less than the subtree_root.

But before it we need to push the current node to a stack. The current node will be changed during the subtree parsing and we will need to return to it.

![]({{page.images_folder}}/8.png)

### Right parenthesis

A right parenthesis means that the high priority section has ended. We pop a node from the stack and set the current node pointer to it. We don't need the "subtree_root" node anymore, so we can delete it and move all its children to the current node.

![]({{page.images_folder}}/9.png)

### Argument delimiter

Argument delimiters (commas in most cases) split operands for functions (e.g., "max(a, b)") and sometimes for operators (e.g., "a in (b, c)"). They are always used together with parentheses. It means that if we find a comma in an expression, we can assume that we are inside parentheses block, and therefore there is a "subtree_root" node in the tree.

When we handle subtree operators, the current node can point to an operator in this subtree. When we get next token after a comma, it will be added to this curren node, e.g. multiplication. It is wrong because that token must be added to the subtree_root. This means, that we just need to point the current node to the nearest "subtree_root" node.

![]({{page.images_folder}}/10.png)

# Execute an abstract syntax tree

The same execution approach I mentioned for prefix notation works for syntax trees as well. Trees are really good for recursion. To calculate an expression we need to do a recursive depth-first traversal (DFT) of its syntax tree and execute each node. Python-like pseudocode would be like this:

```python
operators = ['+', '-', '*', '/', 'in', ...]
functions = ['max', 'min', 'sin', 'cos', 'round', ...]

def execute(node):
  if node.token in operators or node.token in functions:
    operands = []
    for childNode in node.children:
      operands.append(execute(childToken))
    result = ... # execute the operator or the function with its operands
    return result
  else:
    return node.token # we might need to parse string to int or do something else here

result = execute(syntaxTreeRootNode)
```

Operands can be variables. In this case we need to add context to the executor. The simplest context is a dictionary.

```python
context = {
  "x": 2,
  "pi": 3.14
}
```

In this case when we have an operand node, we need to search it in the context. If not found, then it's probably a constant, and we need to try to parse it from a string.

---

I don't add more code examples because the implementation really depends on the programming language. In this text I used Python-like pseudocode, but when I implemented this system in a real project, I used Java. With Python it is easier to work with different types (ints, floats, strings). In Java I had to implement my own type casting system to make sure that all the nodes return the right type, but I was able to add compile-time type checks to make sure that we don't try to sum an integer and a string. In JavaScript it would be even more different.