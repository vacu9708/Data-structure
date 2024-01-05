### DisjointSet.h
~~~c++
#ifndef DISJOINTSET_H
#define DISJOINTSET_H

#include <stdio.h>
#include <stdlib.h>

typedef struct tagDisjointSet
{
    struct tagDisjointSet *Parent;
    void *Data;
} DisjointSet;

void DS_UnionSet(DisjointSet *Set1, DisjointSet *Set2);
DisjointSet *DS_FindSet(DisjointSet *Set);
DisjointSet *DS_MakeSet(void *NewData);
void DS_DestroySet(DisjointSet *Set);

#endif
~~~
### DisjointSet.c
~~~c++
#include "DisjointSet.h"

void DS_UnionSet(DisjointSet* Set1, DisjointSet* Set2) //  Set the new parent of the set
{
    Set2 = DS_FindSet(Set2);
    Set2->Parent = Set1;
}

DisjointSet* DS_FindSet(DisjointSet* Set) // Return the parent of the set
{
    while (Set->Parent != NULL)
        Set = Set->Parent;
    return Set;
}

DisjointSet* DS_MakeSet(void* NewData)
{
    DisjointSet* NewNode = (DisjointSet*)malloc(sizeof(DisjointSet));
    NewNode->Parent = NULL;
    NewNode->Data = NewData;
    return NewNode;
}

void DS_DestroySet(DisjointSet* Set)
{
    free(Set);
}
~~~
### Test_DisjointSet.c
~~~c++
#include <stdio.h>
#include "DisjointSet.h"

int main(void){
    int a=1, b=2, c=3, d=4;
    DisjointSet* Set1 = DS_MakeSet(&a);
    DisjointSet* Set2 = DS_MakeSet(&b);
    DisjointSet* Set3 = DS_MakeSet(&c);
    DisjointSet* Set4 = DS_MakeSet(&d);

    printf("Set1 == Set2 : %d\n", DS_FindSet(Set1) == DS_FindSet(Set2));

    DS_UnionSet(Set1, Set2);
    printf("Set1 == Set2 : %d\n", DS_FindSet(Set1) == DS_FindSet(Set2));

    DS_UnionSet(Set2, Set3);
    DS_UnionSet(Set3, Set4);
    printf("Set2 == Set4 : %d\n", DS_FindSet(Set2) == DS_FindSet(Set4));
}
~~~

### Result
![image](https://github.com/vacu9708/Data-structure/assets/67142421/10fefce2-cb62-433a-95ca-6b17986263f5)
