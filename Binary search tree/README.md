# Binary search tree
>Binary search tree : A binary tree where the left child of a parent is smaller than the parent and the right child is bigger than the parent.
>It can be used to search for a number in **O(log(n))** time, which is faster than linear search that takes **O(n)**.

## How to delete a node that has 2 children in Binary Search tree
>Place either the minimum of the right subtree or the maximum of the left subtree in the position of the node to be deleted

![Deleting a node with 2 children in binary search tree](https://user-images.githubusercontent.com/67142421/148779207-367d1165-eb0b-4e88-9de9-ac6703f663ba.png)

~~~C++
#include <iostream>
using namespace std;

struct Node {
	int data = 0;
	Node* left = NULL, * right = NULL;
};
Node* root = NULL;

Node* new_node(int data) {
	Node* temp = new Node;
	temp->data = data;
	//temp->left = temp->right = NULL;
	return temp;
}

Node* insert(Node* node, int data) {
	if (node == NULL) // When a node doesn't exist in the current address, create a node.
		return new_node(data);

	if (data < node->data) // If new node is smaller than parent, go to the left child
		node->left = insert(node->left, data);
	else // If not, go to the right child
		node->right = insert(node->right, data);
	return node; // In order not to return a trash address to other nodes that have been visited.
}

void insert_without_recursion(int data) {
	if (root == NULL) { // If there isn't a root, make a root
		root = new_node(data);
		return;
	}

	Node* crawler = root;
	while (true)
		// If new node is smaller than parent, go to the left child
		if (data < crawler->data)
			if (crawler->left == NULL) { // If the parent has a left child, insert in the left child
				crawler->left = new_node(data);
				return;
			}
			else // If the parent has a left child, go to the left child to get to the destination
				crawler = crawler->left;
		//-----
		// If not smaller than parent, go to the right child
		else
			if (crawler->right == NULL) { // If the parent doesn't have a right child, insert in the right child
				crawler->right = new_node(data);
				return;
			}
			else // If the parent has a right child, go to the right child to get to the destination
				crawler = crawler->right;
		//-----
}

Node* minNode(Node* node) {
	Node* crawler = node;
	while (crawler && crawler->left != NULL) // Find the leftmost leaf to get to the minimum node
		crawler = crawler->left;
	return crawler;
}

Node* delete_node(Node* crawler, int target) { // Does the same function as delete_node() below
	if (crawler == NULL) {
		cout << "Not found\n";
		return crawler; // To prevent read violation and trash address, instead put NULL address
	}

	if (target == crawler->data) { // If target was found
		// If the node has only one child or no child
		if (crawler->left == NULL) {
			Node* temp = crawler->right; // Store the right child of target node to substitute the target node with it
			delete crawler;
			return temp;
		}
		else if (crawler->right == NULL) { // Same process as right above
			Node* temp = crawler->left;
			delete crawler;
			return temp;
		}
		//-----
		// Else if the target has two children
		Node* temp = minNode(crawler->right); // Find the minimum of right subtree
		crawler->data = temp->data; // Place the minimum of right subtree in the position of the node to be deleted
		crawler->right = delete_node(crawler->right, temp->data); // Delete the minimum of right subtree
		//-----
	}
	else if (target < crawler->data) // If target is smaller than parent, go to left child to find the target
		crawler->left = delete_node(crawler->left, target);
	else // Else, same process as right above
		crawler->right = delete_node(crawler->right, target);
	return crawler; // In order not to put a trash address in DFS
}

void delete_node2(Node* crawler, int target) { // Does the same function as delete_node2() above but longer code
	Node* prev = crawler;
	bool prev_left = false;
	while (crawler != NULL) {
		if (target == crawler->data) { // If target was found
			// If the node is with only one child or no child
			if (crawler->left == NULL) {
				Node* temp = crawler->right; // Store the right child of target node to substitute the target node with it
				delete crawler;
				prev_left == true ? prev->left = temp : prev->right = temp;
				return;
			}
			else if (crawler->right == NULL) { // Same process as right above
				Node* temp = crawler->left;
				delete crawler;
				prev_left == true ? prev->left = temp : prev->right = temp;
				return;
			}
			//-----
			// If the node has two children
			Node* temp = minNode(crawler->right); // Find the minimum of right subtree
			crawler->data = temp->data; // Place the minimum of right subtree in the position of the node to be deleted
			delete_node2(crawler->right, temp->data);
			return;
			//-----
		}
		// If target is smaller than parent, go to left child to find the target
		else if (target < crawler->data) {
			prev = crawler;
			crawler = crawler->left;
			prev_left = true;
		}
		//-----
		// Else, same process as right above
		else {
			prev = crawler;
			crawler = crawler->right;
			prev_left = false;
		}
		//-----
	}
	cout << "Not found\n";
}

// In order traversal (ascending order)
void search_ascending_order(Node* crawler) {
	if (crawler == NULL)
		return;

	search_ascending_order(crawler->left);
	cout << crawler->data << " -> ";
	search_ascending_order(crawler->right);
}

void search_descending_order(Node* crawler) {
	if (crawler == NULL)
		return;

	search_descending_order(crawler->right);
	cout << crawler->data << " -> ";
	search_descending_order(crawler->left);
}

void search(int target) {
	Node* crawler = root;

	while (true) {
		if (crawler == NULL) {
			printf("(%d) not found\n", target);
			return;
		}

		if (target == crawler->data) {// If target found, print the target
			printf("(%d) found\n", crawler->data);
			return;
		}
		else if (target < crawler->data) // If target is smaller than parent, search to the left to find the target
			crawler = crawler->left;
		else // Same process as above
			crawler = crawler->right;
	}
}

int main() {
	root = insert(root, 3);
	insert(root, 1);
	insert(root, 7);
	insert(root, 5);
	cout << "Search in ascending order : "; search_ascending_order(root); cout << "\n";
	cout << "Search in descending order : "; search_descending_order(root); cout << "\n";

	short node_to_delete = 3;
	delete_node(root, node_to_delete);
	printf("After deleting (%d) : ", node_to_delete); search_ascending_order(root); cout << "\n";

	search(3);
	search(5);
}
~~~
## Result
<img src="https://user-images.githubusercontent.com/67142421/148778914-a7f42d34-addd-4c75-a6df-3638c62bb195.png">


