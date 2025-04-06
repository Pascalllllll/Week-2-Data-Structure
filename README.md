## Week-2-Linked-List

In Week 1 of the course, we focused on introducing the basics of C programming and how to manage data effectively using structures and functions.

---

### 🧩 Code Exercises

#### 🔹 Problem 1: *Check Unique-Letter Anagrams Using Linked Lists*
*Description:*  
Write a C program that:
- Reads two words from user input.
- Stores each character of the words in a singly linked list.
- Removes duplicate characters from each word so that only unique characters remain.
- Checks whether the two words are anagrams based on their unique letters (i.e., both words contain the exact same set of letters, regardless of order or repetition).

Uses the following functions:
- `getPhrase()` – Reads a word and stores each character into a linked list.
- `makeUnique()` – Creates a new list that only contains unique characters.
- `areAnagrams()` – Compares two lists of unique characters to check if they are anagrams.
- `deleteNode()` – Deletes a specific node from a linked list.

*Solution:*  
```c
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

typedef struct node {
    char ch;
    struct node *next;
} Node, *NodePtr;

NodePtr getPhrase();
NodePtr makeUnique(NodePtr);
int areAnagrams(NodePtr, NodePtr);
void deleteNode(NodePtr*, NodePtr);

int main() {
    NodePtr phrase1, phrase2;
    printf("Ketik kata pertama: ");
    phrase1 = getPhrase();
    printf("Ketik kata kedua: ");
    phrase2 = getPhrase();

    while (phrase1 != NULL && phrase2 != NULL) {
        phrase1 = makeUnique(phrase1);
        phrase2 = makeUnique(phrase2);
        
        if (areAnagrams(phrase1, phrase2)) 
            printf("Kata-kata tersebut adalah anagram\n");
        else 
            printf("Kata-kata tersebut bukan anagram\n");

        printf("Ketik kata pertama: ");
        phrase1 = getPhrase();
        printf("Ketik kata kedua: ");
        phrase2 = getPhrase();
    }
    return 0;
}

NodePtr getPhrase() {
    NodePtr top = NULL, last = NULL, np;
    char c = getchar();
    while (c != '\n') {
        np = (NodePtr) malloc(sizeof(Node));
        np->ch = c;
        np->next = NULL;
        if (top == NULL) 
            top = np;
        else 
            last->next = np;
        last = np;
        c = getchar();
    }
    return top;
}

NodePtr makeUnique(NodePtr top) {
    NodePtr unique = NULL, np, current, prev;
    while (top != NULL) {
        current = unique;
        prev = NULL;
        while (current != NULL && current->ch != top->ch) {
            prev = current;
            current = current->next;
        }
        if (current == NULL) {
            np = (NodePtr) malloc(sizeof(Node));
            np->ch = top->ch;
            np->next = unique;
            unique = np;
        }
        top = top->next;
    }
    return unique;
}

int areAnagrams(NodePtr w1, NodePtr w2) {
    NodePtr temp1 = w1, temp2, prev;
    while (temp1 != NULL) {
        temp2 = w2;
        prev = NULL;
        while (temp2 != NULL) {
            if (temp1->ch == temp2->ch) {
                deleteNode(&w2, prev);
                break;
            }
            prev = temp2;
            temp2 = temp2->next;
        }
        if (temp2 == NULL) return 0;
        temp1 = temp1->next;
    }
    return (w2 == NULL) ? 1 : 0;
}

void deleteNode(NodePtr* head, NodePtr prev) {
    NodePtr temp;
    if (prev == NULL) {
        temp = *head;
        *head = (*head)->next;
    } else {
        temp = prev->next;
        prev->next = temp->next;
    }
    free(temp);
}

/*
Enter number of kiosks (max 20): 2

Kiosk #1
Enter kiosk name: Donut Bliss
Enter revenue: 100000
Enter expenses: 45000

Kiosk #2
Enter kiosk name: Juice Hub
Enter revenue: 85000
Enter expenses: 60000

Bazaar Profit Report:
Kiosk Name          Revenue    Expenses   Profit    
Donut Bliss         100000.00  45000.00   55000.00  
Juice Hub           85000.00   60000.00   25000.00  
*/
```

#### 🔹 Problem 2: *Circle Counting Game*
*Description:*  
Create a program that simulates a counting game among `n` children standing in a circle. Starting from the first child, count up to `m`, and eliminate the child at position `m`. The counting continues from the next child, and the process repeats until only one child remains. Display the number of the child who wins the game..

The program uses a circular singly linked list to represent the children. Implement the following functions:
- `buatNode(int n)` – Creates a new node representing a child.
- `tautkanSirkular(int n)` – Builds a circular linked list of n children.
- `mainkanGame(NodePtr pertama, int x, int m)` – Simulates the elimination process for x rounds (where x = n - 1) until one child remains.

*Solution:*  
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node {
    int num;
    struct node *next;
} Node, *NodePtr;

NodePtr buatNode(int n);
NodePtr tautkanSirkular(int n);
NodePtr mainkanGame(NodePtr pertama, int x, int m);

int main() {
    NodePtr curr;
    int n, m;
    do {
        printf("Masukkan jumlah anak dan panjang hitungan: ");
        scanf("%d %d", &n, &m);
    } while (n < 1 || m < 1);
    curr = tautkanSirkular(n);
    curr = mainkanGame(curr, n-1, m);
    printf("Anak yang menang: %d\n", curr->num);
    return 0;
}

NodePtr buatNode(int n) {
    NodePtr np = (NodePtr) malloc(sizeof(Node));
    np->num = n;
    np->next = NULL;
    return np;
}

NodePtr tautkanSirkular(int n) {
    NodePtr pertama, np;
    pertama = np = buatNode(1);
    for (int h = 2; h <= n; h++) {
        np->next = buatNode(h);
        np = np->next;
    }
    np->next = pertama;
    return pertama;
}

NodePtr mainkanGame(NodePtr pertama, int x, int m) {
    NodePtr prev, curr = pertama;
    for (int h = 1; h <= x; h++) {
        for (int c = 1; c < m; c++) {
            prev = curr;
            curr = curr->next;
        }
        prev->next = curr->next;
        free(curr);
        curr = prev->next;
    }
    return curr;
}
/*
Input:
Enter number of children and counting length: 5 3

Output:
The winning child is: 4
*/
```

#### 🔹 Problem 3: *Add Two Numbers*
*Description:*  
Write a program that adds two non-negative integers represented as linked lists. Each digit of the numbers is stored in reverse order, with each node containing a single digit. The function should return the sum as a new linked list in the same reverse order format.

You must implement the following:
- `buatNode(int val)` – Creates a new node with a given value.
- `tambahDuaAngka(struct ListNode* l1, struct ListNode* l2)` – Adds two numbers represented by linked lists and returns the result as a linked list.
- `cetakList()` and `bebasList()` – For printing and cleaning up the linked list respectively.

*Solution:*
```c
#include <stdio.h>
#include <stdlib.h>

struct ListNode {
    int val;
    struct ListNode *next;
};

struct ListNode* buatNode(int val) {
    struct ListNode* nodeBaru = (struct ListNode*)malloc(sizeof(struct ListNode));
    nodeBaru->val = val;
    nodeBaru->next = NULL;
    return nodeBaru;
}

struct ListNode* tambahDuaAngka(struct ListNode* l1, struct ListNode* l2) {
    struct ListNode* kepalaDummy = buatNode(0);
    struct ListNode* p = l1, *q = l2, *current = kepalaDummy;
    int carry = 0;
    
    while (p != NULL || q != NULL) {
        int x = (p != NULL) ? p->val : 0;
        int y = (q != NULL) ? q->val : 0;
        int sum = carry + x + y;
        carry = sum / 10;
        current->next = buatNode(sum % 10);
        current = current->next;
        if (p != NULL) p = p->next;
        if (q != NULL) q = q->next;
    }
    
    if (carry > 0) {
        current->next = buatNode(carry);
    }
    
    struct ListNode* hasil = kepalaDummy->next;
    free(kepalaDummy);
    return hasil;
}

void cetakList(struct ListNode* node) {
    while (node != NULL) {
        printf("%d", node->val);
        if (node->next != NULL) printf(" -> ");
        node = node->next;
    }
    printf("\n");
}

void bebasList(struct ListNode* node) {
    while (node != NULL) {
        struct ListNode* temp = node;
        node = node->next;
        free(temp);
    }
}

int main() {
    struct ListNode* l1 = buatNode(2);
    l1->next = buatNode(4);
    l1->next->next = buatNode(3);

    struct ListNode* l2 = buatNode(5);
    l2->next = buatNode(6);
    l2->next->next = buatNode(4);

    struct ListNode* hasil = tambahDuaAngka(l1, l2);

    cetakList(hasil);

    bebasList(l1);
    bebasList(l2);
    bebasList(hasil);

    return 0;
}
/*
Input:
l1 = 2 -> 4 -> 3  (represents 342)
l2 = 5 -> 6 -> 4  (represents 465)

Output:
7 -> 0 -> 8       (represents 807)
*/
```
