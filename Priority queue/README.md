# Priority queue

## Why is heap used instead of linked list to implement priority queue?
* **Heap**
  * insertion() : O(logn)
  * get() : O(logn) [Max-heapify is needed after get() which takes O(logn)]
* **Linked list**
  * insertion() : O(n) [It takes O(n) to compare the priority of all the elements before inserting]
  * get() : O(1)
As stated above, priority queue using heap is more stable than linked list. Priority queue using linked list is unstable in terms of time complexity.

## Priority queue using heap
~~~C++
#include <iostream>
#include <vector>
using namespace std;

void heapify(vector<int>& heap_tree, int parent) { // Function to max-heapify the tree
    int size = heap_tree.size();
    int left = 2 * parent + 1, right = 2 * parent + 2;
    // If child is larger than parent, swap parent and child
    if (left < size && heap_tree[left] > heap_tree[parent]) { // Changing the inequality sign makes it min-heapify
        swap(heap_tree[left], heap_tree[parent]);
        heapify(heap_tree, left);
    }
    if (right < size && heap_tree[right] > heap_tree[parent]) {
        swap(heap_tree[right], heap_tree[parent]);
        heapify(heap_tree, right);
    }
}


void insert(vector<int>& heap_tree, int newNum) { // Function to insert an element into the tree
    heap_tree.push_back(newNum);
    for (int i = heap_tree.size() / 2 - 1; i >= 0; i--) // Max-heapify
        heapify(heap_tree, i);
}

void deleteNode(vector<int>& heap_tree, int target) { // Function to delete an element from the tree
    int size = heap_tree.size();
    for (int i = 0; i < size; i++)
        if (heap_tree[i] == target) { // If target was found
            printf("Delete (%d)\n", target);
            heap_tree[i] = heap_tree[size - 1]; // Change it to the last node
            heap_tree.pop_back(); // Delete the last node
            for (int i = size / 2 - 1; i >= 0; i--) // Max-heapify
                heapify(heap_tree, i);
            break;
        }
}

int get(vector<int>& heap_tree) {
    int highest_priority = heap_tree[0];

    // Put the last element of heap tree into the first index
    int size = heap_tree.size();
    heap_tree[0] = heap_tree[size - 1];
    heap_tree.pop_back();
    //-----
    for (int i = size / 2 - 1; i >= 0; i--) // Max-heapify
        heapify(heap_tree, i);
    return highest_priority;
}

void print_list(vector<int>& heap_tree) {
    for (int i = 0; i < heap_tree.size(); ++i)
        cout << heap_tree[i] << " ";
    cout << "\n";
}

int main() {
    vector<int> heap_tree;
    for (int i = 1; i <= 9; i += 2)
        insert(heap_tree, i);
    printf("Heap tree : "); print_list(heap_tree);

    deleteNode(heap_tree, 7);
    printf("Heap tree : "); print_list(heap_tree);

    printf("Get (%d)\n", get(heap_tree));
    printf("Heap tree : "); print_list(heap_tree);
}
~~~
## Result
![Untitled](https://user-images.githubusercontent.com/67142421/148804359-b3bc1e37-6b7a-44ba-ae3c-5ac311296b27.png)


## Priority queue using linked list
~~~C++
#include <iostream>

using namespace std;

class Node {
public:
	int data;
	Node* next;
};

Node* front;
void insert_in_order(int data) { // In descending order
	Node* new_node = new Node;
	new_node->data = data;
	new_node->next = 0;

	if (front == 0) { // When there are no nodes
		front = new_node;
		return;
	}

	Node* crawler = front;
	Node* prev = 0;

	while (crawler->next != 0 && data < crawler->data) { // Search until the new data is bigger
		prev = crawler;
		crawler = crawler->next;
	}

	if (crawler == front || crawler->next == 0) { // If the crawler is at front or rear
		if (data > crawler->data) { // If the new data is bigger
			new_node->next = crawler; // Put new node on the left of cralwer
			front = new_node;
		}
		else // If the new data is smaller
			crawler->next = new_node; // Put new node on the right of cralwer
		return;
	}

	prev->next = new_node; // Put new node on the left of cralwer
	new_node->next = crawler;
}

void get() {
	if (front == NULL) {
		printf("Can't get : Empty list\n");
		return;
	}

	int highest_priority = front->data;
	Node* node_to_remove = front;
	front = front->next;
	delete node_to_remove;
	printf("Get (%d)\n", highest_priority);
}

void delete_node(int target) {
	Node* crawler = front, * prev_crawler = NULL;

	if (front == NULL) {
		printf("Can't delete : empty list");
		return;
	}

	while(true){
		if (crawler->data == target)
			break;

		if (crawler->next == NULL) {
			printf("Can't delete : out of range\n");
			return;
		}

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

void print_list() {
	Node* crawler = front;
	while (crawler != 0) {
		cout << crawler->data << " ";
		crawler = crawler->next;
	}
	cout << "\n";
}

int main(void) {
	insert_in_order(4);
	insert_in_order(3);
	insert_in_order(5);
	insert_in_order(2);
	insert_in_order(6);
	printf("Print list : "); print_list();

	short data_to_delete = 5;
	printf("Delete (%d)\n", data_to_delete); delete_node(data_to_delete);

	for (int i = 0; i < 5; i++)
		get();
}
~~~
## Result
![Untitled](https://user-images.githubusercontent.com/67142421/148807940-c75376d9-519d-46db-b6ec-ec8eff25fad8.png)

