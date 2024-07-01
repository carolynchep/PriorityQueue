# Priority Queue Implementation


This repository contains an implementation of a generic priority queue data structure in Python, leveraging type hints and a custom Entry class. The priority queue supports typical operations such as insert, remove_min, min, and checking if the queue is empty.

## Overview

The `PriorityQueue` class uses a binary heap (via the `heapq` module) to store its elements, providing an example of how to use generics in Python to create a type-safe data structure. 

**Tree**: a set of nodes storing elements. A binary tree is an ordered tree where
every node has at most two children. The left child precedes a right child in the
ordering of children.

**A heap** is a binary tree T that satisfies the relational property on the way keys are
stored on T and structural property of T

## Features

- **Insert**: Add an element with a priority to the priority queue.
- **Remove Min**: Remove and return the element with the highest priority (lowest key).
- **Min**: Retrieve the element with the highest priority (lowest key) without removing it.
- **Is Empty**: Check if the priority queue is empty.
- **Len**: Get the number of elements in the priority queue.
- **String Representation**: Get a string representation of the priority queue entries.

## Classes

### `EmptyError`

A custom exception class that extends `Exception` to handle priority queue-specific errors.

### `Entry`

A generic class that represents an entry in the priority queue with a key (priority) and value (data).

#### Methods

- `__init__`: Initialize an entry with a key and value.
- `__str__`: Return a string representation of the entry.
- `__eq__`: Compare two entries for equality.
- `__lt__`: Compare the priority of two entries.
- `__repr__`: Return a string representation of the entry for debugging.

### `PriorityQueue`

A generic priority queue class that uses a binary heap to manage its elements.

#### Methods

- `__init__`: Initialize an empty priority queue.
- `__len__`: Return the number of elements in the priority queue.
- `is_empty`: Check if the priority queue is empty.
- `insert`: Add an entry to the priority queue.
- `remove_min`: Remove and return the entry with the highest priority.
- `min`: Return the entry with the highest priority without removing it.

## Usage

### Example

Here's an example of how to use the priority queue:

```
# see https://medium.com/@steveYeah/using-generics-in-python-99010e5056eb
from typing import Generic, TypeVar

K = TypeVar("K", int, float, str)  # allows variable K to be used to represent a generic type
V = TypeVar("V")
E = TypeVar("E")  # used to represent Entry

import heapq
import random
import string

class EmptyError(Exception):
    ''' class extending Exception to better document stack errors '''
    def __init__(self, message: str):
        self.message = message

class Entry(Generic[K,V]):
    def __init__(self, priority: K, data: V):
        self._key   = priority
        self._value = data
    def __str__(self):  return f"({self._key},{self._value})"
    def __eq__(self, other):
        return self._key == other._key and self._value == other._value
    def __lt__(self, other):
        return self._key < other._key
    # not the Pythonic way to use __repr__ but allows us to print list of Entry
    def __repr__(self):
        key = f"'{self._key}'"   if isinstance(self._key,   str) else f"{self._key}"
        val = f"'{self._value}'" if isinstance(self._value, str) else f"{self._value}"
        return f"({key},{val})"

class PriorityQueue(Generic[E]):
    slots = ('_container')

    def __init__(self):
        self._container: list[Entry] = list()

    def __len__(self) -> int:
        return len(self._container)

    def is_empty(self) -> bool:
        return len(self._container) == 0

    def insert(self, key: K, item: V) -> None:
        #create a new entry with key and item
        #insert the entry into the heap
        new_entry = Entry(key, item)
        heapq.heappush(self._container, new_entry)


    def remove_min(self) -> Entry:
        if len(self._container) == 0:
            raise EmptyError("can't remove from empty heap")
        else:
            return heapq.heappop(self._container)


    def min(self) -> Entry:
        if len(self._container) == 0:
            raise EmptyError("can't remove from empty heap")
        else:
            return self._container[0]

def main():
    heap = [1,2,3,4,5,6,7,8,9]
    #hq.heapify(heap)
    print(len(heap))
    print(heap)

    pq = PriorityQueue()
    pq.insert(5, 'task1')
    pq.insert(3, 'task2')
    pq.insert(6, 'task3')
    pq.insert(2, 'task4')

    print(f"Priority Queue: {pq._container}")
    print(f"Minimum element: {pq.min()}")

    print("Removing elements:")
    while not pq.is_empty():
        print(pq.remove_min())

if __name__ == "__main__":
    main()

```
Running the main function should produce the following output:

```
Priority Queue: [(2,task4), (3,task2), (6,task3), (5,task1)]
Minimum element: (2,task4)
Removing elements:
(2,task4)
(3,task2)
(5,task1)
(6,task3)
```
