#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node* next;
};


struct Node* createNode(int data) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}


void insertMiddle(struct Node** head_ref, int data) {
    struct Node* newNode = createNode(data);
    struct Node* slow = *head_ref;
    struct Node* fast = *head_ref;

   
    if (*head_ref == NULL) {
        *head_ref = newNode;
        return;
    }

   
    while (fast != NULL && fast->next != NULL) {
        slow = slow->next;
        fast = fast->next->next;
    }

 
    newNode->next = slow->next;
    slow->next = newNode;
}


void deleteMiddle(struct Node** head_ref) {
    
    if (*head_ref == NULL) {
        return;
    }

    struct Node* slow = *head_ref;
    struct Node* fast = *head_ref;
    struct Node* prev = NULL;

    
    if (slow->next == NULL) {
        free(slow);
        *head_ref = NULL;
        return;
    }

    
    while (fast != NULL && fast->next != NULL) {
        fast = fast->next->next;
        prev = slow;
        slow = slow->next;
    }

    
    if (prev != NULL) {
        prev->next = slow->next;
        free(slow);
    }
}


void printList(struct Node* node) {
    while (node != NULL) {
        printf("%d -> ", node->data);
        node = node->next;
    }
    printf("NULL\n");
}


void freeList(struct Node* head) {
    struct Node* temp;
    while (head != NULL) {
        temp = head;
        head = head->next;
        free(temp);
    }
}


int main() {
    struct Node* head = NULL;

    // Create a linked list 1->2->3->4->5
    head = createNode(1);
    head->next = createNode(2);
    head->next->next = createNode(3);
    head->next->next->next = createNode(4);
    head->next->next->next->next = createNode(5);

    printf("Original List: ");
    printList(head);

   
    insertMiddle(&head, 10);
    printf("After Inserting 10 in the Middle: ");
    printList(head);

    
    deleteMiddle(&head);
    printf("After Deleting Middle Node: ");
    printList(head);

   
    freeList(head);
    return 0;
}