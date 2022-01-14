# Queue
**Queue** : First In First Out

![image-91](https://user-images.githubusercontent.com/67142421/148844843-6b484f8d-87a2-4657-bdce-d7589ec942d7.png)

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
	if (front == NULL) { // If the queue is empty, create front
		printf("Put (%d)\n", data);
		front = new Node(data);
		rear = front;
		return;
	}

	printf("Put (%d)\n", data);
	rear->next = new Node(data);
	rear = rear->next;
}

int get() {	
	// Store previous front
	Node* prev_front = front;
	int data = prev_front->data;
	//-----
	front = front->next;
	delete prev_front;
	return data;
}

void main() {
	put(0);
	put(1);
	put(2);

	printf("\n");
	for (int i = 0; i < 3; i++)
		printf("Get (%d)", get());
}
~~~
## Result
![Untitled](https://user-images.githubusercontent.com/67142421/148811614-83fe5009-8aa2-4657-9116-df5999a4fcda.png)

## Circular queue
~~~C++
#include <iostream>
using namespace std;

const char QUEUE_LENGTH = 4; // Actually the size is 3

int queue[QUEUE_LENGTH] = { 0, };
int front = 0, rear = 0;

void put(int data) {
	if ((rear + 1) % QUEUE_LENGTH == front)
		printf("The queue is full\n");
	else {
		printf("Put (%d)\n", data);
		queue[rear] = data;
		rear = (rear + 1) % QUEUE_LENGTH; // If it's the last index, go back to index 0, if not, index++
	}
}

void get() {
	if (front == rear)
		printf("The queue is empty\n");
	else {
		printf("Get (%d)\n", queue[front]);
		front = (front + 1) % QUEUE_LENGTH;
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
![Untitled](https://user-images.githubusercontent.com/67142421/148811780-8cae3043-6f66-4003-a5fc-88f26461aca4.png)

