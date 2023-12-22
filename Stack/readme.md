# Stack
![stack-660x345](https://user-images.githubusercontent.com/67142421/148844511-1d9a04c3-cb47-4609-b94e-1a9a27723e4f.png)

>A stack holds a linear sequence of elements where the element inserted at the last, is the first element to come out, i.e. a stack is based on **Last In, First Out (LIFO)** principle.

~~~C++
#include <iostream>
#include <string>
using namespace std;
#define SIZE 3

int stack[SIZE];
int index = 0; // index = -1으로 시작하는 책도 있다

void push(int data) {
    if (index >= SIZE) {
        cout << "The stack is full\n";
        return;
    }
        
    cout << "Push (" << data << ")\n";
    stack[index++] = data; 
}

string top() {
    if (index <= 0)
        return "The stack is empty\n";
    
    return to_string(stack[--index]);
}


void main() {
    push(0);
    push(1);
    push(2);
    push(3); // Can't push because it's full
    cout << top() << "\n";
    cout << top() << "\n";
    cout << top() << "\n";
    cout << top() << "\n"; // Empty stack
}
~~~
## Output
![Untitled](https://user-images.githubusercontent.com/67142421/148784742-8885e10f-f812-4679-9364-8315f9690ed9.png)
