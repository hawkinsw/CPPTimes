### What's News

A rare [fungus](https://www.wsj.com/finance/commodities-futures/africa-cocoa-production-prices-reseeding-e4d19821?st=iwgUNM&reflink=desktopwebshare_permalink) has made its way into the forests of North America. It's brutality is almost as spectacular as its capriciousness: an afflicted tree loses its ability to grow leaves and caps the number of splits in every one of its branches to two.

### Elmsbeth

We are now moving on to something _completely_ different than sorting, but, ultimately, related. One of the benefits of sorting an array of data was to allow us to efficiently search for a value. When a list of data is in sorted order, we can use a very fast searching algorithm (binary search).[^optimal]

[^optimal]: In fact, binary search is an _optimal_ search algorithm -- we cannot do any better!

We create an list of data and sort it. Now we can efficiently search it. What happens, though, when we either add or remove an element from the list of elements? Well, if we want to be able to continue to use the binary search algorithm, then we will have to guarantee that the data is still in sorted order. This prerequisite may be maintained when removing elements, but it is not like that an array of data will remain sorted when we add new elements.

So, why couldn't we just resort the list after adding or deleting new elements? We could, of course, but that will get "costly" very quickly. We saw how hard (computationally) it is to sort a list. We definitely do _not_ want to do that every single time that we add or remove an element.

The data structure that we will study for the rest of the semester is going to allow us to keep a list of data ordered even as we add and remove elements so that we can always get the benefit of an efficient search! Pretty cool!

### Trees

Let's just pull the Band Aid right off -- the data structure that we are going to study is named the tree. There are a _ton_ of uses for the tree in computer science. We will look at just one of those uses at the end of this document. In class we worked "forward" from the example to the terminology. Here we will work backward -- going from the technical description of the tree to the examples.

#### The Vocabulary

![Tree Vocabulary - Part A.png](./graphics/Tree%20Vocabulary%20-%20Part%20A.png)

The figure shown above is a visualization of a _general tree_. We will give a precise definition of the general tree later. Every red dot is a _node_ in the tree and the black lines that connect nodes to one another are known as _edges_. Unlike earlier "graphs" that we have seen in this course, the edges here are directed and "descend" from top to bottom.

Nodes have at most one parent. The _parent_ of a node A is the node from which node A descends. A node may have many children. A node B is a _child_ of node A if node B's parent is node A. Yes, these definitions are circular and can be quite complicated! Sorry! The good news is that they are relatively intuitive. A _leaf_ node is a node without any children. The _root_ node is the node without any parents. If a node A is an ancestor of a node B, then there is a unique path between the two nodes. The path consists of the nodes themselves and all the nodes that are reached when following edges between them. If there is a path between two nodes, then it is unique.

Every node has a height and a level. The _height_ of a node in a tree is it's position from bottom to top. The _level_ of a node is its position from the top to the bottom.

#### Height

1.  A node with no node children has height 1.
2.  Any other node has a height equal to the maximum _level_ of all of its descendants.

This definition is not helpful -- we haven't determined what the _level_ is yet, after all! Intuitively, the height of a node is the number of nodes between it and its "deepest" leaf (including itself _and_ the leaf). We will define _deepest_ as having the longest path.

#### Level

1.  The root node has a level of 1.
2.  Every other node has a level of 1 more than it's parent's level.

![Tree Vocabulary - Part B.png](./graphics/Tree%20Vocabulary%20-%20Part%20B.png)

The figure shown above is another visualization of a general tree highlighting a different set of vocabulary. Each node has a certain number of children. The maximum number of children that any node in the tree has is known as the _ariness_ of the tree. If all the nodes in the tree $T$ have a maximum of $n$ children, then $T$ is an _n-ary_ tree.

### A Recursive Data Structure

We learned a term a long time ago -- the recursive data structure. The recursive data structure is a data structure that can be defined in terms of itself. Is a tree a recursive data structure? Yes, indeed it is. We can see that visually. In the second visualization of the general tree (immediately above), the nodes rooted at node a form a tree. Each of its children, too, forms their own tree. For example, node b is a child of node a and it roots a general tree, too! The same goes for node c -- it is a member of the general tree rooted at a _and_ the root of its own tree.

Precisely, a _general_ (_rooted_) tree $T$ is a set of nodes split into disjoint subsets where

1.  One nodes in the set $T$ is defined as the root, and
2.  every other node is the member of one subset of nodes whose members themselves make up a general tree.

The written definition makes it clear that the general tree is a recursive data structure, but the visualization of the general tree makes it clear, too.

If two trees that are otherwise identical are different because of the relative order of their subtrees, then that tree is _ordered_. If two trees are not distinguished according to the relative order of their subtrees, then they are _oriented_.

### The Most Useful Trees

In computer science, we often make great use of binary trees, or 2-ary. In other words, a binary tree is a form of a general tree where every node has at most 2 children.

Simply because a tree has at most two children does not necessarily make it useful. However, when we add another constraint to a binary tree (stay tuned!), then binary trees become especially useful!

### Further Reading:

Liu, David, and Mario Badr. "15.1 Introduction to Trees.” Toronto.edu, 2025, [www.teach.cs.toronto.edu/~csc110y/fall/notes/15-trees/01-trees-intro.html](www.teach.cs.toronto.edu/~csc110y/fall/notes/15-trees/01-trees-intro.html). Accessed 2 Dec. 2025.