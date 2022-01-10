# Hash table
A hash table is a data structure that implements an associative array abstract data type, a structure that can map keys to values. 
A hash table uses a hash function to compute an index, also called a hash code, into an array, from which the desired value can be found.
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
		return hashcode % table_length; // Convert hash code to index with hash function
	}

	void insert(string key, string data) { // Insert data into hash table
		int index = make_index(key);
		table[index].push_back(Node(key, data));
	}

	void deleteData(string key) { // Delete the last data
		int index = make_index(key);
		if (table[index].empty()) {
			cout << "(" << key << ") not found\n";
			return;
		}
		cout << "Delete (" << key << ")\n";
		table[index].pop_back();
	}

	void search(string key) {
		int index = make_index(key);
		if (table[index].empty())
			cout << "Search : (" << key << ") not found\n";
		for (Node i : table[index])
			cout << "Search : (" << key << ") " << "(" << i.data << ") found\n";
	}
};

int main() {
	HashTable* h = new HashTable(11);
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
![Untitled](https://user-images.githubusercontent.com/67142421/148783425-8fae9f1b-3f41-4d8b-a70b-ae4e2781c94b.png)
