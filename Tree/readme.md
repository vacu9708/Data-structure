# Left-child right-sibling binary tree
The Left-Child Right-Sibling (LCRS) tree is a way of representing a general tree using a binary tree structure. This representation allows trees with nodes that have an arbitrary number of children to be implemented using binary trees. Here's how it works:
- Left-Child: The left child of a node in the LCRS representation corresponds to the first child of the same node in the original tree.
- Right-Sibling: The right child of a node in the LCRS representation corresponds to the next sibling of the same node in the original tree.

### Test_LCRStree.c
~~~c++
#include "LCRSTree.h"
 
int main( void )
{
    LCRSNode* Root = LCRS_CreateNode('A');
    LCRSNode* B = LCRS_CreateNode('B');
    LCRSNode* C = LCRS_CreateNode('C');
    LCRSNode* D = LCRS_CreateNode('D');
    LCRSNode* E = LCRS_CreateNode('E');
    LCRSNode* F = LCRS_CreateNode('F');
    LCRSNode* G = LCRS_CreateNode('G');
    LCRSNode* H = LCRS_CreateNode('H');
    LCRSNode* I = LCRS_CreateNode('I');
    LCRSNode* J = LCRS_CreateNode('J');
    LCRSNode* K = LCRS_CreateNode('K');

    LCRS_AddChildNode(Root, B);
    LCRS_AddChildNode(B, C);
    LCRS_AddChildNode(B, D);
    LCRS_AddChildNode(D, E);
    LCRS_AddChildNode(D, F);

    LCRS_AddChildNode(Root, G);
    LCRS_AddChildNode(G, H);

    LCRS_AddChildNode(Root, I);
    LCRS_AddChildNode(I, J);
    LCRS_AddChildNode(J, K);

    LCRS_PrintTree(Root, 0);

    LCRS_DestroyTree(Root);
}
~~~

### LCRStree.h
~~~c++
#ifndef LCRS_TREE_H
#define LCRS_TREE_H

#include <stdio.h>
#include <stdlib.h>

typedef char ElementType;

typedef struct tagLCRSNode
{
    struct tagLCRSNode* LeftChild;
    struct tagLCRSNode* RightSibling;

    ElementType Data;
} LCRSNode;

LCRSNode* LCRS_CreateNode(ElementType NewData);
void LCRS_DestroyNode(LCRSNode* Node);
void LCRS_DestroyTree(LCRSNode* Root);

void LCRS_AddChildNode(LCRSNode* ParentNode, LCRSNode* ChildNode);
void LCRS_PrintTree(LCRSNode* Node, int Depth);

#endif
~~~

### LCRStree.c
~~~c++
#include "LCRSTree.h"

LCRSNode* LCRS_CreateNode( ElementType NewData )
{
    LCRSNode* NewNode = (LCRSNode*)malloc(sizeof(LCRSNode));
    
    NewNode->LeftChild = NULL;
    NewNode->RightSibling = NULL;
    NewNode->Data = NewData;
    
    return NewNode;
}

void LCRS_DestroyNode( LCRSNode* Node )
{
    free( Node );
}

void LCRS_DestroyTree( LCRSNode* Root )
{
    if ( Root->RightSibling != NULL )
        LCRS_DestroyTree( Root->RightSibling );
    
    if ( Root->LeftChild != NULL )
        LCRS_DestroyTree( Root->LeftChild );
    
    Root->LeftChild = NULL;
    Root->RightSibling = NULL;
    
    LCRS_DestroyNode( Root );
}

void LCRS_AddChildNode( LCRSNode* ParentNode, LCRSNode* ChildNode )
{
    if ( ParentNode->LeftChild == NULL )
        ParentNode->LeftChild = ChildNode;
    else
    {
        LCRSNode* TempNode = ParentNode->LeftChild;
        
        while ( TempNode->RightSibling != NULL )
            TempNode = TempNode->RightSibling;
        
        TempNode->RightSibling = ChildNode;
    }
}

void LCRS_PrintTree( LCRSNode* Node, int Depth )
{
    int i = 0;
    for ( i=0; i<Depth-1; i++ )
        printf(" ");

    if ( Depth > 0 )
        printf("+--");

    printf("%c\n", Node->Data);

    if ( Node->LeftChild != NULL )
        LCRS_PrintTree( Node->LeftChild, Depth + 1 );

    if ( Node->RightSibling != NULL )
        LCRS_PrintTree( Node->RightSibling, Depth );
}
~~~
## Result
![image](https://github.com/vacu9708/Data-structure/assets/67142421/8781d876-c663-490c-9639-d452e730963b)

![image](https://github.com/vacu9708/Data-structure/assets/67142421/4b945bde-e532-489c-8bd7-b5dcc739c597)

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
