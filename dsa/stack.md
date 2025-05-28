# Title

## Purpose

Learning about stacks is essential for understanding how function calls and recursion work in programming languages, as well as for organizing and managing data efficiently.

## Concept

![stack](/assets/img/stack.png)
[Image Source](https://www.geeksforgeeks.org/stack-data-structure/)

Stack uses mechanism of first in last out (FILO), we can only add or pop the top element.

* Coding example:
  ```javascript
  class Stack {
    constructor() {
      this.items = [];
    }
  
    push(element) {
      this.items.push(element);
    }
  
    pop() {
      if (this.items.length === 0) {
        return "Underflow";
      }
      return this.items.pop();
    }
  
    peek() {
      return this.items[this.items.length - 1];
    }
  
    isEmpty() {
      return this.items.length === 0;
    }
  
    size() {
      return this.items.length;
    }
  
    print() {
      console.log(this.items.toString());
    }
  }
  ```
* spec
  ```javascript
  const Stack = require('../examples/stack.js');
  
  describe('Stack', () => {
    describe('with item', () => {
      let testStack;
      beforeEach(() => {
        testStack = new Stack();
        const values = [1, 74, 888, 62, 33];
        for(let i = 0; i < values.length; i++){
            testStack.push(values[i]);
        }
      });
  
      test('#push', () => {
        testStack.push(0);
        expect(testStack.print()).toEqual([ 1, 74, 888, 62, 33, 0 ]);
      });
  
      test('#pop', () => {
        testStack.pop()
        expect(testStack.print()).toEqual([ 1, 74, 888, 62 ]);
      })
  
      test('#peek', () => {
        expect(testStack.peek()).toEqual(33);
      })
  
      test('#isEmpty', () => {
        expect(testStack.isEmpty()).toEqual(false);
      })
  
      test('#size', () => {
        expect(testStack.size()).toEqual(testStack.items.length);
      })
    })
  
    describe('no item', () => {
      let testStack = new Stack();
      test('#pop', () => {
        expect(testStack.pop()).toEqual("Underflow");
      })
  
      test('#isEmpty', () => {
        expect(testStack.isEmpty()).toEqual(true);
      })
    })
  });
  ```
* Time complexity
  * Create an item: O(1)
    * Simply add an item on the stack.
  * Read an item: O(1)
    * We can only access the top, so the time complexity of this operation is O(1), which is constant time complexity.
  * Update an item: O(1)
    * Again, we can only access the top, so the time complexity of this operation is O(1).
  * Delete an item: O(1)
    * Again, we can only access the top, so the time complexity of this operation is O(1).

### Three in One

* Problem: Use a single array to implement three stacks
* Example:
  ```javascript
  // array length = 15
  [
    1, 2, 3, 4, null
    1, 2, 3, null, null
    1, null, null, null, null
  ]

  // push(3, 4)
  [
    1, 2, 3, 4, null
    1, 2, 3, null, null
    1, 4, null, null, null // it will push 3 to the third sub-array
  ]

  // pop(3)
  [
    1, 2, 3, 4, null
    1, 2, 3, null, null
    null, null, null, null, null // it will pop out the element in third sub array
  ]
  ```
* Implement (just divide array into three portion)
  ```javascript
  class threeStacksInOneArray {
    // divide array equally
    // all start from 1, whichStack and position
    constructor(arrayLength) {
      this.elements = new Array(arrayLength).fill(null);
      this.starts = [0, Math.round(arrayLength / 3), Math.round(arrayLength / 3) * 2]
      this.ends = [Math.round(arrayLength / 3) - 1, Math.round(arrayLength / 3) * 2 - 1, arrayLength - 1]
      this.addAts = [0, Math.round(arrayLength / 3), Math.round(arrayLength / 3) * 2]
    }

    push(whichStack, value) {
      if (this.addAts[whichStack] == this.ends[whichStack]) {
        return `no more space for ${whichStack + 1}th stack`
      }
      this.elements[this.addAts[whichStack]] = value
      this.addAts[whichStack] += 1
    }

    pop(whichStack) {
      if (this.addAts[whichStack] == this.ends[whichStack]) {
        return `no more value to pop in ${whichStack + 1}th stack`
      }
      this.elements[this.addAts[whichStack] - 1] = null
      this.addAts[whichStack] -= 1
    }

    peek(whichStack) {
      return this.elements[this.addAts[whichStack - 1] - 1]
    }

    isEmpty(whichStack) {
      const start = this.starts[whichStack - 1]
      const end = this.ends[whichStack - 1]
      const subStack = []
  
      for (let i = start; i < end; i++) {
        subStack.push(this.elements[i]);
      }
  
      return subStack.every(element => { return element === null; })
    }

    size(whichStack) {
      const start = this.starts[whichStack - 1]
      const end = this.ends[whichStack - 1]
      let result = 0
  
      for (let i = start; i < end; i++) {
        if (this.elements[i] !== null) {
          result += 1
        }
      }
  
      return result
    }
  
    print(whichStack) {
      const start = this.starts[whichStack - 1]
      const end = this.ends[whichStack - 1]
      const subStack = []
  
      for (let i = start; i <= end; i++) {
        subStack.push(this.elements[i]);
      }
  
      return subStack
    }
  }

  module.exports = threeStacksInOneArray;
  ```
* spec
  ```javascript
  const ThreeStackInOneArray = require('../examples/three_stacks_in_one_array.js');

  describe('ThreeStackInOneArray', () => {
    let testStack;
    let totalLength = 17;
    let data = [
      [1, 74],
      [888, 62, 33],
      [83, 44],
    ]
    beforeEach(() => {
      testStack = new ThreeStackInOneArray(totalLength);
      for(let i = 0; i < data.length; i++) {
        for (let j = 0; j < data[i].length; j ++) {
          testStack.push(i, data[i][j]);
        }
      }
    });
  
    test('#init', () => {
      expect(testStack.elements).toEqual([
        1, 74, null, null, null, null,
        888, 62, 33, null, null, null,
        83, 44, null, null, null,
      ])
      expect(testStack.addAts).toEqual([2, 9, 14])
      expect(testStack.ends).toEqual([5, 11, 16])
    })
  
    test('#push', () => {
      testStack.push(0, 4);
      expect(testStack.elements).toEqual([
        1, 74, 4, null, null, null,
        888, 62, 33, null, null, null,
        83, 44, null, null, null,
      ]);
      expect(testStack.addAts).toEqual([3, 9, 14])
    });
  
    test('#pop', () => {
      testStack.pop(1);
      expect(testStack.elements).toEqual([
        1, 74, null, null, null, null,
        888, 62, null, null, null, null,
        83, 44, null, null, null,
      ]);
      expect(testStack.addAts).toEqual([2, 8, 14])
    })
  
    test('#peek', () => {
      expect(testStack.peek(1)).toEqual(74)
      expect(testStack.peek(2)).toEqual(33)
      expect(testStack.peek(3)).toEqual(44)
    })
  
    test('#size', () => {
      expect(testStack.size(1)).toEqual(2)
      expect(testStack.size(2)).toEqual(3)
      expect(testStack.size(3)).toEqual(2)
    })
  
    test('#print', () => {
      expect(testStack.print(1)).toEqual([1, 74, null, null, null, null])
      expect(testStack.print(2)).toEqual([888, 62, 33, null, null, null])
      expect(testStack.print(3)).toEqual([83, 44, null, null, null])
    })
  
    describe('with empty sub stack', () => {
      let testStack;
      let totalLength = 17;
      let data = [
        [1, 74],
        [888, 62, 33],
        [],
      ]
      beforeEach(() => {
        testStack = new ThreeStackInOneArray(totalLength);
        for(let i = 0; i < data.length; i++) {
          for (let j = 0; j < data[i].length; j ++) {
            testStack.push(i, data[i][j]);
          }
        }
      });
  
      test('#isEmpty', () => {
        expect(testStack.isEmpty(1)).toEqual(false)
        expect(testStack.isEmpty(2)).toEqual(false)
        expect(testStack.isEmpty(3)).toEqual(true)
      })
    })
  });
  ```

### Stack of Plates

* Question: Imagine a (literal) stack of plates. If the stack gets too high, it might topple. Therefore, in real life, we would likely start a new stack when the previous stack exceeds some threshold. Implement a data structure SetOfStacks that mimics this. SetOfStacks should be composed of several stacks and should create a new stack once the previous one exceeds capacity. SetOfStacks .push() and SetOfStacks .pop() should behave identically to a single stack (that is, pop( ) should return the same values as it would if there were just a single stack).
* Example:
  * I think there is only one stack to be created when previous one exceeds capacity.
  * Also, I think I can use index to check whether new one is empty so that I can pop.
  * Also, I think I can use index to tell program which stack to push value.
* Code
  ```javascript
  class SetOfStacks {
    constructor(capacity) {
      this.capacity = capacity
      this.stacks = []
      this.currentStack = null
    }

    this.push(value) {
      if(this.currentStack === null) {
        this.currentStack = new Stack()
        this.currentStack.push(value)
      } else if(this.currentStack.size() >= this.capacity) {
        this.stacks.push(this.currentStack)
        this.currentStack = new Stack()
      } else {
        this.currentStack.push(value)
      }
    }

    this.pop(value) {
      if(this.currentStack === null && this.stacks.length === 0) {
        return 'no element'
      } else if(this.currentStack.length === 0) {
        this.currentStack = this.stacks.pop()
        this.currentStack.pop()
      } else {
        this.currentStack.pop()
      }
    }
  }
  ```
* Test
  ```javascript
  describe('set of stacks', () => {
    let setOfStacks
    describe('capacity === 3', () => {
      setOfStacks = new SetOfStacks(3)
      describe('existed elements = [1, 2, 3, 4, 5, 6]', () => {
        beforeEach(() => {
          [1, 2, 3, 4, 5, 6].forEach((element) => {
            setOfStacks.push(element)
          })
        })
        test('#push 7', () => {
          setOfStacks.push(7)
          expect(setOfStacks.stacks.length).toEqual(3)
        })
      })

      describe('three stacks but last stack is empty', () => {
        beforeEach(() => {
          [1, 2, 3, 4, 5, 6, 7].forEach((element) => {
            setOfStacks.push(element)
          })
          setOfStacks.pop()
        })
        test('#pop', () => {
          expect(setOfStacks.pop()).toEqual(6)
          expect(setOfStacks.stacks.length).toEqual(2)
        })
      })

      describe('existed elements is none', () => {
        test('#pop', () => {
          expect(setOfStacks.pop()).toEqual('no element')
        })
      })
    })
  })
  ```

## Reference

cracking the coding interview

[Three In One: How to Implement 3 Stacks Using 1 Array](https://www.youtube.com/watch?v=TKzVzobAI8E)
