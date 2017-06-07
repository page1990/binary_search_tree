# binary_search_tree
python实现的二叉树查询

```
class Node(object):
    """充分考虑到所有的情况
    """

    def __init__(self, data):
        self.left = None
        self.right = None
        self.data = data

    def insert(self, data):
        if self.data:
            if data < self.data:
                if self.left:
                    self.left.insert(data)
                else:
                    self.left = Node(data)
            elif data > self.data:
                if self.right:
                    self.right.insert(data)
                else:
                    self.right = Node(data)
        else:
            self.data = data

    def lookup(self, data, parent=None):
        if data < self.data:
            if self.left is None:
                return None, None
            else:
                return self.left.lookup(data, self)
        elif data > self.data:
            if self.right is None:
                return None, None
            else:
                return self.right.lookup(data, self)
        else:
            return self, parent

    def children_count(self):
        cnt = 0

        if self.left:
            cnt += 1
        if self.right:
            cnt += 1
        return cnt

    def delete(self, data):
        node, parent = self.lookup(data)

        if node is not None:
            children_count = node.children_count()

            if children_count == 0:
                if parent:
                    if parent.left is node:
                        parent.left = None
                    else:
                        parent.right = None
                    del node
                else:
                    parent.data = None
            elif children_count == 1:
                if node.right:
                    n = node.right
                else:
                    n = node.left

                if parent:
                    if node.right:
                        parent.right = node.right
                    else:
                        parent.left = node.left
                    del node
                else:
                    self.right = n.right
                    self.left = n.left
                    self.data = n.data
            else:
                parent = node
                successor = node.right

                while successor.left:
                    parent = successor
                    successor = successor.left

                node.data = successor.data

                if parent.left is successor:
                    parent.left = successor.right
                else:
                    parent.right = successor.right

    def print_tree(self):
        if self.left:
            self.left.print_tree()
        print(self.data)

        if self.right:
            self.right.print_tree()

    def tree_data(self):
        stack = []
        node = self

        while stack or node:
            if node:
                stack.append(node)
                node = node.left
            else:
                node = stack.pop()
                yield node.data
                node = node.right
```

参考文献:http://www.laurentluce.com/posts/binary-search-tree-library-in-python/comment-page-1/
