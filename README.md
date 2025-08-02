#include <stdio.h>
#include <ctype.h>  // for isalnum
#include <string.h>
#define MAX 100
char stack[MAX];
int top = -1;
void push(char c) {
    stack[++top] = c;
}
char pop() {
    return stack[top--];
}
char peek() {
    return stack[top];
}
int precedence(char op) {
    switch (op) {
        case '^': return 3;
        case '*':
        case '/': return 2;
        case '+':
        case '-': return 1;
        default: return 0;
    }
}
int isOperator(char c) {
    return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
}
void infixToPostfix(char* infix) {
    char postfix[MAX];
    int j = 0;
    for (int i = 0; infix[i] != '\0'; i++) {
        char c = infix[i];
        if (isalnum(c)) {
            postfix[j++] = c;
        } else if (c == '(') {
            push(c);
        } else if (c == ')') {
            while (top != -1 && peek() != '(') {
                postfix[j++] = pop();
            }
            pop(); // remove '('
        } else if (isOperator(c)) {
            while (top != -1 && precedence(peek()) >= precedence(c)) {
                postfix[j++] = pop();
            }
            push(c);
        } 
    }
    while (top != -1) {
        postfix[j++] = pop();
    }
    postfix[j] = '\0';
    printf("\nPostfix Expression: %s\n", postfix);
}
int main() {
    char infix[MAX];
    printf("Enter infix expression: ");
    scanf("%s", infix);
    infixToPostfix(infix);
    return 0;
}
