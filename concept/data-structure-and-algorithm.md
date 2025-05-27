# Title

## Purpose

To solve problems more efficiently, we aim to optimize both time and space complexity to the point where further improvements are not possible.

## Concept

We can decompose all the programming logics into four actions: create, read, update, delete. The **time complexity** of CRUD an element in specific data structure is as follow: (The order is based on popularity)

| Data Structure  | Create       | Read        | Update      | Delete      |
|-----------------|--------------|-------------|-------------|-------------|
| Arrays          | O(1) - O(n)  | O(1)        | O(1)        | O(1) - O(n) |
| Linked Lists    | O(1)         | O(n)        | O(n)        | O(1)        |
| Hash Tables     | O(1)         | O(1)        | O(1)        | O(1)        |
| Trees           | O(log n)     | O(log n)    | O(log n)    | O(log n)    |
| Graphs          | O(1)         | O(v+e)      | O(1)        | O(v+e)      |
| Stacks          | O(1)         | O(n)        | O(n)        | O(1)        |
| Queues          | O(1)         | O(n)        | O(n)        | O(1)        |
| Heaps           | O(log n)     | O(1)        | O(log n)    | O(log n)    |
| Tries           | O(k)         | O(k)        | O(k)        | O(k)        |


In this table, n is the number of elements in the data structure, v is the number of vertices, e is the number of edges, and k is the length of the word for tries. Note that these are average case complexities, and the actual time complexity can vary based on the specific implementation and usage of the data structure.

## Example

* Array: Fast index access, fixed size.
* Linked List: Frequent inserts/deletes.
* Hash Table: Fast lookup by key.
* Tree: Sorted data, range queries.
* Graph: Represent networks.
* Stack: LIFO (e.g., recursion).
* Queue: FIFO (e.g., tasks).
* Heap: Get min/max fast.
* Trie: Prefix searches.
* Union-Find: Group tracking, dynamic connectivity.

## reference

* [cracking the coding interview](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/0984782850)
* [The top data structures you should know for your next coding interview](https://www.freecodecamp.org/news/the-top-data-structures-you-should-know-for-your-next-coding-interview-36af0831f5e3/)
