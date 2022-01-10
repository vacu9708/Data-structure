# Queue
**Queue** : First In First Out

~~~C++
// Circular queue
#include <iostream>
#include <string>
using namespace std;

const char QUEUE_LENGTH = 4;

int queue[QUEUE_LENGTH] = { 0, };
int front = 0, rear = 0;

void enQ(int data) {
	if ((rear + 1) % QUEUE_LENGTH == front)
		cout << "The queue is full\n";
	else {
		rear = (rear + 1) % QUEUE_LENGTH; // If it's the last index, go back to index 0, or else index++
		cout << "Enqueue (" << data << ")\n";
		queue[rear] = data;
	}
}

string deQ() {
	if (front == rear)
		return "The queue is empty\n";
	else {
		front = (front + 1) % QUEUE_LENGTH;
		return to_string(queue[front]);
	}
}

void main() {
	enQ(0);
	enQ(1);
	enQ(2);
	enQ(3); // Can't put in more elements because the queue is full
	
	for (int i = 0; i < 4; i++)
		cout << "Dequeue : " << deQ() << "\n";
}
~~~
![Untitled](https://user-images.githubusercontent.com/67142421/148781335-6733cb27-860c-44ba-b39a-5f480d82d9a4.png)
