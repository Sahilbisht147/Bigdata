#include"mylist.h"
#include<stdio.h>
#include<stdlib.h>
struct node {
    int data;
    struct node* next;
};

struct node* create(char* str) {
    struct node* head = NULL;
    struct node* temp = NULL;

    for (int i = 0; str[i] != '\0'; i++) {
        int x = str[i] - '0';
        struct node* n = (struct node*)malloc(sizeof(struct node));
        n->data = x;
        n->next = NULL;
        if (head == NULL) {
            head = temp = n;
        } else {
            temp->next = n;
            temp = n;
        }
    }
    return head;
}

struct node* rev_list(struct node* head) {
    struct node* curr = head;
    struct node* prev = NULL;
    struct node* next = NULL;

    while (curr != NULL) {
        next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }

    return prev;
}

struct node* add_lists(struct node* head1, struct node* head2) {
    head1 = rev_list(head1);
    head2 = rev_list(head2);

    struct node* newListHead = NULL;
    struct node* temp = NULL;
    int carry = 0;

    while (head1 != NULL || head2 != NULL || carry != 0) {
        int sum = carry;
        if (head1 != NULL) {
            sum += head1->data;
            head1 = head1->next;
        }
        if (head2 != NULL) {
            sum += head2->data;
            head2 = head2->next;
        }

        struct node* n = (struct node*)malloc(sizeof(struct node));
        n->data = sum % 10;
        n->next = NULL;

        carry = sum / 10;

        if (newListHead == NULL) {
            newListHead = temp = n;
        } else {
            temp->next = n;
            temp = n;
        }
    }

    return rev_list(newListHead);
}

struct node* subtract_lists(char* str1, char* str2) {
    struct node* head1 = create(str1);
    struct node* head2 = create(str2);
    struct node* result = NULL;
    struct node* temp = NULL;
    int borrow = 0;

    head1 = rev_list(head1);
    head2 = rev_list(head2);

    while (head1 != NULL || head2 != NULL) {
        int data1 = (head1 != NULL) ? head1->data : 0;
        int data2 = (head2 != NULL) ? head2->data : 0;

        int diff = data1 - data2 - borrow;

        if (diff < 0) {
            borrow = 1;
            diff += 10;
        } else {
            borrow = 0;
        }

        struct node* new_node = (struct node*)malloc(sizeof(struct node));
        new_node->data = diff;
        new_node->next = NULL;

        if (result == NULL) {
            result = temp = new_node;
        } else {
            temp->next = new_node;
            temp = new_node;
        }

        if (head1 != NULL) {
            head1 = head1->next;
        }
        if (head2 != NULL) {
            head2 = head2->next;
        }
    }

    return rev_list(result);
}

void display_list(struct node* head) {
    struct node* current = head;
    while (current != NULL) {
        printf("%d ", current->data);
        current = current->next;
    }
    printf("\n");
}
struct node* divide_lists(struct node* dividend, struct node* divisor) {
    struct node* result = NULL;
    struct node* temp = NULL;
    int quotient = 0;

    while (dividend != NULL) {
        int dividend_value = dividend->data;
        int divisor_value = (divisor != NULL) ? divisor->data : 0;

        if (divisor_value == 0) {
        }

        if (dividend_value < divisor_value) {
            struct node* zero = (struct node*)malloc(sizeof(struct node));
            zero->data = 0;
            zero->next = NULL;

            if (result == NULL) {
                result = temp = zero;
            } else {
                temp->next = zero;
                temp = zero;
            }

            dividend = dividend->next;
        } else {
            int current_quotient = dividend_value / divisor_value;
            quotient = quotient * 10 + current_quotient;

            struct node* q = (struct node*)malloc(sizeof(struct node));
            q->data = current_quotient;
            q->next = NULL;

            if (result == NULL) {
                result = temp = q;
            } else {
                temp->next = q;
                temp = q;
            }


            int remainder = dividend_value % divisor_value;
            struct node* r = (struct node*)malloc(sizeof(struct node));
            r->data = remainder;
            r->next = NULL;

            if (dividend->next == NULL && remainder == 0) {
                free(r); 
            } else {
                dividend->data = remainder;
                r->next = dividend->next;
                dividend->next = r;
            }
        }
    }

    return result;
}
struct node* modulo_lists(struct node* dividend, struct node* divisor) {
    struct node* result = NULL;
    struct node* temp = NULL;

    while (dividend != NULL) {
        int dividend_value = dividend->data;
        int divisor_value = (divisor != NULL) ? divisor->data : 0;

        if (divisor_value == 0) {
    
        }

        int remainder = dividend_value % divisor_value;

        struct node* r = (struct node*)malloc(sizeof(struct node));
        r->data = remainder;
        r->next = NULL;

        if (result == NULL) {
            result = temp = r;
        } else {
            temp->next = r;
            temp = r;
        }
        dividend = dividend->next;
    }

    return result;
}



