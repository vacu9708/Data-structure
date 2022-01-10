# Stack
Stack : Last In First Out (undo)
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
![Untitled](https://user-images.githubusercontent.com/67142421/148784742-8885e10f-f812-4679-9364-8315f9690ed9.png)