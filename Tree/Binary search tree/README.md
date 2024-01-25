# Binary search tree
Binary search tree is a used for fast lookups where the left child of a parent is smaller than the parent and the right child is bigger than the parent.<br>
It can be used to search for data in **O(log(n))** time, which is faster than linear search that takes **O(n)**, but slower than the corresponding operations on hash tables.

## Node deletion
- **Deleting a node that has no child**: Just delete it.
- **Deleting a node that has one child**: put the child of the deleted node to the deleted node.
- **Deleting a node that has 2 children**: Replace the deleted node with the max node in its left subtree. (or the min node in the right subtree)<br>
This is because a left node has to be smaller than its parent and a right node bigger.

![image](https://user-images.githubusercontent.com/67142421/176267897-54f6b683-1030-4394-b91e-57225fd1f85c.png)

## Worst case
When the elements that are stored are [1,2,3,4,5,6]. -> **O(n)**

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
	if (node == NULL)
		return new_node(data);

	if (data < node->data) // If new node is smaller than parent, go to the left child
		node->left = insert(node->left, data);
	else if (data > node->data) // If not, go to the right child
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
			if (crawler->left == NULL) { // If the parent doesn't have a left child, insert in the left child
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

Node* find_max_node(Node* node) {
    Node* current = node;
    while (current->right)
        current = current->right;
    return current;
}

Node* delete_node(Node* curr, int target) {
    if (curr == nullptr)
        return root;

    if (target < curr->data) {
        curr->left = delete_node(curr->left, target);
    } else if (target > curr->data) {
        curr->right = delete_node(curr->right, target);
    } else { // Found the target
        if (curr->left == nullptr) { // Just move the child
            Node* temp = curr->right;
            delete curr;
            return temp;
        } else if (curr->right == nullptr) { // Just move the child
            Node* temp = curr->left;
            delete curr;
            return temp;
        } else { // Has 2 children
            Node* max_node = find_max_node(curr->left);
            curr->data = max_node->data;
            curr->left = delete_node(curr->left, max_node->data);
        }
    }
    return curr;
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
## Output
<img src="https://user-images.githubusercontent.com/67142421/148778914-a7f42d34-addd-4c75-a6df-3638c62bb195.png">


