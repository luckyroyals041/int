---

### **1. Operator Precedence Parser in C**
**AIM:**  
To implement a top-down parser using operator precedence for arithmetic expressions.

**DESCRIPTION:**  
The program uses two stacks, one for operators and another for operands, to parse arithmetic expressions based on operator precedence rules. It dynamically constructs the parse tree as a string representation of sub-expressions. The input expression is tokenized, and operators are reduced based on precedence to maintain the correct order of operations.

**PROGRAM:**
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <string.h>

#define MAX_STACK_SIZE 100

// Stack for operators and operands
char operatorStack[MAX_STACK_SIZE];
char operandStack[MAX_STACK_SIZE][MAX_STACK_SIZE];
int operatorTop = -1, operandTop = -1;

// Precedence table
int precedence(char op) {
    switch (op) {
        case '+': return 1;
        case '-': return 1;
        case '*': return 2;
        case '/': return 2;
        default: return -1;
    }
}

// Push operator onto stack
void pushOperator(char op) {
    operatorStack[++operatorTop] = op;
}

// Pop operator from stack
char popOperator() {
    return operatorStack[operatorTop--];
}

// Push operand onto stack
void pushOperand(char *operand) {
    strcpy(operandStack[++operandTop], operand);
}

// Pop operand from stack
char *popOperand() {
    return operandStack[operandTop--];
}

// Perform reduction and construct subexpression
void reduce() {
    char operator = popOperator();
    char *rightOperand = popOperand();
    char *leftOperand = popOperand();
    
    char subexpression[MAX_STACK_SIZE];
    sprintf(subexpression, "(%s %c %s)", leftOperand, operator, rightOperand);
    
    pushOperand(subexpression);
}

// Parse the expression
void parse(char *expression) {
    char *token = strtok(expression, " ");
    while (token) {
        if (isdigit(token[0])) {  // Operand
            pushOperand(token);
        } else {  // Operator
            while (operatorTop != -1 &&
                   precedence(operatorStack[operatorTop]) >= precedence(token[0])) {
                reduce();
            }
            pushOperator(token[0]);
        }
        token = strtok(NULL, " ");
    }
    
    // Reduce remaining operators
    while (operatorTop != -1) {
        reduce();
    }
}

int main() {
    char expression[] = "3 + 5 * 2 - 8";  // Example input
    
    parse(expression);
    printf("Parsed Expression Tree: %s\n", operandStack[operandTop]);
    
    return 0;
}
```

**OUTPUT:**  
For input expression `3 + 5 * 2 - 8`, the program outputs:  
```
Parsed Expression Tree: ((3 + (5 * 2)) - 8)
```

---

### **2. Lexical Analyzer using Flex**
**AIM:**  
To create a lexical analyzer using Flex that identifies keywords, operators, numbers, and identifiers from a given input program.

**DESCRIPTION:**  
This program uses Flex to tokenize input text into predefined categories like keywords (e.g., `int`, `if`, `while`), operators (e.g., `+`, `-`, `*`, `/`, `=`), numbers, and identifiers. It uses regular expressions to match patterns and outputs the matched token type for each input.

**PROGRAM:**
```flex
%{
#include <stdio.h>
%}

/* Keywords and Operators */
KEYWORD    (int|if|while|return|else)
OPERATOR   (\+|\-|\*|\/|\=)

%%

{KEYWORD}   { printf("Keyword: %s\n", yytext); }
{OPERATOR}  { printf("Operator: %s\n", yytext); }
[0-9]+      { printf("Number: %s\n", yytext); }
[a-zA-Z_][a-zA-Z_0-9]* { printf("Identifier: %s\n", yytext); }
[ \t\n]     ;  /* Ignore whitespace */
.           { printf("Unknown character: %s\n", yytext); }

%%

int main() {
    yylex();  /* Start lexical analysis */
    return 0;
}
```

**OUTPUT:**  
For input text:  
```
int x = 5 + y;
```
The output will be:  
```
Keyword: int
Identifier: x
Operator: =
Number: 5
Operator: +
Identifier: y
Unknown character: ;
```

---

Let me know if you'd like further clarification or enhancements to the programs! 😊
