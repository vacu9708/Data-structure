# Linked list
A linked list

~~~C++
#include <iostream>
#include <string>
using namespace std;

class my_SLL {
public:
	class Node {
	public:
		int data;
		Node* next = NULL;
	};

	Node* front = 0;
	Node* rear = 0;

	Node* new_node(int data) {
		Node* node = new Node;
		node->data = data;
		return node;
	}

	void push_front(int data) {
		if (front == NULL) { // If there are no nodes
			front = rear = new_node(data);
			return;
		}

		Node* prev_front = front;
		front = new_node(data);
		front->next = prev_front;
	}

	void push_back(int data) {
		if (front == NULL) { // If there are no nodes
			front = rear = new_node(data);
			return;
		}

		rear->next = new_node(data);
		rear = rear->next;
	}

	void insert(int index, int data) {
		Node* cralwer = front, * prev_cralwer = NULL;

		if (front == NULL) {
			printf("Can't insert : empty list");
			return;
		}

		for (int i = 0; i < index; i++) { // Go to the node to insert
			prev_cralwer = cralwer;
			cralwer = cralwer->next;

			if (cralwer == NULL) {
				printf("Can't insert : out of range\n");
				return;
			}
		}

		if (cralwer == front) { // When inserting at front
			front = new_node(data);
			front->next = cralwer;
		}
		else {
			prev_cralwer->next = new_node(data);
			prev_cralwer->next->next = cralwer;
		}
	}

	void delete_node(int index) {
		Node* cralwer = front, * prev_cralwer = NULL;

		if (front == NULL) {
			printf("Can't delete : empty list");
			return;
		}

		for (int i = 0; i < index; i++) { // Go to the node to delete
			prev_cralwer = cralwer;
			cralwer = cralwer->next;

			if (cralwer == NULL) {
				printf("Can't delete : out of range\n");
				return;
			}
		}

		if (cralwer == front) { // When deleting front
			front = front->next;
			delete cralwer;
		}
		else {
			prev_cralwer->next = cralwer->next;
			delete cralwer;
		}
	}

	string at(int index) {
		Node* cralwer = front; // Put the address of front into cralwer

		if (front == NULL)
			return "Can't get the data : empty list";

		for (int i = 0; i < index; i++) { // Go to the node to index
			cralwer = cralwer->next;

			if (cralwer == NULL)
				return "Can't get the data : out of range";
		}

		return to_string(cralwer->data);
	}

	string back() {
		if (rear == 0)
			return "Can't get the data : empty list";

		return to_string(rear->data);
	}

	void print_list() {
		Node* cralwer = front;
		while (cralwer != NULL) {
			cout << cralwer->data << " ";
			cralwer = cralwer->next;
		}
		cout << "\n";
	}
};

int main(void) {
	my_SLL list1;

	cout << list1.at(0) << "\n";

	for (int i = 0; i <= 3; i++)
		list1.push_back(i);
	for (int i = 4; i <= 7; i++)
		list1.push_front(i);

	printf("Printing list : "); list1.print_list();

	short index_to_delete = 1;
	list1.delete_node(index_to_delete);
	printf("After deleting node (%d) : ", index_to_delete); list1.print_list();

	short index_to_insert = 1;
	list1.insert(index_to_insert, 9);
	printf("After deleting node (%d) : ", index_to_insert); list1.print_list();

	cout << list1.at(0) << "\n";
	cout << list1.back() << "\n";
}
~~~

## Result
![Untitled](https://user-images.githubusercontent.com/67142421/148793828-87cbd907-6457-47e0-90a4-6ca680a343e7.png)
