### Singly Linked List implementation

```
// Singly Linked List implementation code using C program
#include<stdio.h>
#include<stdlib.h>

typedef struct node{
    int data;
    struct node *next;
}NODE;

NODE *start = NULL;

void display();
void insert_beg();
void insert_mid();
void insert_end();
void delete_beg();
void delete_mid();
void delete_end();
void clear_list();

int main(){
    int num,option;
    printf("\n\n *****MAIN MENU *****");
    printf("\n 0: EXIT");
    printf("\n 1: Display the list");
    printf("\n 2: Add a node at the beginning");
    printf("\n 3: Add a node on a given index");
    printf("\n 4: Add a node at the end");
    printf("\n 5: Delete a node from the beginning");
    printf("\n 6: Delete a node from a given index");
    printf("\n 7: Delete a node from the end");
    do{
        printf("\n Enter your option : ");
        scanf("%d", &option);
        switch(option){
            case 1:
                display();
                break;
            case 2:
                insert_beg();
                display();
                break;
            case 3:
                insert_mid();
                display();
                break;
            case 4:
                insert_end();
                display();
                break;
            case 5:
                delete_beg();
                display();
                break;
            case 6:
                delete_mid();
                display();
                break;
            case 7:
                delete_end();
                display();
                break;
            default:
                printf("Wrong Option\n");
                break;
        }
    }while(option!=0);
    clear_list();
    display();
    getch();
    return 0;
}

void display(){
    NODE *ptr;
    int i;
    ptr = start;
    printf("Data elements: ");
    while(ptr != NULL){
        printf("%d -> " , ptr -> data);
        ptr = ptr -> next;
    }
    printf("NULL\n");
}

void insert_beg(){
    NODE *new_node;
    int num;
    printf("\n Enter the data : ");
    scanf("%d", &num);
    new_node=(NODE *)malloc(sizeof(NODE));
    new_node->data= num;
    new_node->next = start;
    start = new_node;
}
void insert_mid(){
    NODE *new_node,*ptr;
    int num,posn,i;
    printf("\n Enter the data : ");
    scanf("%d", &num);
    printf("\n Enter index position : ");
    scanf("%d", &posn);
    new_node=(NODE *)malloc(sizeof(NODE));
    new_node->data= num;
    if (start==NULL || posn == 0){
        new_node->next=start;
        start=new_node;
    }
    else{
    ptr = start;
        for(i=1;i<posn && ptr->next != NULL; i++){
            ptr = ptr ->next;
        }
        new_node->next = ptr->next;
        ptr->next = new_node;
    }
}
void insert_end(){
    NODE *new_node, *ptr;
    int num;
    printf("\n Enter the data : ");
    scanf("%d", &num);
    new_node=(NODE *)malloc(sizeof(NODE));
    new_node->data= num;
    new_node->next = NULL;
    if(start==NULL){
        start = new_node;
    }
    else{
        ptr = start;
        while(ptr -> next != NULL){
            ptr = ptr -> next;
        }
        ptr -> next = new_node;
    }
}
void delete_beg(){
    NODE *ptr;
    ptr = start;
    if(ptr == NULL){
        printf("Linked List is already empty, Delete NOT possible!\n");
    }
    else{
        start = ptr ->next;
        printf("element: %d will be deleted from Linked List\n", ptr->data);
        free(ptr);
    }
}
void delete_mid(){
    int i, posn;
    NODE *ptr, *temp;
    ptr = start;
    printf("\n Enter index position : ");
    scanf("%d", &posn);
    if(ptr==NULL){
        printf("Linked List is already empty, Delete NOT possible!\n");
    }
    else if(ptr->next==NULL || posn == 0){
        start = ptr->next;
        printf("element: %d will be deleted from Linked List\n", ptr->data);
        free(ptr);
    }
    else{
        for(i=1; i<posn && ptr->next->next != NULL; i++){
            ptr = ptr->next;
        }
        temp = ptr->next;
        ptr->next = temp->next;
        printf("element: %d will be deleted from Linked List\n", temp->data);
        free(temp);
    }
}
void delete_end(){
    NODE *ptr, *temp;
    ptr = start;
    if(ptr == NULL){
        printf("Linked List is already empty, Delete NOT possible!\n");
    }
    else if(ptr->next == NULL){
        start = NULL;
        printf("element: %d will be deleted from Linked List\n", ptr->data);
        free(ptr);
    }
    else{
        while(ptr->next->next != NULL){
            ptr = ptr -> next;
        }
        temp =ptr->next;
        ptr->next = NULL;
        printf("element: %d will be deleted from Linked List\n", temp->data);
        free(temp);
    }
}
void clear_list(){
    NODE *temp;
    temp = start;
    while(start != NULL){
        temp = start;
        start  = start->next;
        free(temp);
    }
    if(start == NULL){
        printf("\nAll nodes deleted Successfully!\n");
    }
}

```
