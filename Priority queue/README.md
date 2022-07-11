# Priority queue
>A priority queue is a collection of data elements similar to a regular queue in which an element with the highest priority leaves first.<br>
>In a priority queue, elements with a high priority are served before elements with a low priority.<br>
>An element with the highest priority is always at the beginning of the queue.
>Normally ***Heap*** is used to implement a priority queue.

## An example of Min heap
![Min-heap](https://user-images.githubusercontent.com/67142421/149667051-b130801a-328e-4656-9b11-3e5cb98bf787.png)

## Why is heap used instead of linked list to implement priority queue?
* **Heap**
  * insertion() : **O(logn)** [Heapifying is needed after insertion(), which takes **O(logn)**]
  * get() : **O(logn)** [Heapifying is needed after get(), which takes **O(logn)**]
* **Linked list**
  * insertion() : **O(n)** [It takes **O(n)** to compare the priority of all the elements before inserting)]
  * get() : **O(1)** [Because all that needs to be done is just get the front data]
>As stated above, priority queue using heap is more stable than linked list in terms of time complexity while priority queue using linked list is unstable.

## Priority queue using heap
~~~C++
#include <iostream>
#include <vector>
using namespace std;

vector<int> heap_tree;

void heapify(int parent) { // Function to max-heapify the tree
    int left = 2 * parent + 1, right = 2 * parent + 2;
    int tree_size = heap_tree.size();
    // If child is larger than parent, swap parent and child
    if (left < tree_size && heap_tree[left] > heap_tree[parent]) { // Changing the inequality sign makes it min-heapify
        swap(heap_tree[left], heap_tree[parent]);
        heapify(left);
    }
    if (right < tree_size && heap_tree[right] > heap_tree[parent]) {
        swap(heap_tree[right], heap_tree[parent]);
        heapify(right);
    }
}


void insert(int new_num) { // Function to insert an element into the tree
    int tree_size = heap_tree.size();
    heap_tree.push_back(new_num);
    for (int i = tree_size / 2 - 1; i >= 0; i--) // Max-heapify from the parent of last element
        heapify(i);
}

int get(vector<int>& heap_tree) {
    int tree_size = heap_tree.size();
    int highest_priority = heap_tree[0];

    // Put the last element of heap tree into the first index
    int tree_size = heap_tree.size();
    heap_tree[0] = heap_tree[tree_size - 1];
    heap_tree.pop_back();
    //-----
    for (int i = tree_size / 2 - 1; i >= 0; i--) // Max-heapify
        heapify(i);
    return highest_priority;
}

void deleteNode(vector<int>& heap_tree, int target) { // Function to delete an element from the tree
    int tree_size = heap_tree.size();
    for (int i = 0; i < tree_size; i++)
        if (heap_tree[i] == target) { // If target was found
            printf("Delete (%d)\n", target);
            heap_tree[i] = heap_tree[tree_size - 1]; // Change it to the last node
            heap_tree.pop_back(); // Delete the last node
            for (int i = tree_size / 2 - 1; i >= 0; i--) // Max-heapify
                heapify(i);
            return;
        }
    printf("Target (%d) not found\n", target);
}

void print_array(vector<int>& heap_tree) {
    int tree_size = heap_tree.size();
        for (int i = 0; i < tree_size; ++i)
            cout << heap_tree[i] << " ";
    cout << "\n";
}

int main() {
    for (int i = 1; i <= 9; i += 2)
        insert(i);
    printf("Heap tree : "); print_array(heap_tree);

    deleteNode(heap_tree, 7);
    printf("Heap tree : "); print_array(heap_tree);

    printf("Get (%d)\n", get(heap_tree));
    printf("Heap tree : "); print_array(heap_tree);
}
~~~
## Output
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

