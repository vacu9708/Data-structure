Test_ExpressionTree.c
~~~c++
#include "ExpressionTree.h"

int main(void)
{
    ETNode* Root = NULL;

    char PostfixExpression[20] = "71*52-/";
    ET_BuildExpressionTree(PostfixExpression, &Root);

    printf("Preorder...\n");
    ET_PreorderPrintTree(Root);
    printf("\n\n");

    printf("Inorder...\n");
    ET_InorderPrintTree(Root);
    printf("\n\n");

    printf("Postorder...\n");
    ET_PostorderPrintTree(Root);
    printf("\n\n");

    printf("Evaluation Result : %f\n", ET_Evaluate(Root));
}
~~~

ExpressionTree.h
~~~c++
#ifndef EXPRESSION_TREE_H
#define EXPRESSION_TREE_H

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef char ElementType;

typedef struct tagETNode {
    char data;
    struct tagETNode* Left;
    struct tagETNode* Right;

    ElementType Data;
} ETNode;

ETNode* ET_CreateNode(char data);
void ET_DestroyNode(ETNode* node);
void ET_DestroyTree(ETNode* root);

void ET_PreorderPrintTree(ETNode* node);
void ET_InorderPrintTree(ETNode* node);
void ET_PostorderPrintTree(ETNode* node);
void ET_BuildExpressionTree(char* postfixExpression, ETNode** node);
double ET_Evaluate(ETNode* tree);

#endif
~~~

ExpressionTree.c
~~~c++
#include "ExpressionTree.h"

ETNode* ET_CreateNode(char NewData)
{
    ETNode* NewNode = (ETNode*)malloc(sizeof(ETNode));

    NewNode->Left = NULL;
    NewNode->Right = NULL;
    NewNode->Data = NewData;

    return NewNode;
}

void ET_DestroyNode(ETNode* Node)
{
    free(Node);
}

void ET_DestroyTree(ETNode* Root)
{
    if (Root == NULL)
        return;

    ET_DestroyTree(Root->Left);
    ET_DestroyTree(Root->Right);

    ET_DestroyNode(Root);
}

void ET_PreorderPrintTree(ETNode* Node)
{
    if (Node == NULL)
        return;

    printf(" %c", Node->Data);
    ET_PreorderPrintTree(Node->Left);
    ET_PreorderPrintTree(Node->Right);
}

void ET_InorderPrintTree(ETNode* Node)
{
    if (Node == NULL)
        return;

    printf("(");
    ET_InorderPrintTree(Node->Left);
    printf(" %c", Node->Data);
    ET_InorderPrintTree(Node->Right);
    printf(")");
}

void ET_PostorderPrintTree(ETNode* Node)
{
    if (Node == NULL)
        return;

    ET_PostorderPrintTree(Node->Left);
    ET_PostorderPrintTree(Node->Right);
    printf(" %c", Node->Data);
}

void ET_BuildExpressionTree(char* PostfixExpression, ETNode** Node)
{
    int len = strlen(PostfixExpression);
    char Token = PostfixExpression[len - 1];

    PostfixExpression[len - 1] = '\0';

    switch (Token)
    {
    case '+':
    case '-':
    case '*':
    case '/':
        (*Node) = ET_CreateNode(Token);
        ET_BuildExpressionTree(PostfixExpression, &(*Node)->Right);
        ET_BuildExpressionTree(PostfixExpression, &(*Node)->Left);
        break;
    default:
        (*Node) = ET_CreateNode(Token);
        break;
    }
}

double ET_Evaluate(ETNode* Tree)
{
    double Left = 0;
    double Right = 0;

    if (Tree == NULL)
        return 0;

    if (Tree->Left == NULL && Tree->Right == NULL)
        return Tree->Data - '0';

    Left = ET_Evaluate(Tree->Left);
    Right = ET_Evaluate(Tree->Right);

    switch (Tree->Data)
    {
    case '+':
        return Left + Right;
    case '-':
        return Left - Right;
    case '*':
        return Left * Right;
    case '/':
        return Left / Right;
    }

    return 0;
}
~~~

Expression<br>
![image](https://github.com/vacu9708/Data-structure/assets/67142421/5918033f-671e-41ee-a3c1-b417bb198b3c)

Result<br>
![image](https://github.com/vacu9708/Data-structure/assets/67142421/7733c742-2667-4a53-b5f2-acd7abe9df9e)
