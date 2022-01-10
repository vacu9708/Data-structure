# Trie
A trie, also called prefix tree, is a type of search tree used for locating specific keys from within a set. These keys are most often strings.

~~~C++
// -----C++ implementation of operations on Trie
#include <iostream>
#include <string>
#include <vector>
using namespace std;

const int ALPHABET_SIZE = 26; // ALPHABET_SIZE = 67 to include uppercase letters

struct TrieNode {
	TrieNode* children[ALPHABET_SIZE] = { NULL, };
	string data = "";
};
TrieNode* root;

void insert(string data) {
	TrieNode* crawl = root;
	int data_length = data.length();
	for (int i = 0; i < data_length; i++) { // Crawl trie
		int index = data[i] - 'a'; // To index
		if (crawl->children[index] == NULL) // If the pointer doesn't have address, that is, if it's not allocated (if the character doesn't exist)
			crawl->children[index] = new TrieNode;
		crawl = crawl->children[index]; // Move crawler
	}
	crawl->data = data; // Store data
}

void delete_word(string target) {
	TrieNode* crawler = root;
	int index = 0;
	int target_length = target.length();
	vector<TrieNode*> letters(1, root);

	for (int i = 0; i < target_length; i++) {
		index = target[i] - 'a';
		if (crawler->children[index] == NULL)
			return;

		crawler = crawler->children[index];
		letters.push_back(crawler);
	}

	for (int i = letters.size() - 1; i >= 1; i--) {
		bool deallocation = true;
		for (int j = 0; j < 26; j++) {// Check if target has any children
			// If the target has a child which has never been visited, only delete the data without deallocation so that other words connected to the target node can be accessed
			// For example, when deleting "the" : 
			// 1. There is "there" but there isn't "the". In this case, there is no change because the data in the location of "the" is "".
			// 2. There are both "there" and "the". In this case, only the data in the location of "the" is deleted and becomes "".
			// 3. There is "the" and "atheb : Their paths don't overlap.
			if (letters[i]->children[j] != NULL) {
				deallocation = false;
				break;
			}
		}
		//-----
		if(deallocation == false)
			letters[i]->data = "";
		else {
			delete letters[i];
			letters[i - 1]->children[target[i] - 'a'] = NULL;
		}
	}
}

void delete_word_simple(string target) { // Memory leak occurs
	TrieNode* crawl = root;
	int target_length = target.length();
	for (int i = 0; i < target_length; i++) {
		int index = target[i] - 'a';
		if (crawl->children[index])
			crawl = crawl->children[index];
	}
	if (crawl->data == target)
		crawl->data = "";
}


void match_whole_word_search(string target) {
	TrieNode* crawl = root;
	for (int i = 0; i < target.length(); i++) {
		int index = target[i] - 'a';
		if (crawl->children[index] == NULL) {
			cout << "(" << target << ") not found\n";
			return;
		}
			
		crawl = crawl->children[index];
	}
	if (crawl->data == target) // If target was found
		cout << "(" << crawl->data << ") found" << endl;
	else
		cout << "(" << target << ") not found\n";
}

void DFS(TrieNode* crawler) { // For partial string search
	if (crawler->data != "")
		cout << "(" << crawler->data << ") found" << "\n";

	for (int i = 0; i < ALPHABET_SIZE; i++)
		if (crawler->children[i] != NULL)
			DFS(crawler->children[i]);
}

void partial_string_search(string target) {
	TrieNode* crawler = root;
	int target_length = target.length();
	for (int i = 0; i < target_length; i++) {
		int index = target[i] - 'a';
		if (crawler->children[index] == NULL)
			return;
		
		crawler = crawler->children[index];
	}
	DFS(crawler);
}

int main() {
	root = new TrieNode;

	// Input keys (use only 'a' through 'z' and lower case)
	string words[] = { "the", "there", "though", "answer", "any" };

	for (string i : words)
		insert(i);

	partial_string_search("th");
	match_whole_word_search("the");
	match_whole_word_search("there");

	delete_word("there");
	match_whole_word_search("there");
}
~~~

## Result
![Untitled](https://user-images.githubusercontent.com/67142421/148843104-56256946-d593-4a74-a2ba-69b09d34862c.png)
