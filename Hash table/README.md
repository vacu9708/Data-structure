# Hash table
>A hash table is an associative array, whose indices are normally arbitrary strings or other complicated objects, which maps keys to values used for searching for elements.<br>
>A hash table uses a hash function to compute an index, also called a hash code, into an array, from which the desired value can be found very fast irrespective of the number of the elements.

## The characteristic of Hash table
>Each key is mapped to a specific index by a *hash function* in the array and its value stored in that location unless a *hash collision* occurs. Because of this, insertion, search and delete operations are performed in constant time **O(1)**.

![image](https://github.com/vacu9708/Data-structure/assets/67142421/570dbc4a-2a46-4f9b-b46d-c31a783a7006)

## Hash collision
> There is a possibility that two keys result in the same value. The situation where a newly inserted key maps to an already occupied slot in the hash table is called *collision* and must be handled using some collision handling technique. 
![hashtable2](https://user-images.githubusercontent.com/67142421/148845229-92e74e37-9e50-42db-91cb-c1f49d493891.png)

## Solution of hash collision : Separate chaining
>The idea is to make each cell of hash table have a linked list to solve the hash collision.
![image](https://user-images.githubusercontent.com/67142421/150531067-fe59c4e7-2f4c-4d55-9705-1fb2d1c509ce.png)

~~~C++
#include <iostream>
#include <list>
using namespace std;

class HashTable {
	int table_length;
public:
	class Node {
	public:
		string key;
		string data;
		Node(string key, string value) {
			this->key = key;
			this->data = value;
		}
	};

	list<Node>* buckets;
	HashTable(int table_length) {
		this->table_length = table_length;
		this->buckets = new list<Node>[table_length];
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
		buckets[index].push_back(Node(key, data));
	}

	void deleteData(string key) { // Delete the last data
		int index = make_index(key);

		// Find the node that has the key
		list<Node>::iterator iterator;
		for (iterator = buckets[index].begin(); iterator != buckets[index].end(); iterator++)
			if (iterator->key == key)
				break;
		//-----
		if (iterator == buckets[index].end()) {
			cout << "(" << key << ") not found\n";
			return;
		}

		cout << "Delete (" << key << ")\n";
		buckets[index].erase(iterator);
	}

	void search(string key) {
		int index = make_index(key);

		// Find the node that has the key
		list<Node>::iterator iterator;
		for (iterator = buckets[index].begin(); iterator != buckets[index].end(); iterator++)
			if (iterator->key == key)
				break;
		//-----
		if (iterator == buckets[index].end()) {
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
## Output
![Untitled](https://user-images.githubusercontent.com/67142421/149351330-0c070a9a-0547-44f4-abdf-68530d6f9aee.png)
