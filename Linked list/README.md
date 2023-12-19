# Linked list
A linked list is a collection of elements, called nodes, where each node consists of two parts: data and a reference (or link) to the next node in the sequence. Linked lists are used to store and organize data, and they are particularly useful when you need dynamic data storage that can grow or shrink during program execution.

![1_m11VTAK3YJgRemmfBI_2uw](https://user-images.githubusercontent.com/67142421/148844977-d81a8d5a-4cbc-4ed4-b4bc-5a2254f72203.png)

## Array VS Linked list
#### `Array`
A collection of data elements that is stored contiguously and consecutively.
  * Elements are stored contiguously and can be accessed randomly. The wanted index is just picked immedately, in other words, **O(1) time complexity**
  * Insertion and Deletion operations are costlier since the allocated space is fixed. **O(n) time complexity** because in the worst case, all the elements should be moved into a new array.

#### `Linked list`
A collection of data elements stored non-contiguously, with each element connected through pointers.
  * Elements are stored non-contiguously and can't be accessed randomly. They have to be accessed sequentially in order. **O(n) time complexity** to go through the list to access the last element sequentially.
  * Insertion and Deletion operations are fast since inserting or deleting a new node just requires creating or deleting a link respectively, which is **O(1) time complexity** operation.
  * Additional memory is consumed beause nodes have pointers

~~~C++
#include <iostream>
using namespace std;

class my_SLL {
public:
	class Node {
	public:
		int data;
		Node* next = NULL;

		Node(int data, Node* next) {
			this->data = data;
			this->next = next;
		}
	};

	Node* front = 0;
	Node* rear = 0;

	void push_front(int data) {
		if (front == NULL) { // If there are no nodes
			front = rear = new Node(data, 0);
			return;
		}

		front = new Node(data, front);
	}

	void push_back(int data) {
		if (front == NULL) { // If there are no nodes
			front = rear = new Node(data, 0);
			return;
		}

		rear->next = new Node(data, 0);
		rear = rear->next;
	}

	void insert(int index, int data) {
		Node* crawler = front, * prev_crawler = NULL;

		for (int i = 0; i < index; i++) { // Go to the node to insert
			prev_crawler = crawler;
			crawler = crawler->next;
		}

		if (crawler == front) // When inserting at front
			front = new Node(data, crawler);
		else
			prev_crawler->next = new Node(data, crawler);
	}

	void delete_node(int index) {
		Node* crawler = front, * prev_crawler = NULL;

		for (int i = 0; i < index; i++) { // Go to the node to delete
			prev_crawler = crawler;
			crawler = crawler->next;
		}

		if (crawler == front) { // When deleting front
			front = front->next;
			delete crawler;
		}
		else {
			prev_crawler->next = crawler->next;
			delete crawler;
		}
	}

	int at(int index) {
		Node* crawler = front; // Put the address of front into crawler

		for (int i = 0; i < index; i++) // Go to the node to index
			crawler = crawler->next;

		return crawler->data;
	}

	int back() {
		return rear->data;
	}

	void change_value(int index, int desired_value) {
		Node* crawler = front; // Put the address of front into crawler

		for (int i = 0; i < index; i++) // Go to the node to index
			crawler = crawler->next;

		crawler->data = desired_value;
	}

	void print_list() {
		Node* crawler = front;
		while (crawler != NULL) {
			cout << crawler->data << " ";
			crawler = crawler->next;
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

	short index_to_insert = 1, value_to_insert = 9;
	list1.insert(index_to_insert, value_to_insert);
	printf("After inserting %d in index (%d) : ", value_to_insert, index_to_insert); list1.print_list();

	short index_to_change = 1, desired_value = 8;
	list1.change_value(index_to_change, desired_value);
	printf("After changing index (%d) to %d : ", index_to_change, desired_value); list1.print_list();


	printf("At index (0) : ");  cout << list1.at(0) << "\n";
	printf("At the last index : "); cout << list1.back() << "\n";
}
~~~

## Output
![image](https://user-images.githubusercontent.com/67142421/156928240-14739da0-21a3-4403-80c0-3cd7fa3a7fbb.png)
