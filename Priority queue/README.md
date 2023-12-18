# Priority queue
>A priority queue is a collection of data elements similar to a regular queue in which an element with the highest priority leaves first.<br>
>Elements with higher priority are placed before their children<br>
>The element with the highest priority is always at the beginning of the queue.
>Normally ***Heap*** is used to implement a priority queue.

## Example of Min heap
![Min-heap](https://user-images.githubusercontent.com/67142421/149667051-b130801a-328e-4656-9b11-3e5cb98bf787.png)

## Why is heap used instead of linked list to implement priority queue?
* **Heap**
  * heap_push() : **O(logn)** [Upward-heapifying takes **O(logn)** as it is the reverse process of the binary search]
  * heap_pop(): **O(logn)** [Downward-heapifying takes **O(logn)** in the same way as the binary search]
* **Linked list**
  * insert() : **O(n)** [It takes **O(n)** to compare the priority of all the elements before inserting)]
  * get() : **O(1)** [Because all that needs to be done is just get the front data]
>As stated above, priority queue using heap is more stable than linked list in terms of time complexity while priority queue using linked list is unstable.

## Caution
Before heapifying, the array must first be transformed into a heap tree by lifting up elements with higher priority, which takes O(NlogN)<br>

## Priority queue using heap
~~~C++
#include <iostream>
#include <vector>
using namespace std;

void print_array(vector<int>& heap_tree) {
    int tree_size = heap_tree.size();
        for (int i = 0; i < tree_size; ++i)
            cout << heap_tree[i] << " ";
    cout << "\n";
}

void downward_heapify(vector<int>& heap_tree, int curr) {
    int left = curr*2+1, right = curr*2+2; // children
    int least = curr;
    // If a child is smaller than its parent, swap them
    if (left < heap_tree.size() && heap_tree[left] < heap_tree[least]) // becomes max_heapify by changing the inequality 
        least=left;
    if (right < heap_tree.size() && heap_tree[right] < heap_tree[least])
        least=right;
    if (least != curr) {
        swap(heap_tree[least], heap_tree[curr]);
        downward_heapify(heap_tree, least);
    }
}

// void heapify_all(vector<int>& heap_tree){
//     for (int i = (heap_tree.size()-2)/2; i >= 0; i--) // from last child's parent
//         downward_heapify(heap_tree, i);
// }

void upward_heapify(vector<int>& heap_tree, int curr){
    int parent=(curr-1)/2;
    // Return if out of range or the parent is smaller than the current node
    if(!(curr>=1 || heap_tree[curr]<heap_tree[parent]))
        return;
    swap(heap_tree[curr], heap_tree[parent]);
    upward_heapify(heap_tree, parent);
}

void heap_push(vector<int>& heap_tree, int data) {
    heap_tree.push_back(data);
    upward_heapify(heap_tree, heap_tree.size() - 1);
}

int heap_pop(vector<int>& heap_tree) {
    int highest_priority = heap_tree[0];
    // Move the the last node to the place to be deleted for a better time complexity in the vector
    heap_tree[0] = heap_tree[heap_tree.size() - 1];
    heap_tree.pop_back();
    downward_heapify(heap_tree, 0);
    return highest_priority;
}

int main() {
    vector<int> heap_tree;
    for (int i = 5; i > 0; i--)
        heap_push(heap_tree, i);
    printf("Heap tree : "); print_array(heap_tree);
    for(int i=0; i<5; i++){
        printf("Get (%d)\n", heap_pop(heap_tree));
        printf("Heap tree : "); print_array(heap_tree);
    }
}
~~~
## Output
![image](https://github.com/vacu9708/Data-structure/assets/67142421/d142e6e6-9a2a-498d-b23d-df6a5ca902a9)

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

int get() {
	int highest_priority = front->data;
	Node* node_to_remove = front;
	front = front->next;
	delete node_to_remove;
	return highest_priority;
}

void delete_node(int target) {
	Node* crawler = front, * prev_crawler = NULL;

	while (true) {
		if (crawler->data == target)
			break;

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

	for (int i = 0; i < 4; i++)
		printf("Get (%d)\n", get());
}
~~~
## Output
![Untitled](https://user-images.githubusercontent.com/67142421/148811152-0abb0d7b-68ea-4e46-b16b-04a0d3fd97cf.png)

