![image](https://github.com/vacu9708/Data-structure/assets/67142421/16adefa5-7b05-496b-8912-0b266ace2d3f)

A red-black tree stays balanced with operations like insertion, deletion, and search in O(log n) time complexity,

## Red black tree properties
The properties that must be preserved are:
- **Node Color**: Every node is either red or black.
- **Root Property**: The root of the tree is always black.
- **Leaf Property**: Every leaf (NIL node) is black.
- **Red Node Property**: Red nodes cannot have red children (i.e., no two red nodes can be adjacent).
- **Black Depth Property**: Every path from a node to its descendant NIL nodes has the same number of black nodes.

## Cases During Insertion
When a new node is inserted, it is initially colored red. This can lead to a violation of the Red-Black Tree properties, particularly the Red Node Property. The cases during insertion are:

- **Case 1**: The new node is at the root of the tree.
    - **Action**: Color the node black to satisfy the root property.
- **Case 2**: The new node's parent is black.
    - **Action**: No action needed, as the tree remains valid.
- **:Case 3**:: The new node's parent and uncle are red.
    - **Action**:: Color both the parent and the uncle black and the grandparent red. Then, recheck the tree starting from the grandparent.
- **Case 4**:: The new node's parent is red but the uncle is black; the new node is added to the right of the left child or to the left of the right child (the "triangle" configuration).
    - **Action**:: Perform a rotation (left or right, respectively) on the parent, transforming the case into Case 5.
- **Case 5**:: The new node's parent is red but the uncle is black; the new node is added to the left of the left child or to the right of the right child (the "line" configuration).
    - **Action**: Perform a rotation on the grandparent (right or left, respectively), swap the colors of the grandparent and parent, and recheck the tree.
## Cases During Deletion
Deletion can be more complex due to the need to replace the deleted node and potentially rebalance the tree. The cases during deletion are:
- **Case 1**: The sibling of the node being fixed is red.
    - **Action**: Recolor the sibling and the parent, and perform a rotation on the parent.
- **Case 2**: The sibling and its children are black.
    - **Action**: Recolor the sibling and move the problem up the tree by rechecking the tree starting from the parent.
- **Case 3**: The sibling is black, its left child is red, and its right child is black.
    - **Action**: Recolor the sibling and its left child, and perform a rotation on the sibling. This transforms the case into Case 4.
- **Case 4**: The sibling is black and its right child is red.
    - **Action**: Perform a rotation on the parent, recolor the sibling and the right child, and recheck the tree.

~~~python
class Node:
    def __init__(self, data, color="RED"):
        self.data = data
        self.color = color
        self.parent = None
        self.left = None
        self.right = None

class RedBlackTree:
    def __init__(self):
        self.TNULL = Node(0, color="BLACK")  # TNULL node represents NULL
        self.root = self.TNULL

    def insert(self, key):
        # Ordinary Binary Search Insertion
        node = Node(key)
        node.left = self.TNULL
        node.right = self.TNULL

        y = None
        x = self.root

        while x != self.TNULL:
            y = x
            if node.data < x.data:
                x = x.left
            else:
                x = x.right

        # y is parent of x
        node.parent = y
        if y is None:
            self.root = node
        elif node.data < y.data:
            y.left = node
        else:
            y.right = node

        # If new node is a root node, simply return
        if node.parent is None:
            node.color = "BLACK"
            return

        # If the grandparent is None, simply return
        if node.parent.parent is None:
            return

        # Fix the tree
        self.fix_insert(node)

    def fix_insert(self, k):
        while k.parent.color == "RED":
            if k.parent == k.parent.parent.right:
                u = k.parent.parent.left  # uncle
                if u.color == "RED":
                    # case 3.1
                    u.color = "BLACK"
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    k = k.parent.parent
                else:
                    if k == k.parent.left:
                        # case 3.2.2
                        k = k.parent
                        self.right_rotate(k)
                    # case 3.2.1
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    self.left_rotate(k.parent.parent)
            else:
                # same as above with "right" and "left" exchanged
                u = k.parent.parent.right

                if u.color == "RED":
                    # mirror case 3.1
                    u.color = "BLACK"
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    k = k.parent.parent
                else:
                    if k == k.parent.right:
                        # mirror case 3.2.2
                        k = k.parent
                        self.left_rotate(k)
                    # mirror case 3.2.1
                    k.parent.color = "BLACK"
                    k.parent.parent.color = "RED"
                    self.right_rotate(k.parent.parent)
            if k == self.root:
                break
        self.root.color = "BLACK"

    def left_rotate(self, x):
        y = x.right
        x.right = y.left
        if y.left != self.TNULL:
            y.left.parent = x

        y.parent = x.parent
        if x.parent is None:
            self.root = y
        elif x == x.parent.left:
            x.parent.left = y
        else:
            x.parent.right = y
        y.left = x
        x.parent = y

    def right_rotate(self, x):
        y = x.left
        x.left = y.right
        if y.right != self.TNULL:
            y.right.parent = x

        y.parent = x.parent
        if x.parent is None:
            self.root = y
        elif x == x.parent.right:
            x.parent.right = y
        else:
            x.parent.left = y
        y.right = x
        x.parent = y
    # ... [Previous code from the first half]

    def delete_node(self, data):
        self.delete_node_helper(self.root, data)

    def delete_node_helper(self, node, key):
        z = self.TNULL
        while node != self.TNULL:
            if node.data == key:
                z = node

            if node.data <= key:
                node = node.right
            else:
                node = node.left

        if z == self.TNULL:
            print("Couldn't find key in the tree")
            return

        y = z
        y_original_color = y.color
        if z.left == self.TNULL:
            x = z.right
            self.rb_transplant(z, z.right)
        elif z.right == self.TNULL:
            x = z.left
            self.rb_transplant(z, z.left)
        else:
            y = self.minimum(z.right)
            y_original_color = y.color
            x = y.right
            if y.parent == z:
                x.parent = y
            else:
                self.rb_transplant(y, y.right)
                y.right = z.right
                y.right.parent = y

            self.rb_transplant(z, y)
            y.left = z.left
            y.left.parent = y
            y.color = z.color
        if y_original_color == "BLACK":
            self.fix_delete(x)

    def rb_transplant(self, u, v):
        if u.parent is None:
            self.root = v
        elif u == u.parent.left:
            u.parent.left = v
        else:
            u.parent.right = v
        v.parent = u.parent

    def fix_delete(self, x):
        while x != self.root and x.color == "BLACK":
            if x == x.parent.left:
                s = x.parent.right
                if s.color == "RED":
                    s.color = "BLACK"
                    x.parent.color = "RED"
                    self.left_rotate(x.parent)
                    s = x.parent.right

                if s.left.color == "BLACK" and s.right.color == "BLACK":
                    s.color = "RED"
                    x = x.parent
                else:
                    if s.right.color == "BLACK":
                        s.left.color = "BLACK"
                        s.color = "RED"
                        self.right_rotate(s)
                        s = x.parent.right

                    s.color = x.parent.color
                    x.parent.color = "BLACK"
                    s.right.color = "BLACK"
                    self.left_rotate(x.parent)
                    x = self.root
            else:
                # mirror the code above
                s = x.parent.left
                if s.color == "RED":
                    s.color = "BLACK"
                    x.parent.color = "RED"
                    self.right_rotate(x.parent)
                    s = x.parent.left

                if s.right.color == "BLACK" and s.left.color == "BLACK":
                    s.color = "RED"
                    x = x.parent
                else:
                    if s.left.color == "BLACK":
                        s.right.color = "BLACK"
                        s.color = "RED"
                        self.left_rotate(s)
                        s = x.parent.left

                    s.color = x.parent.color
                    x.parent.color = "BLACK"
                    s.left.color = "BLACK"
                    self.right_rotate(x.parent)
                    x = self.root
        x.color = "BLACK"

    def minimum(self, node):
        while node.left != self.TNULL:
            node = node.left
        return node
    
    def print_tree(self):
        def print_helper(node, indent, last):
            # Prints the tree structure on the screen
            if node != self.TNULL:
                print(indent, end='')
                if last:
                    print("R----", end='')
                    indent += "     "
                else:
                    print("L----", end='')
                    indent += "|    "

                s_color = "RED" if node.color == "RED" else "BLACK"
                print(str(node.data) + "(" + s_color + ")")
                print_helper(node.left, indent, False)
                print_helper(node.right, indent, True)
        print_helper(self.root, "", True)

if __name__ == "__main__":
    rbt = RedBlackTree()
    rbt.insert(55)
    rbt.insert(40)
    rbt.insert(65)
    rbt.insert(60)
    rbt.insert(75)
    rbt.insert(57)
    rbt.print_tree()

    rbt.delete_node(40)
    rbt.print_tree()
~~~
### Result
![image](https://github.com/vacu9708/Data-structure/assets/67142421/1345e8da-e365-4d29-8874-735edb1efdf8)

![image](https://github.com/vacu9708/Data-structure/assets/67142421/1b10b4d3-61ec-410d-a278-93d541a9ae9d)
