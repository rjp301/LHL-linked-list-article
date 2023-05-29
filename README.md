# 🔗 Linked List: The Chain Datatype

## Introduction

The _linked list_ is a linear data structure in computer science that consist of a series of _nodes_. All the nodes are stored separately in memory and contain both some amount of data and a reference field or _pointer_ that links them together. A linear data structure is one in which data is stored sequentially, though not necessarily contiguously, i.e. there is no branching of data paths.

The best real-world analogy for a linked list is a chain. Each link in the chain connects to the next and if one of the links breaks the chain becomes two chains. The whole chain can be manipulated by grabbing any of the links.

![Data structure of a singly linked list](Linked%20List%20The%20Chain%20Datatype%20c5d630822be04a9abe829f8c36fe5709/Screen_Shot_2022-03-11_at_4.30.15_PM.png)

Data structure of a singly linked list

![Data structure of a doubly linked list](Linked%20List%20The%20Chain%20Datatype%20c5d630822be04a9abe829f8c36fe5709/Screen_Shot_2022-03-12_at_3.56.39_PM.png)

Data structure of a doubly linked list

There exist both _doubly linked lists_ and _singly linked lists_. The difference being that in a singly linked list each node only has information about the next node in the chain whereas in a doubly linked list the nodes know both what precedes and follows them. In a singly linked list the first node is referred to as the _head_ and is passed around as the means to access the list. The head must be the point of access because there is no means of travelling backwards through a singly linked list; unlike a doubly linked one. The last node in the list does not have a pointer i.e. it points to null. For the purposes of this essay, only singly linked lists will be discussed to keep things simple.

## Compared to an Array

Though they are both _linear_ data structures, the linked list and the array have quite different properties and means of accessing data. An array stores elements in _contiguous memory locations_ allowing for the easy access of specific elements by their index. Linked list elements need not necessarily be stored next to one another in memory as each element contains within it the reference to the next element in the chain.

![Data structure of an array](Linked%20List%20The%20Chain%20Datatype%20c5d630822be04a9abe829f8c36fe5709/Screen_Shot_2022-03-11_at_10.34.27_PM.png)

Data structure of an array

This major difference in data storage method leads to some interesting differences between the two schemas.

**Space Differences:** Because each element in the linked lists must store a pointer to the next element along with the data payload, the total memory taken up by a linked list will be greater than an array of equal length.

**Time Differences:** Due to the contiguous nature of arrays, any element can be randomly accessed in linear time using its index. However in a linked list each node starting from the head must be traversed in order to get to a specific value. Thus accessing the _i-th_ element will take _O(i)_ time. Think of accessing an array as opening a book to a specific page number with a bookmark and accessing a list as counting the links of a chain to get to the _i-th_ one.

You may be wondering why anyone would use a linked list if it both takes up more space and takes longer to access a specific element? The advantage of linked lists over arrays lies in their non-contiguous nature which allows the data structure to be modified very quickly do to the small changes in memory required.

The insertion or deletion of elements from an array is a very expensive process because it requires elements to be shifted in memory to make room for the new element or fill the gap left by the deletion. Take the following sorted list of integers for instance: `[1,4,7,9,12,17]`

In order to add `5` to the array the elements `7,9,12,17` will have to be shifted by one space in memory. Additionally, if we deleted `4` those same elements would have to be shifted in the other direction.

If this data were stored as a linked list instead, it could be represented by the following schema: `{1 -> 4 -> 7 -> 9 -> 12 -> 17}`

Adding `5` to the dataset would be a simple matter of creating a node with a value of five and a pointer pointing to `7`, then modifying the point in `4` to point to the new node rather than `7`. Deleting the `4` node is an even quicker process; the pointer of `1` is changed from `4` to `7`. No memory shifting of multiple elements was required for either of these processes. This demonstrates the key application where lists are more advantageous than arrays is when there is a lot of insertion and deletion of elements with the size changing dynamically and frequently.

## Implementation

As there is no native object definition for a linked list in JavaScript, to use this datatype one must implement it themselves or use a library such as [yallist](https://www.npmjs.com/package/yallist) (over 68M weekly downloads).

[yallist](https://www.npmjs.com/package/yallist)

### Definition

One method of implementing a singly linked list is defining a Node class and a LinkedList class. The node class should contain some data and reference to the next node in the chain. The LinkedList class should contain a head attribute pointing to the first node in the list and some methods pertaining to the list. The below code snippets detail such an implementation. A simple method for printing to the console has been added in the LinkedList definition to illustrate the structure of the list. Obviously this would not be present in a production code base.

```js
class Node {
  constructor(val) {
    this.data = val; // store data in node
    this.next = null; // next is null by default
  }
}
```

```js
class LinkedList {
  constructor(head) {
    this.head = head;
  }

  print() {
    let temp = this.head; // start at beginning of list
    while (temp) {
      // write the data contained within the element
      process.stdout.write(temp.data);

      // traverse to the next node
      temp = temp.next;

      // append arrow if there is a next node
      if (temp) process.stdout.write(" --> ");
    }
    console.log(""); // move cursor to new line once done
  }
}
```

```js
const n1 = new Node("A");
const n2 = new Node("B");
const n3 = new Node("C");
const n4 = new Node("D");
const n5 = new Node("E");

const ll = new LinkedList(n1);
n1.next = n2;
n2.next = n3;
n3.next = n4;
n4.next = n5;

ll.print(); // A --> B --> C --> D --> E
```

### List Traversal

Because there is no means of randomly accessing the elements of a linked list, it is necessary to _traverse_ the list to find any value other than the head. This means looping over each node, checking for a condition, then moving to the node referenced in the pointer if that condition is not met. The essential steps of this pattern are as follows, assuming this is all happening within a method of the LinkedList class defined above:

1. Initialize throw away variable with the first node in the chain `let temp = this.head`
2. Begin a while loop with a condition that will stop the traversal when the temp variable has the desired value. For instance, `while (temp)` to get to the end of the list, or `while (temp && temp.data !== key)` to find a specific data value.
3. Reassign the temp variable within the while loop to be the next element in the array `temp = temp.next`.

## Common Operations

As the strength of linked lists is the efficiency of insertion and deletion of elements, those are the most important methods of this data type. The following code snippets are suggestions for how to implement your own methods on a link list class.

### Inserting a Node

To insert a link in a chain you must break the chain creating two smaller chains, then connect the end of the first and the start of the second to your new link. It is much the same way with lists.

#### Beginning of list

Adding a node to the beginning of the linked list is a simple three-step process taking a constant amount of time. The new node is created with some data, the current head is assigned to be next of the new node and the head of the list is changed to the new node. The length of the list does not affect this operation.

![Screen Shot 2022-03-11 at 4.30.57 PM.png](Linked%20List%20The%20Chain%20Datatype%20c5d630822be04a9abe829f8c36fe5709/Screen_Shot_2022-03-11_at_4.30.57_PM.png)

```js
// method defined within LinkedList class
push(data) {
  const newNode = new Node(data); // allocate the new node with some data
  newNode.next = this.head; // assign new node next to be current head
  this.head = newNode; // replace the current head with the new node
}
```

```js
ll.print(); // A --> B --> C --> D --> E
ll.push("Z");
ll.print(); // Z --> A --> B --> C --> D --> E
```

#### End of list

Because the list is represented by the first node and the storage structure of a linked list is disjointed, the only way to find the last element is by traversing the entire list until a node with no pointer is found.

![Screen Shot 2022-03-11 at 4.31.24 PM.png](Linked%20List%20The%20Chain%20Datatype%20c5d630822be04a9abe829f8c36fe5709/Screen_Shot_2022-03-11_at_4.31.24_PM.png)

```jsx
// method defined within LinkedList class
append(data) {
  const newNode = new Node(data); // allocate the new node with some data

  // if list is empty make new node head
  if (!this.head) {
    this.head = newNode;
    return;
  }

  // traverse until reaching the last node
  let last = this.head;
  while (last.next) {
    last = last.next;
  }

  last.next = newNode; // assign the new node to come after the last
}
```

```jsx
ll.print(); // A --> B --> C --> D --> E
ll.append("Z");
ll.print(); // A --> B --> C --> D --> E --> Z
```

#### Before another Node

Adding a node to the middle of the list is fairly simple as long as the node previous to the desired position is known. The procedure follows a similar pattern to adding a node to the beginning of the list.

![Screen Shot 2022-03-12 at 9.34.22 AM.png](Linked%20List%20The%20Chain%20Datatype%20c5d630822be04a9abe829f8c36fe5709/Screen_Shot_2022-03-12_at_9.34.22_AM.png)

```jsx
// method defined within LinkedList class
insert(prev, data) {
  const newNode = new Node(data); // allocate the new node with some data
  newNode.next = prev.next; // connect new node with previous nodes next
  prev.next = newNode; // connect previous node to new node
}
```

```jsx
ll.print(); // A --> B --> C --> D --> E
ll.insert(n3, "Z");
ll.print(); // A --> B --> C --> Z --> D --> E
```

## Deleting a Node

Removing a link from a chain again involves breaking the chain but this time reattaching the two strands one link closer together.

![Screen Shot 2022-03-12 at 9.44.50 AM.png](Linked%20List%20The%20Chain%20Datatype%20c5d630822be04a9abe829f8c36fe5709/Screen_Shot_2022-03-12_at_9.44.50_AM.png)

### By Data Value

To delete a node by the value it contains the list must first be traversed. Once the node is found the nodes previous and following it are connected. Then the deleted node is removed from memory by assigning it `null`.

```jsx
// method defined within LinkedList class
deleteKey(key) {
  let temp = this.head;
  let prev;

  // check if head node is to be deleted
  if (temp && temp.data === key) {
    this.head = temp.next; // replace head with next in list
    temp = null; // remove deleted node from memory
    return;
  }

  // traverse through list to find first element matching the key
  while (temp) {
    if (temp.data === key) break; // stop traversing if value found
    prev = temp;
    temp = temp.next;
  }

  if (temp === null) return false; // if value not found
  prev.next = temp.next; // knit list together around selected node
  temp = null; // remove deleted node from memory
  return true;
}
```

```jsx
ll.print(); // A --> B --> C --> D --> E
ll.deleteKey("B");
ll.print(); // A --> C --> D --> E
```

## Use Cases

Some example use cases where it would be more performant to use a linked list over an array:

1. Back and forward buttons in an application or web browser to navigate between views
2. Previous and skip buttons in a music application

## References

[Linked List | Set 1 (Introduction) - GeeksforGeeks](https://www.geeksforgeeks.org/linked-list-set-1-introduction/?ref=lbp)

[Explore - LeetCode](https://leetcode.com/explore/learn/card/linked-list/)
