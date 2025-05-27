# Title

## Purpose

Learning linked lists is valuable because they provide a flexible and efficient data structure for dynamic memory allocation and manipulation, enabling dynamic resizing and efficient insertion, deletion, and traversal operations compared to fixed-size arrays.

## Concept

### Singly Linked List

* Graph
  ```mermaid
  graph LR
    id1((A)) --> id2((B))
    id2((B)) --> id3((C))
    id3((C)) --> id4((...))
  ```
* Code
  ```javascript
  class Node {
    constructor(value, next = null) {
      this.value = value;
      this.next = next;
    }
  }
  
  class LinkedList {
    constructor() {
      this.head = null;
    }

    // Create
    prepend(value) {
      const newNode = new Node(value);
  
      if (!this.head) {
        this.head = newNode;
      } else {
        newNode.next = this.head;
        this.head = newNode;
      }
    }
  
    
    append(value) {
      const newNode = new Node(value);
      if (!this.head) {
        this.head = newNode;
      } else {
        const tail = this.traverseTo(this.size() - 1);
        tail.next = newNode;
      }
    }
  
    insert(index, value) {
      if (index === 0) {
        this.prepend(value);
      } else if (index >= this.size()) {
        this.append(value);
      } else {
        const newNode = new Node(value);
        const leader = this.traverseTo(index - 1);
        const nextNode = leader.next;
  
        leader.next = newNode;
        newNode.next = nextNode;
      }
    }
  
    // Read
    traverseTo(index) {
      if (index < 0 || index >= this.size()) {
        throw new Error('Index out of bounds');
      }
      let currentNode = this.head;
      for (let i = 0; i < index; i++) {
        currentNode = currentNode.next;
      }
      return currentNode;
    }
  
    size() {
      let count = 0;
      let currentNode = this.head;
  
      while (currentNode !== null) {
        count++;
        currentNode = currentNode.next;
      }
  
      return count;
    }
  
    printList() {
      const list = [];
      let currentNode = this.head;
  
      while (currentNode !== null) {
        list.push(currentNode.value);
        currentNode = currentNode.next;
      }
  
      return list;
    }
  
    // Update
    update(index, value) {
      if (index < 0 || index >= this.size()) {
        throw new Error('Index out of bounds');
      }
  
      const target = this.traverseTo(index);
      target.value = value;
    }
  
    // Delete
    remove(index) {
      if (index < 0 || index >= this.size()) {
        throw new Error('Index out of bounds');
      }
  
      if (index === 0) {
        this.head = this.head.next;
      } else {
        const leader = this.traverseTo(index - 1);
        const unwantedNode = leader.next;
  
        leader.next = unwantedNode.next;
      }
    }
  }

  module.exports = LinkedList;
  ```
### Time complexity
* Create an element on head (prepend): O(1) - Creating a new node and updating the head pointer can be done in constant time without any traversal.
* Insert an element: O(n) - Traversing to the target position to insert the element takes time proportional to the number of nodes linked before that position.
* Read an element: O(n) - Traversing to the target position to read the element takes time proportional to the number of nodes linked before that position.
* Update an element: O(n) - Traversing to the target position to update the element takes time proportional to the number of nodes linked before that position.
* Delete an element: O(n) - Traversing to the target position to delete the element takes time proportional to the number of nodes linked before that position.

### Spec

```javascript
const SinglyLinkedList = require('../examples/singly_linked_list.js');

describe('SinglyLinkedList', () => {
  let testLinkedList;
  beforeEach(() => {
    testLinkedList = new SinglyLinkedList();
    const values = [1, 74, 888, 62, 33];
    for(let i = 0; i < values.length; i++){
      testLinkedList.prepend(values[i]);
    }
  });

  test('#prepend', () => {
    testLinkedList.prepend(0);
    expect(testLinkedList.printList()).toEqual([ 0, 33, 62, 888, 74, 1 ]);
  });

  test('#append', () => {
    testLinkedList.append(0);
    expect(testLinkedList.printList()).toEqual([ 33, 62, 888, 74, 1, 0 ]);
  });

  test('#insert', () => {
    testLinkedList.insert(2, 1000);
    expect(testLinkedList.printList()).toEqual([ 33, 62, 1000, 888, 74, 1 ]);
  });

  test('#traverse', () => {
    expect(testLinkedList.traverseTo(2).value).toEqual(888);
  })

  test('#printList', () => {
    expect(testLinkedList.printList()).toEqual([33, 62, 888, 74, 1]);
  })
  
  test('#update', () => {
    testLinkedList.update(3, 4)
    expect(testLinkedList.printList()).toEqual([33, 62, 888, 4, 1]);
  })
  
  test('#remove', () => {
    testLinkedList.remove(3)
    expect(testLinkedList.printList()).toEqual([33, 62, 888, 1]);
  })
});
```

### Runner Technique

* Concept: two pointers iterates through a linked list at the same time.
* Detecting Cycles
  ```javascript
  function hasCycle(head) {
    let slow = head;
    let fast = head;
  
    while (fast !== null && fast.next !== null) {
      slow = slow.next;
      fast = fast.next.next;
  
      if (slow === fast) {
        return true;
      }
    }
  
    return false;
  }
  ```
* Finding the Middle Node
  ```javascript
  function findMiddle(head) {
    let slow = head;
    let fast = head;
  
    while (fast !== null && fast.next !== null) {
      slow = slow.next;
      fast = fast.next.next;
    }
  
    return slow;
  }
  ```

### Remove Dups

* Problem: Write code to remove duplicates from an unsorted linked list
* Information:
  * Example: from [1, 4, 6, 3, 2, 7, 4, 8, 3] to [1, 4, 6, 3, 2, 7, 8]
  * You can remove any duplicate nodes you want as long as the result are all unique values
* Edge cases: Only one node in this linked list
* Brute force: compare each node with the rest linked nodes
* Best time complexity: Because we need to traverse all the nodes to read their values at least once, the time complexity will be at least O(n).
* Code example
  ```javascript
  function removeDup(linkedList) {
    set = new Set();
    let currentNode = linkedList.head
    let previousNode = null
  
    while (currentNode !== null) {
      if (!set.has(currentNode.value)) {
        set.add(currentNode.value)
      } else {
        previousNode.next = currentNode.next
      }
      previousNode = currentNode
      currentNode = currentNode.next
    }
  
    return linkedList.printList()
  }
  ```
* test
  ```javascript
  const { removeDup } = require('../examples/remove_dup.js');
  const LinkedList = require('../examples/singly_linked_list.js');
  
  describe('RemoveDup', () => {
    let testLinkedList;
    beforeEach(() => {
      testLinkedList = new LinkedList();
      const values = [1, 4, 6, 3, 2, 7, 4, 8, 3];
      for(let i = 0; i < values.length; i++){
        testLinkedList.append(values[i]);
      }
    });
  
    test('#', () => {
      const result = removeDup(testLinkedList)
      expect(result).toEqual([1, 4, 6, 3, 2, 7, 8]);
    });
  });
  ```

### Doubly Linked List

* Graph
  ```mermaid
    graph LR
      id1((A)) --> id2((B))
      id2((B)) --> id1((A))
      id2((B)) --> id3((C))
      id3((C)) --> id2((B))
      id3((C)) --> id4((...))
      id4((...)) --> id3((C))
  ```
* coding example:
  ```javascript
  class Node {
    constructor(data) {
      this.data = data;
      this.prev = null;
      this.next = null;
    }
  }

  class DoublyLinkedList {
    constructor() {
      this.head = null;
      this.tail = null;
    }
  
    // Create
    append(data) { // Create a node on the tail
      const newNode = new Node(data);
      if (this.head === null) {
        this.head = newNode;
        this.tail = newNode;
      } else {
        this.tail.next = newNode;
        newNode.prev = this.tail;
        this.tail = newNode;
      }
    }
  
    prepend(data) { // Create a node on the head
      const newNode = new Node(data);
      if (this.head === null) {
        this.head = newNode;
        this.tail = newNode;
      } else {
        this.head.prev = newNode;
        newNode.next = this.head;
        this.head = newNode;
      }
    }
  
    insert(position, data) { // Create a node at a particular position
      const newNode = new Node(data);
      if (this.head === null) {
        this.head = newNode;
        this.tail = newNode;
      } else if (position === 0) {
        this.prepend(data); // Insert at the head
      } else {
        const nodeOnPosition = this.traverseToIndex(position - 1);
        newNode.next = nodeOnPosition.next;
        newNode.prev = nodeOnPosition;
        
        if (nodeOnPosition.next !== null) {
          nodeOnPosition.next.prev = newNode;
        } else {
          this.tail = newNode; // If inserted at the end, update tail
        }
        
        nodeOnPosition.next = newNode;
      }
    }
  
    // Read
    value(position) { // Return the value of node at a particular position
      const node = this.traverseToIndex(position);
      return node ? node.data : null;
    }
  
    values() { // Return values from head to tail
      let current_node = this.head;
      const result = [];
      while (current_node !== null) {
        result.push(current_node.data);
        current_node = current_node.next;
      }
      return result;
    }
  
    reverseValues() { // Return values from tail to head
      let current_node = this.tail;
      const result = [];
      while (current_node !== null) {
        result.push(current_node.data);
        current_node = current_node.prev;
      }
      return result;
    }
  
    // Traverse
    traverseToIndex(index) {
      let currentNode = this.head;
      for (let i = 0; i < index && currentNode !== null; i++) {
        currentNode = currentNode.next;
      }
      return currentNode;
    }
  
    // Update
    update(position, value) { // Update the value at a particular position
      const node = this.traverseToIndex(position);
      if (node) {
        node.data = value;
      }
    }
  
    // Delete
    remove(position) { // Remove the node at a particular position
      if (this.head === null) return;
  
      let current_node = this.head;
      
      // Remove from head
      if (position === 0) {
        this.head = this.head.next;
        if (this.head !== null) {
          this.head.prev = null;
        } else {
          this.tail = null; // If the list becomes empty
        }
        return;
      }
  
      current_node = this.traverseToIndex(position);
  
      if (current_node === null) return; // Position out of range
      
      // Remove from tail
      if (current_node === this.tail) {
        this.tail = this.tail.prev;
        this.tail.next = null;
        return;
      }
  
      // Remove from middle
      current_node.prev.next = current_node.next;
      current_node.next.prev = current_node.prev;
    }
  }

  module.exports = DoublyLinkedList
  ```
* why we need doubly?
  * Traversal in both directions
* Spec
  ```javascript
  const DoublyLinkedList = require('../examples/doubly_linked_list.js');

  describe('DoublyLinkedList', () => {
    let testLinkedList;
    beforeEach(() => {
      testLinkedList = new DoublyLinkedList();
      const values = [1, 74, 888, 62, 33];
      for(let i = 0; i < values.length; i++){ // 33 <- 62 <- 888 <- 74 <- 1
        testLinkedList.prepend(values[i]);
      }
    });
  
    test('#prepend', () => { // 0 <- 33 <- 62 <- 888 <- 74 <- 1
      testLinkedList.prepend(0);
      expect(testLinkedList.values()).toEqual([ 0, 33, 62, 888, 74, 1 ]);
      expect(testLinkedList.reverseValues()).toEqual([ 1, 74, 888, 62, 33, 0 ]);
    });
  
    test('#append', () => { // 33 <- 62 <- 888 <- 74 <- 1 <- 0
      testLinkedList.append(0);
      expect(testLinkedList.values()).toEqual([ 33, 62, 888, 74, 1, 0 ]);
      expect(testLinkedList.reverseValues()).toEqual([ 0, 1, 74, 888, 62, 33 ]);
    });
  
    test('#insert', () => { // 33 <- 1000 <- 62 <- 888 <- 74 <- 1
      testLinkedList.insert(2, 1000);
      expect(testLinkedList.values()).toEqual([ 33, 1000, 62, 888, 74, 1 ]);
    });
    
    test('#update', () => {
      testLinkedList.update(2, 1000);
      expect(testLinkedList.printList()).toEqual([ 33, 1000, 888, 74, 1 ]);
    })
  
    test('#delete', () => {
      testLinkedList.update(2, 1000);
      expect(testLinkedList.printList()).toEqual([ 33, 1000, 888, 74, 1 ]);
    })
  });
  ```

## reference

cracking the coding interview

[Practical Linked List in Ruby](https://www.rubyguides.com/2017/08/ruby-linked-list/)
[Linked Lists in JavaScript (ES6 code)](https://codeburst.io/linked-lists-in-javascript-es6-code-part-1-6dd349c3dcc3)
[The Jest Object](https://jestjs.io/docs/getting-started)
