# Enumerating each level in a tree

Given a tree $T$, return each level of this tree as a separate vector / array.

## Discussion
The idea is the same as level order traversal, but asks us to be able to identify when we switch between levels in the tree. 

With vanilla level order traversal, we maintain a queue of nodes to visit. Each time we process the queue's head, we add the children of currently processed node to the tail of the queue. The psuedocode for this looks like so:

#### Iterative
```psuedo
// This function traverses a tree level by level
// yield is the same as python's yield keyword
define LevelOrder(Tree T):
	Queue Q
	Node root = T.root
	Enqueue(Q, root)
	while(|Q| > 0):
		root = Dequeue(Q).value
		yield root
		foreach child in root.children:
			Enqueue(child)	
```
#### Recursive
```psuedo
// Traverses a tree level by level
// yield is the same as python's yield keyword
define LevelOrder(Tree T):
	Queue Q
	Node root = T.root
	Enqueue(Q, root)
	LevelOrderUtil(Q)

define LevelOrderUtil(Queue Q):
	if(|Q| > 0):
		root = Dequeue(Q).value
		yield root
		foreach child in root.children:
			Enqueue(child)
		LevelOrderUtil(Q)
```

Now, to capture each level, we need the following: Separate the addition of children from the parent's processing queue. To this end, we maintain a separate queue of the parent level and children. The code for this looks like so:

## Code


#### Iterative
```psuedo
// This function traverses a tree level by level
// Returns a sequence of lists representing each level
// yield is the same as python's yield keyword
define LevelOrder(Tree T):
	Queue Q
	Node root = T.root
	List parentList
	Enqueue(Q, root)
	while(|Q| > 0):
		Queue parentQ = Q
		// Q represents elements in the current level
		while(|Q| > 0):
			parentList.push(Dequeue(Q))
		// Q is empty at this point
		while(|parentQ| > 0):
			Node n = Dequeue(parentQ)
			foreach child in n.children:
				Enqueue(Q, child)
		yield parentList
```

#### Recursive
```psuedo
// This function traverses a tree level by level
// Returns a sequence of lists representing each level
// yield is the same as python's yield keyword
define LevelOrder(Tree T):
	Queue Q
	Node root = T.root
	Enqueue(Q, root)
	LevelOrderUtil(Q)

define LevelOrderUtil(Queue Q):
	if(|Q| > 0):
		Queue parentQ = Q
		List parentList
		// Q represents elements in the current level
		while(|Q| > 0):
			parentList.push(Dequeue(Q))
		// Q is empty at this point
		while(|parentQ| > 0):
			Node n = Dequeue(parentQ)
			foreach child in n.children:
				Enqueue(Q, child)
		yield parentList
		LevelOrderUtil(Q)
```

### TODO 
--- Further optimizations are possible at a cursory glance like combining the while loops into one (to de-populate and populate the queues in a single loop), look into these