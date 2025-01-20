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
