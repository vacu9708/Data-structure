# Double ended queue
![image](https://user-images.githubusercontent.com/67142421/230274701-62526e26-06b6-44b8-9f92-868c549ee63a.png)

A deque (short for "double-ended queue") is a mix of a queue and a stack, as it allows insertion and deletion of elements from both ends.

~~~python
class Node:
    def __init__(self, data):
        self.left=self.right=None
        self.data=data

class Deque:
    def __init__(self):
        self.head=self.tail=None

    def push_right(self, data):
        if not self.head:
            self.head=Node(data)
            self.tail=self.head
            return
        node=Node(data)
        self.tail.right=node
        node.left=self.tail
        self.tail=self.tail.right

    def push_left(self, data):
        if not self.head:
            self.head=Node(data)
            self.tail=self.head
            return
        node=Node(data)
        self.head.left=node
        node.right=self.head
        self.head=self.head.left

    def pop_right(self):
        data=self.tail.data
        self.tail=self.tail.left
        self.tail.right=None
        return data

    def pop_left(self):
        data=self.head.data
        self.head=self.head.right
        self.head.left=None
        return data
~~~
