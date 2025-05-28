# Title

## Purpose

Heaps are necessary because they provide an efficient way to manage **priority-based** operations, such as retrieving the minimum or maximum element, which is valuable in various applications like scheduling, graph algorithms, and priority queues.

## Concept

Although heap is a complete binary tree, we usually use array to construct heap because of efficient memory usage, sequential access, cache efficiency, simplicity of indexing, and improved space efficiency.

### Min Heap

* Definition: A min heap is a complete binary tree where the value of each node is smaller than or equal to the values of its child nodes, with the minimum value located at the root node.
* Visualization
  ```mermaid
  graph TD
    B((10)) --> C((15))
    B((10)) --> D((20))
    C((15)) --> E((18))
    C((15)) --> F((17))
    D((20)) --> G((25))
    D((20)) --> H((30))
  ```
  Index:  0   1   2   3   4   5   6  
  Value: [10, 15, 20, 18, 17, 25, 30]
* Code example
  ```javascript
  class MinHeap {
    constructor(values = []) {
      this.values = values;
      this.heapify();
    }
  
    insert(value) {
      this.values.push(value);
      this._siftUp(this.values.length - 1);
    }
  
    findMin() {
      return this.values[0];
    }
  
    update(value, index) {
      const old = this.values[index];
      this.values[index] = value;
      if (value < old) this._siftUp(index);
      else this._siftDown(index);
    }
  
    delete() {
      const last = this.values.pop();
      if (this.values.length > 0) {
        this.values[0] = last;
        this._siftDown(0);
      }
    }
  
    heapify() {
      for (let i = Math.floor(this.values.length / 2) - 1; i >= 0; i--) {
        this._siftDown(i);
      }
    }
  
    // _siftUp swaps with the parent if the current node is smaller, repeating until it's in the correct position.
    _siftUp(i) {
      while (i > 0) {
        let parent = Math.floor((i - 1) / 2);
        if (this.values[i] >= this.values[parent]) break;
        [this.values[i], this.values[parent]] = [this.values[parent], this.values[i]];
        i = parent;
      }
    }
  
    // 
    _siftDown(i) {
      const n = this.values.length;
      while (true) {
        let left = 2 * i + 1; // left children
        let right = 2 * i + 2; // right children
        let smallest = i;
        if (left < n && this.values[left] < this.values[smallest]) smallest = left;
        if (right < n && this.values[right] < this.values[smallest]) smallest = right;
        if (smallest === i) break;
        [this.values[i], this.values[smallest]] = [this.values[smallest], this.values[i]];
        i = smallest;
      }
    }
  }
  
  module.exports = MinHeap
  ```
* Spec
  ```javascript
  MinHeap = require('./min_heap.js')

  describe('MinHeap', () => {
    let heap = new MinHeap([5, 12, 8, 3, 9, 7])
  
    // init
    test('#new', () => {
      expect(heap.values).toEqual([3, 5, 7, 12, 9, 8])
    })
  
    // create
    test('#insert', () => {
      heap.insert(4)
      expect(heap.values).toEqual([3, 5, 4, 12, 9, 8, 7])
    })
  
    // read
    test('findMin', () => {
      expect(heap.findMin()).toEqual(3)
    })
  
    // update
    test('#update', () => {
      heap.update(6, 3)
      expect(heap.values).toEqual([3, 5, 7, 6, 9, 8])
    })
  
    // destroy
    test.only('#delete', () => {
      heap.delete()
      expect(heap.values).toEqual([5, 7, 12, 9, 8])
    })
  })
  ```

### Max Heap

## Reference

[Heaps in 3 minutes — Intro](https://www.youtube.com/watch?v=0wPlzMU-k00)
