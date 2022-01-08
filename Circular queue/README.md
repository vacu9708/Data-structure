Queue : First In First Out
-----circular queue(순환 큐)(array에선 deQ마다 모든 원소를 앞으로 당겨야하기 때문에 linear queue를 쓸 수 없다)
~~~C++
#include <iostream>
#include <string>
using namespace std;
#define QUEUE_LENGTH 3

int queue[QUEUE_LENGTH] = { 0, };
int front = 0, rear = 0;

void enQ(int data) {
	if ((rear + 1) % QUEUE_LENGTH == front)
		cout << "The queue is full\n";
	else {
		rear = (rear + 1) % QUEUE_LENGTH; // 마지막 index면 index 0으로 돌아가고, 아니면 index++
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
		enQ(2); // 꽉 차서 못 넣음
		cout << deQ() << "\n";
		cout << deQ() << "\n";
		cout << deQ() << "\n"; // 비어있어서 못 뺌
}
~~~
