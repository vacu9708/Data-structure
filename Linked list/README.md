# Linked list
>A linked list is a linear container of data elements whose order is not given by their physical placement in memory unlike *Array*

![1_m11VTAK3YJgRemmfBI_2uw](https://user-images.githubusercontent.com/67142421/148844977-d81a8d5a-4cbc-4ed4-b4bc-5a2254f72203.png)

## Array VS Linked list
* **Array**
  * A collection of data elements that is stored contiguously and consecutively.
  * Memory is allocated during the compile time (Static memory allocation), so the size of the array must be specified at the time of array declaration.
  * Elements can be accessed randomly. The wanted index is just picked immedately, in other words, **O(1) time complexity**
  * Insertion and Deletion operations are costlier since the memory locations are consecutive and fixed. **O(n) time complexity** because in the worst case, all the elements 	 might have to be pushed back.

* **Linked list**
  * A collection of data elements that is stored randomly and in which each element is connected using pointers
  * Elements can't be accessed randomly. They have to be accessed sequentially in order. **O(n) time complexity** in the worst case which is going through the list to access the last element sequentially.
  * Insertion and Deletion operations are fast since a new node can be connected to the list anywhere in memory and a node can be easily deleted by just changing the link using pointers. **O(1) time complexity**

~~~C++
#include <iostream>
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

		for (int i = 0; i < index; i++) { // Go to the node to insert
			prev_cralwer = cralwer;
			cralwer = cralwer->next;
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

		for (int i = 0; i < index; i++) { // Go to the node to delete
			prev_cralwer = cralwer;
			cralwer = cralwer->next;
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

	int at(int index) {
		Node* cralwer = front; // Put the address of front into cralwer

		for (int i = 0; i < index; i++) { // Go to the node to index
			cralwer = cralwer->next;
		}

		return cralwer->data;
	}

	int back() {
		return rear->data;
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

	for (int i = 0; i <= 3; i++)
		list1.push_back(i);
	for (int i = 4; i <= 7; i++)
		list1.push_front(i);

	printf("Printing list : "); list1.print_list();

	short index_to_delete = 1;
	list1.delete_node(index_to_delete);
	printf("After deleting index (%d) : ", index_to_delete); list1.print_list();

	short index_to_insert = 1;
	list1.insert(index_to_insert, 9);
	printf("After deleting index (%d) : ", index_to_insert); list1.print_list();

	cout << list1.at(0) << "\n";
	cout << list1.back() << "\n";
}
~~~

## Output
![Untitled](https://user-images.githubusercontent.com/67142421/148810452-62986203-1c65-4737-94f0-0f9a6020fb0e.png)

## Exceptions handled
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
			if (cralwer->next == NULL) {
				printf("Can't delete : out of range\n");
				return;
			}
		
			prev_cralwer = cralwer;
			cralwer = cralwer->next;
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
			if (cralwer->next == NULL)
				return "Can't get the data : out of range";

			cralwer = cralwer->next;		
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
	printf("After deleting index (%d) : ", index_to_delete); list1.print_list();

	short index_to_insert = 1;
	list1.insert(index_to_insert, 9);
	printf("After deleting index (%d) : ", index_to_insert); list1.print_list();

	cout << list1.at(0) << "\n";
	cout << list1.back() << "\n";
}
~~~
## Output
![Untitled](https://user-images.githubusercontent.com/67142421/148794108-cc2cb13a-ee86-4521-81ba-6f2e36a620af.png)
