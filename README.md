#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node* link;
};

struct node *head, *q, *ptr;

int main() {
    int n, item;
    printf("Enter the number of nodes: ");
    scanf("%d", &n);
    printf("Enter head node: ");
    scanf("%d", &item);
    
    head = (struct node*)malloc(sizeof(struct node));
    head->data = item;
    head->link = NULL;

    ptr = head;
    for (int i = 1; i < n; i++) {
        q = (struct node*)malloc(sizeof(struct node));
        printf("Enter next node: ");
        scanf("%d", &item);
        q->data = item;
        q->link = NULL;
        ptr->link = q;
        ptr = q;
    }

    ptr = head;
    printf("Linked list values:\n");
    while (ptr != NULL) {
        printf("%d ", ptr->data);
        ptr = ptr->link;
    }
    printf("\n");

    int x, count = 0;
    printf("Enter the number which you want to search: ");
    scanf("%d", &x);
    ptr = head; 
    while (ptr != NULL) {
        if (ptr->data == x) {
            count++;
        }
        ptr = ptr->link; 
    }
    printf("The number %d appears %d times.\n", x, count);
    

    return 0;
}
// second question   





#include <stdio.h>
#include <stdlib.h>

struct node {
    int data;
    struct node* link;
};

struct node *head, *q, *ptr;

int main() {
    int n, item;
    printf("Enter the number of nodes: ");
    scanf("%d", &n);
    printf("Enter head node: ");
    scanf("%d", &item);
    
    head = (struct node*)malloc(sizeof(struct node));
    head->data = item;
    head->link = NULL;

    ptr = head;
    for (int i = 1; i < n; i++) {
        q = (struct node*)malloc(sizeof(struct node));
        printf("Enter next node: ");
        scanf("%d", &item);
        q->data = item;
        q->link = NULL;
        ptr->link = q;
        ptr = q;
    }

    ptr = head;
    printf("Linked list values:\n");
    while (ptr != NULL) {
        printf("%d ", ptr->data);
        ptr = ptr->link;
    }
    printf("\n");

    int x, count = 0;
    printf("Enter the number which you want to search: ");
    scanf("%d", &x);
    ptr = head; 
    while (ptr->data!=x) {
       count++;
       ptr=ptr->link;
    }
    printf("The number %d index %d in linklist.\n", x, count);
    

    return 0;
}





// posfix evaluation 
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

#define MAX 100

// Stack structure
struct Stack {
    int top;
    int arr[MAX];
};

// Function to initialize the stack
void initStack(struct Stack* stack) {
    stack->top = -1;
}

// Function to check if the stack is empty
int isEmpty(struct Stack* stack) {
    return stack->top == -1;
}

// Function to push an element onto the stack
void push(struct Stack* stack, int value) {
    if (stack->top == MAX - 1) {
        printf("Stack Overflow\n");
        return;
    }
    stack->arr[++stack->top] = value;
}

// Function to pop an element from the stack
int pop(struct Stack* stack) {
    if (isEmpty(stack)) {
        printf("Stack Underflow\n");
        exit(1);
    }
    return stack->arr[stack->top--];
}

// Function to evaluate a postfix expression
int evaluatePostfix(char* exp) {
    struct Stack stack;
    initStack(&stack);
    
    for (int i = 0; exp[i] != '\0'; i++) {
        if (isdigit(exp[i])) { 
            push(&stack, exp[i] - '0');  // Convert character to integer
        } else {  
            int val2 = pop(&stack);
            int val1 = pop(&stack);

            switch (exp[i]) {
                case '+': push(&stack, val1 + val2); break;
                case '-': push(&stack, val1 - val2); break;
                case '*': push(&stack, val1 * val2); break;
                case '/': push(&stack, val1 / val2); break;
            }
        }
    }
    return pop(&stack);
}

// Driver Code
int main() {
    char exp[MAX];

    printf("Enter postfix expression (single-digit numbers & operators): ");
    scanf("%s", exp);

    printf("Result = %d\n", evaluatePostfix(exp));
    
    return 0;
}




//infix to postfix 
#include <stdio.h>
#include <ctype.h>

char stack[50];
int top = -1;

void push(char x) {
    stack[++top] = x;
}

char pop() {
    if (top == -1) {
        return -1;
    }
    return stack[top--];
}

int priority(char x) {
    if (x == '(') return 0;
    if (x == '+' || x == '-') return 1;
    if (x == '*' || x == '/') return 2;
    return -1;
}

int main() {
    char exp[50];
    char *e, x;
    
    printf("Enter an infix expression: ");
    scanf("%s", exp);
    
    e = exp;
    
    while (*e != '\0') {
        if (isalnum(*e)) {
            printf("%c", *e);
        } 
        else if (*e == '(') {
            push(*e);
        }
         else if (*e == ')') {
            while ((x = pop()) != '(') {
                printf("%c", x);
            }
        }
         else {
            while (top != -1 && priority(stack[top]) >= priority(*e)) {
                printf("%c", pop());
            }
            push(*e);
        }
        e++;
    }
    
    while (top != -1) {
        printf("%c", pop());
    }
    
    return 0;
}




//infix evaulation

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

#define MAX 100

// Stack structure for numbers
typedef struct {
    int arr[MAX];
    int top;
} NumStack;

// Stack structure for operators
typedef struct {
    char arr[MAX];
    int top;
} OpStack;

void pushNum(NumStack *s, int value) {
    if (s->top == MAX - 1) return;
    s->arr[++(s->top)] = value;
}

int popNum(NumStack *s) {
    if (s->top == -1) return 0;
    return s->arr[(s->top)--];
}

void pushOp(OpStack *s, char value) {
    if (s->top == MAX - 1) return;
    s->arr[++(s->top)] = value;
}

char popOp(OpStack *s) {
    if (s->top == -1) return 0;
    return s->arr[(s->top)--];
}

char peekOp(OpStack *s) {
    if (s->top == -1) return 0;
    return s->arr[s->top];
}

int precedence(char ch) {
    switch (ch) {
        case '+': case '-': return 1;
        case '*': case '/': return 2;
        default: return -1;
    }
}

void evaluate(NumStack *values, OpStack *operators) {
    int v2 = popNum(values);
    int v1 = popNum(values);
    char op = popOp(operators);
    
    switch (op) {
        case '+': pushNum(values, v1 + v2); break;
        case '-': pushNum(values, v1 - v2); break;
        case '*': pushNum(values, v1 * v2); break;
        case '/': pushNum(values, v1 / v2); break;
    }
}

int evaluateInfix(const char *exp) {
    NumStack values = {.top = -1};
    OpStack operators = {.top = -1};
    
    for (int i = 0; exp[i] != '\0'; i++) {
        char c = exp[i];
        
        if (isdigit(c)) {
            pushNum(&values, c - '0');
        } else if (c == '(') {
            pushOp(&operators, c);
        } else if (c == ')') {
            while (operators.top != -1 && peekOp(&operators) != '(') {
                evaluate(&values, &operators);
            }
            popOp(&operators); // Remove '('
        } else {
            while (operators.top != -1 && precedence(peekOp(&operators)) >= precedence(c)) {
                evaluate(&values, &operators);
            }
            pushOp(&operators, c);
        }
    }
    
    while (operators.top != -1) {
        evaluate(&values, &operators);
    }
    return popNum(&values);
}

int main() {
    char infix[50];
    printf("Enter an infix expression: ");
    scanf("%S", infix);
    printf("Evaluation Result: %d\n", evaluateInfix(infix));
    return 0;
}









