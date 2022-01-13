# Hash table
A hash table is a data structure that implements an associative array abstract data type, a structure that can map keys to values. 
A hash table uses a hash function to compute an index, also called a hash code, into an array, from which the desired value can be found.

![473px-Hash_table_3_1_1_0_1_0_0_SP svg](https://user-images.githubusercontent.com/67142421/148845486-a0a5ecbe-ddfb-4660-983b-781dee2fcf82.png)

## Hash collision
![hashtable2](https://user-images.githubusercontent.com/67142421/148845229-92e74e37-9e50-42db-91cb-c1f49d493891.png)

## Solution of hash collision : Separate chaining
![675px-Hash_table_5_0_1_1_1_1_1_LL svg](https://user-images.githubusercontent.com/67142421/148845616-96162e68-6cb0-44e3-8344-042429f77294.png)

~~~C++
#include <iostream>
#include <list>
#include <string>
using namespace std;

class HashTable {
public:
	int table_length;
	class Node {
	public:
		string key;
		string data;
		Node(string key, string value) {
			this->key = key;
			this->data = value;
		}
	};

	list<Node>* table;
	HashTable(int table_length) {
		this->table_length = table_length;
		this->table = new list<Node>[table_length];
	}

	int make_index(string key) {
		// Make a hash code with key
		int hashcode = 0;
		int weight = 1;
		for (char c : key) {
			hashcode += c * weight; // To distinguish anagrams such as "abc" and "cba"
			weight *= 10;
		}
		return hashcode % table_length; // Hash function : convert hash code to index.
	}

	void insert(string key, string data) { // Insert data into hash table
		int index = make_index(key);
		table[index].push_back(Node(key, data));
	}

	void deleteData(string key) { // Delete the last data
		int index = make_index(key);

		// Find the node that has the key
		auto iterator = table[index].begin();
		for (iterator = table[index].begin(); iterator != table[index].end(); iterator++)
			if (iterator->key == key)
				break;
		//-----
		if (iterator == table[index].end()) {
			cout << "(" << key << ") not found\n";
			return;
		}

		cout << "Delete (" << key << ")\n";
		table[index].erase(iterator);
	}

	void search(string key) {
		int index = make_index(key);

		// Find the node that has the key
		auto iterator = table[index].begin();
		for (iterator = table[index].begin(); iterator != table[index].end(); iterator++)
			if (iterator->key == key)
				break;
		//-----
		if (iterator == table[index].end()) {
			cout << "Search : (" << key << ") -> (not found)\n";
			return;
		}

		cout << "Search : (" << key << ") -> (" << iterator->data << ") found\n";
	}
};

int main() {
	HashTable* h = new HashTable(1);
	list<string> container;
	h->insert("John", "He is cute");
	h->insert("Paul", "He is a cutie");
	h->search("John");
	h->search("Paul");
	h->deleteData("John");
	h->search("John");
}
~~~
## Result
![Untitled](https://user-images.githubusercontent.com/67142421/149351330-0c070a9a-0547-44f4-abdf-68530d6f9aee.png)
