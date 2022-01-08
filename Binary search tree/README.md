# Binary search tree

~~~C++
//Binary search tree : parent의 왼쪽 child는 parent보다 작고, 오른쪽 child는 parent보다 큰 binary tree
//inOrder, preOrder, postOrder traversal
//-----Code
#include <iostream>
#include <string>
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

Node* insert_recursion(Node* node, int data) {
	if (node == NULL) // If the pointer doesn't have address (the node doesn't exist)
		return new_node(data);
	if (data < node->data) // If new node is smaller than parent
		node->left = insert_recursion(node->left, data);
	else
		node->right = insert_recursion(node->right, data);
	return node; // When node->left has address, in order not to return a trash address
}

void insert(int data) {
	if (root == NULL) { // If there isn't root
		root = new_node(data);
		return;
	}

	Node* pointer = root;
	while (true)
		if (data < pointer->data) // Smaller than parent?
			if (pointer->left == NULL) { // If there isn't left child
				pointer->left = new_node(data);
				return;
			}
			else // If there's left child
				pointer = pointer->left;
		else
			if (pointer->right == NULL) {
				pointer->right = new_node(data);
				return;
			}
			else
				pointer = pointer->right;
}

Node* minNode(Node* node) {
	Node* pointer = node;
	while (pointer && pointer->left != NULL) // Find the leftmost leaf
		pointer = pointer->left;
	return pointer;
}

Node* delete_recursion(Node* node, int target) {
	if (node == NULL) // To prevent read violation and trash address
		return node;

	// Find the node to be deleted
	if (target == node->data) { // If target was found
		// If the node is with only one child or no child
		if (node->left == NULL) {
			Node* temp = node->right;
			delete node;
			return temp;
		}
		else if (node->right == NULL) {
			Node* temp = node->left;
			delete node;
			return temp;
		}
		// If the node has two children
		Node* temp = minNode(node->right); // Minimum of right subtree
		node->data = temp->data; // Place the minimum of right subtree in position of the node to be deleted
		node->right = delete_recursion(node->right, temp->data);
	}
	else if (target < node->data)
		node->left = delete_recursion(node->left, target);
	else
		node->right = delete_recursion(node->right, target);
	return node; // In order not to put a trash address
}

void deleteNode(Node* root, int target) {
	Node* pointer = root, * prev = root;
	bool prev_left = false;
	while (pointer) {
		if (target == pointer->data) { // If target was found
			// If the node is with only one child or no child
			if (pointer->left == NULL) {
				Node* temp = pointer->right;
				delete pointer;
				prev_left == true ? prev->left = temp : prev->right = temp;
				return;
			}
			else if (pointer->right == NULL) {
				Node* temp = pointer->left;
				delete pointer;
				prev_left == true ? prev->left = temp : prev->right = temp;
				return;
			}
			// If the node has two children
			Node* temp = minNode(pointer->right); // Minimum of right subtree
			pointer->data = temp->data; // Place the minimum of right subtree in position of the node to be deleted
			deleteNode(pointer->right, temp->data);
			return;
		}
		else if (target < pointer->data) {// If target is smaller than parent
			prev = pointer;
			pointer = pointer->left;
			prev_left = true;
		}
		else {
			prev = pointer;
			pointer = pointer->right;
			prev_left = false;
		}
	}
	cout << "Not found\n";
}

// Inorder traversal
void inOrder(Node* node) {
	if (node != NULL) {
		inOrder(node->left); // Traverse left
		cout << node->data << " -> "; // Traverse root
		inOrder(node->right); // Traverse right
	}
}

string search(int target) {
	Node* pointer = root;
	while (pointer != NULL) {
		if (target == pointer->data)
			return "*" + to_string(pointer->data) + "* found\n";
		else if (target < pointer->data) // If target is smaller than parent
			pointer = pointer->left;
		else
			pointer = pointer->right;
	}
	return "Not found\n";
}

int main() {
	for (int i = 1; i <= 4; i++)
		insert(i);
	cout << "In order traversal : "; inOrder(root); cout << endl;

	deleteNode(root, 4);
	cout << "After deleting a node : "; inOrder(root); cout << endl;

	cout << search(3);
}
~~~

