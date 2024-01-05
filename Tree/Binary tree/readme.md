
# Binary tree
BinaryTree.h
~~~c++
#ifndef BINARY_TREE_H
#define BINARY_TREE_H

#include <stdio.h>
#include <stdlib.h>

typedef char ElementType;

typedef struct tagSBTNode
{
    struct tagSBTNode* Left;
    struct tagSBTNode* Right;

    ElementType Data;
} SBTNode;

SBTNode* SBT_CreateNode(ElementType NewData);
void SBT_DestroyNode(SBTNode* Node);
void SBT_DestroyTree(SBTNode* Root);

void SBT_PreorderPrintTree(SBTNode* Node);
void SBT_InorderPrintTree(SBTNode* Node);
void SBT_PostorderPrintTree(SBTNode* Node);

#endif
~~~
BinaryTree.c
~~~c++
#include "BinaryTree.h"


SBTNode* SBT_CreateNode(ElementType NewData)
{
    SBTNode* NewNode = (SBTNode*)malloc(sizeof(SBTNode));

    NewNode->Left = NULL;
    NewNode->Right = NULL;
    NewNode->Data = NewData;

    return NewNode;
}

void SBT_DestroyNode(SBTNode* Node)
{
    free(Node);
}

void SBT_DestroyTree(SBTNode* Root)
{
    if (Root == NULL)
        return;

    SBT_DestroyTree(Root->Left);
    SBT_DestroyTree(Root->Right);

    SBT_DestroyNode(Root);
}

void SBT_PreorderPrintTree(SBTNode* Node)
{
    if (Node == NULL)
        return;

    printf("%c ", Node->Data);
    SBT_PreorderPrintTree(Node->Left);
    SBT_PreorderPrintTree(Node->Right);
}

void SBT_InorderPrintTree(SBTNode* Node)
{
    if (Node == NULL)
        return;

    SBT_InorderPrintTree(Node->Left);
    printf("%c ", Node->Data);
    SBT_InorderPrintTree(Node->Right);
}

void SBT_PostorderPrintTree(SBTNode* Node)
{
    if (Node == NULL)
        return;

    SBT_PostorderPrintTree(Node->Left);
    SBT_PostorderPrintTree(Node->Right);
    printf("%c ", Node->Data);
}
~~~
Test_BinaryTree.c
~~~c++
#include "BinaryTree.h"

int main(void)
{
    SBTNode* A = SBT_CreateNode('A');
    SBTNode* B = SBT_CreateNode('B');
    SBTNode* C = SBT_CreateNode('C');
    SBTNode* D = SBT_CreateNode('D');
    SBTNode* E = SBT_CreateNode('E');
    SBTNode* F = SBT_CreateNode('F');
    SBTNode* G = SBT_CreateNode('G');

    A->Left = B;
    B->Left = C;
    B->Right = D;

    A->Right = E;
    E->Left = F;
    E->Right = G;

    printf("Preorder...\n");
    SBT_PreorderPrintTree(A);
    printf("\n\n");

    printf("Inorder...\n");
    SBT_InorderPrintTree(A);
    printf("\n\n");

    printf("Postorder...\n");
    SBT_PostorderPrintTree(A);
    printf("\n\n");

    SBT_DestroyTree(A);

    return 0;
}
~~~
## Result
![image](https://github.com/vacu9708/Data-structure/assets/67142421/a7fc7237-4103-46a3-b0c3-1115a8188fa4)

![image](https://github.com/vacu9708/Data-structure/assets/67142421/dba0be37-1782-44a1-b0fe-b351cf3fa039)
