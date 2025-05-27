# Title

## Purpose

Array is one of the most fundamental and widely used data structures, offering efficient storage and retrieval, simple implementation, versatility, efficiency, and being widely used in many fields of computer science.

## Concept

### Time complexity

* Create an element: O(1) to O(N)
  * Inserting an element at the end of an arrayList has O(1) but may be O(N) if we need to resize the arrayList
  * Inserting an element at the beginning of an arrayList has O(N) because we need to shift all the elements
* Read an element: O(1)
  * Given an index i, the read operation in an ArrayList retrieves the element at that index in constant time, regardless of the size of the ArrayList because the addresses of the items are stored **continuously** in memory, which means it can access other addresses by quick calculation of `memory_address = base_address + (index * element_size)`.
* Update an element: O(1)
  * Given an index i and a new value v, the update operation in an ArrayList replaces the element at index i with the new value v in constant time, regardless of the size of the ArrayList.
* Delete an element: O(1) to O(N)
  * In the worst case, where n is the number of elements in the ArrayList because deleting an element requires shifting all subsequent elements one position to the left to fill the gap left by the deleted element. Deleting the last element in an ArrayList has a time complexity of O(1) on average because it can be done by simply updating the size of the ArrayList.

### String

Because the algorithm of string is just like arrayList, so I will put string algorithm in arrayList.

### Array vs ArrayList

An array is a fixed-size data structure, while an ArrayList is a dynamic, resizable data structure.

### Basic Form

In javascript,

#### Example

```javascript
[1, 2, 3, 4]
```

* Data: A collection of elements of the **same data type**, arranged in a contiguous block of memory

### Is Unique (string)

Question: Implement an algorithm to determine if a string has all unique characters

#### Brute force

```javascript
function charactersIsUnique(s) {
  if (typeof s !== 'string') return 'type error';
  for (let i = 0; i < s.length; i++) {
    for (let j = 0; j < s.length; j++) {
      if (s[i] === s[j]) return 'not unique';
    }
  }
  return 'unique';
}
```
The time complexity is $$O(N^2)$$.

#### Unnecessary work

Actually, for any character, we only need to compare the characters on their right-hand side, so we can rewrite it as follow:

```javascript
function charactersIsUnique(s) {
  if (typeof s !== 'string') return 'type error';
  for (let i = 0; i < s.length; i++) {
    for (let j = i + 1; j < s.length; j++) {
      if (s[i] === s[j]) return 'not unique';
    }
  }
  return 'unique';
}
```

Then the number of times will be $$N + (N-1) + ... + 1 = N(1+N)/2$$; however, the time complexity is still $$O(N^2)$$

#### Bottleneck

The place with highest time complexity is the nested for loops, so we should try to use only one for loop. What we can do here is use a hash as follow:

```javascript
function charactersIsUnique(s) {
  if (typeof s !== 'string') return 'wrong type';
  const seen = {};
  for (let i = 0; i < s.length; i++) {
    if (seen[s[i]]) return 'not unique';
    seen[s[i]] = true;
  }
  return 'unique';
}
```

Then the time complexity reduces to $$O(N)$$

#### best time complexity

We at least need to walk through all letters, so it is at least O(N). And we actually only need to walk through once.

#### Test

```javascript
describe('Is Unique', () => {
  describe('s = example', () => {
    let s = 'example'
    expect(charactersIsUnique(s)).toEql(false)
  })
  describe('s = fast', () => {
    let s = 'fast'
    expect(charactersIsUnique(s)).toEql(true)
  })
})
```

### Rotate Matrix

Given an image represented by an NxN matrix, where each pixel in the image is 4 bytes, write a method to rotate the image by 90 degrees. Can you do this in place?

* Example
  * The possible input would be
    ```javascript
    input = [
      [1, 2, 3],
      [4, 5, 6],
      [7, 8, 9],
    ]
    ```
  * And the output of the input above
    ```javascript
    output = [
      [7, 4, 1],
      [8, 5, 2],
      [9, 6, 3]
    ]
    ```

#### Time complexity

Since we must need to walk through all the elements, the time complexity will be at least O(N^2).

#### Code

```javascript
function rotateMatrix(matrix) {
  const n = matrix.length;

  // Transpose
  for (let i = 0; i < n; i++) {
    for (let j = i + 1; j < n; j++) {
      [matrix[i][j], matrix[j][i]] = [matrix[j][i], matrix[i][j]];
    }
  }

  // Reverse each row
  for (let i = 0; i < n; i++) {
    matrix[i].reverse();
  }

  return matrix;
}
```

#### Test

```javascript
describe('rotate matrix', () => {
  let input = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9],
  ]
  test('#', () => {
    expect(rotateMatrix(input)).toEqual([
      [7, 4, 1],
      [8, 5, 2],
      [9, 6, 3]
    ])
  })
})
```

## Reference

cracking the coding interview
