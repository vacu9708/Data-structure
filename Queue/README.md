# Queue
**Queue** : First In First Out

## Circular queue
~~~C++
// Circular queue
#include <iostream>
using namespace std;

const char QUEUE_LENGTH = 4; // Actually the size is 3

int queue[QUEUE_LENGTH] = { 0, };
int front = 0, rear = 0;

void put(int data) {
	if ((rear + 1) % QUEUE_LENGTH == front)
		printf("The queue is full\n");
	else {
		rear = (rear + 1) % QUEUE_LENGTH; // If it's the last index, go back to index 0, or else index++
		printf("Put (%d)\n", data);
		queue[rear] = data;
	}
}

void get() {
	if (front == rear)
		printf("The queue is empty\n");
	else {
		front = (front + 1) % QUEUE_LENGTH;
		printf("Get (%d)\n", queue[front]);
	}
}

void main() {
	put(0);
	put(1);
	put(2);
	put(3); // Can't put in more elements because the queue is full

	for (int i = 0; i < 4; i++)
		get();
}
~~~
## Result
![Untitled](https://user-images.githubusercontent.com/67142421/148781335-6733cb27-860c-44ba-b39a-5f480d82d9a4.png)

## Linked queue
~~~C++
#include <iostream>

using namespace std;

class Node {
public:
	Node* next;
	int data;

	Node(int data) {
		this->data = data;
	}
};

Node* front, * rear;

void put(int data) {
	if (front == NULL) {
		printf("Put (%d)\n", data);
		front = new Node(data);
		rear = front;
		return;
	}

	printf("Put (%d)\n", data);
	rear->next = new Node(data);
	rear = rear->next;
}

void get() {
	if (front == NULL) {
		printf("The queue is empty\n");
		return;
	}

	Node* prev_front = front;
	int data = prev_front->data;
	front = front->next;

	delete prev_front;
	printf("Get (%d)\n", data);
}

void main() {
	put(0);
	put(1);
	put(2);

	printf("\n");
	for (int i = 0; i < 4; i++)
		get();
}
~~~
## Result
![Untitled](https://user-images.githubusercontent.com/67142421/148788540-176bc3ed-4744-4f07-8911-c082ee9c83d7.png)

