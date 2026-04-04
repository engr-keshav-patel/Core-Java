# Day 3 — Collections Framework, `HashMap` Internals, Comparable vs Comparator Notes


# 1) Collections Framework Overview

## ✅ What is Collections Framework

* A unified architecture to **store, manipulate, and retrieve groups of objects**
* Provides:

  * interfaces
  * implementations
  * utility algorithms
* Reduces custom data structure code
* Generic and reusable

## ✅ Core interfaces

* `List`
* `Set`
* `Queue`
* `Map` *(separate hierarchy)*

## Mermaid Hierarchy

```mermaid
flowchart TD
    A[Iterable]
    A --> B[Collection]
    B --> C[List]
    B --> D[Set]
    B --> E[Queue]
    C --> F[ArrayList]
    C --> G[LinkedList]
    D --> H[HashSet]
    D --> I[LinkedHashSet]
    D --> J[TreeSet]
    E --> K[PriorityQueue]
    L[Map] --> M[HashMap]
    L --> N[LinkedHashMap]
    L --> O[TreeMap]
```

## 📝 Points to remember

* `Map` does **not extend Collection**
* choose DS based on:

  * ordering
  * duplicates
  * lookup speed
  * sorting needs

---

# 2) List Implementations

## ✅ `ArrayList`

* dynamic array
* maintains insertion order
* allows duplicates
* fast random access: **O(1)**
* slow middle insertion/deletion: **O(n)**

```java
List<String> list = new ArrayList<>();
list.add("A");
```

## ✅ `LinkedList`

* doubly linked list
* fast insertion/deletion in middle: **O(1)** (after node reach)
* slower random access: **O(n)**
* also implements `Deque`

## 🎯 Interview trap

* frequent reads → `ArrayList`
* frequent inserts/deletes → `LinkedList`

---

# 3) Set Implementations

## ✅ `HashSet`

* no duplicates
* unordered
* backed by `HashMap`
* average add/search: **O(1)**

```java
Set<Integer> set = new HashSet<>();
```

## ✅ `LinkedHashSet`

* insertion order maintained
* uses linked structure

## ✅ `TreeSet`

* sorted ascending by default
* Red-Black Tree based
* operations: **O(log n)**

## 📝 Remember

* duplicate detection uses:

  * `hashCode()`
  * `equals()`

---

# 4) Queue Implementations

## ✅ `PriorityQueue`

* elements ordered by priority
* natural ordering / comparator
* min-heap internally
* insertion/removal: **O(log n)**

```java
Queue<Integer> pq = new PriorityQueue<>();
```

## ✅ `ArrayDeque`

* faster than `Stack`
* supports add/remove from both ends

---

# 5) Map Implementations

## ✅ `HashMap`

* key-value pairs
* one null key, multiple null values
* unordered
* average operations: **O(1)**

## ✅ `LinkedHashMap`

* insertion/access order maintained
* useful for LRU cache

## ✅ `TreeMap`

* sorted by key
* Red-Black Tree
* operations: **O(log n)**

## ✅ `Hashtable`

* synchronized
* legacy
* no null key/value

---

# 6) `HashMap` Internal Working ⭐

## ✅ Put flow

1. compute `hashCode()` of key
2. convert to bucket index
3. if bucket empty → insert
4. if collision → linked list / tree node
5. duplicate key → `equals()` check + replace value

## Mermaid Internal Flow

```mermaid
flowchart TD
    A["key.hashCode()"] --> B[hash spread]
    B --> C[bucket index]
    C --> D{bucket empty?}
    D -->|yes| E[insert node]
    D -->|no| F[equals check]
    F --> G[replace or chain]
```

## ✅ Java 8 optimization

* if bucket nodes > **8** → linked list becomes **Red-Black Tree**
* improves worst-case from **O(n)** to **O(log n)**

## 📝 Most important interview line

> `HashMap` uses **hashCode for bucket selection** and **equals for exact key match**.

---

# 7) Collision Handling

## ✅ What is collision

When multiple keys map to same bucket.

## Example

```java
map.put("FB", 1);
map.put("Ea", 2);
```

These famously produce same hash in many Java versions.

## ✅ Handling

* Java 7 → linked list
* Java 8 → linked list → tree after threshold

## 📝 Remember thresholds

* treeify threshold = **8**
* untreeify threshold = **6**
* min capacity for treeify = **64**

---

# 8) `equals()` + `hashCode()` in Collections

## ✅ Why mandatory

For custom objects in:

* `HashMap`
* `HashSet`
* `LinkedHashSet`

```java
class User {
    int id;

    @Override
    public boolean equals(Object o) {
        return this.id == ((User) o).id;
    }

    @Override
    public int hashCode() {
        return Integer.hashCode(id);
    }
}
```

## 🎯 Interview trap

* same `equals`
* different `hashCode`
* duplicate objects may enter `HashSet`

---

# 9) Fail-Fast vs Fail-Safe

## ✅ Fail-Fast

* throws `ConcurrentModificationException`
* if collection modified during iteration
* examples:

  * `ArrayList`
  * `HashMap`

```java
for (String s : list) {
    list.add("x");
}
```

## ✅ Fail-Safe

* iterates over cloned snapshot
* no exception
* examples:

  * `CopyOnWriteArrayList`
  * `ConcurrentHashMap`

## Mermaid Concept

```mermaid
flowchart LR
    A[Original Collection] --> B[Iterator Snapshot]
```

## 📝 Must remember

* fail-fast = best effort detection
* uses `modCount`

---

# 10) Comparable vs Comparator ⭐

## ✅ `Comparable`

* natural ordering
* class itself defines sort logic
* method: `compareTo()`

```java
class Employee implements Comparable<Employee> {
    int id;
    public int compareTo(Employee e) {
        return this.id - e.id;
    }
}
```

## ✅ `Comparator`

* external sorting logic
* multiple sort strategies possible

```java
Comparator<Employee> byName = (a, b) -> a.name.compareTo(b.name);
```

## 📌 Difference Table

| Topic            | Comparable    | Comparator  |
| ---------------- | ------------- | ----------- |
| package          | `java.lang`   | `java.util` |
| method           | `compareTo()` | `compare()` |
| logic location   | inside class  | external    |
| multiple sorting | no            | yes         |

## 🎯 Interview usage

* one default sort → `Comparable`
* many custom sorts → `Comparator`

---

# 11) `ConcurrentHashMap` Basics

## ✅ Why needed

`HashMap` is not thread-safe.

## ✅ Benefits

* thread-safe
* better performance than `Hashtable`
* segment/bucket level locking (historically)
* modern CAS + synchronized bins

## 📝 Important

* no `ConcurrentModificationException` during concurrent iteration

---

# 12) Common Collections Coding Problems

## ✅ Must practice

* frequency map
* remove duplicates
* sort custom objects
* top K frequent elements
* first non-repeating char
* group anagrams
* LRU cache using `LinkedHashMap`
* merge intervals using list sort
* map values by frequency

---

# 13) Most Important Trap Concepts

## ✅ Must remember bullets

* `HashSet` internally uses `HashMap`
* `TreeSet` uses Red-Black Tree
* `HashMap` allows one null key
* collision uses chaining/treeification
* treeify threshold = 8
* `modCount` used in fail-fast
* `Map` separate hierarchy
* `Comparator` supports multiple sorts
* `PriorityQueue` uses heap
* `LinkedHashMap` useful for LRU

---

# 14) Day 3 Revision Cheatsheet

## ⚡ Fast recall bullets

* `ArrayList` = fast read
* `LinkedList` = fast insert/delete
* `HashSet` = unique unordered
* `TreeSet` = sorted
* `HashMap` = O(1) avg
* hash → bucket → equals
* collision → list/tree
* fail-fast → modCount
* `Comparable` = default sort
* `Comparator` = custom sort

---

# 15) Interview Answer Framework

For each collections answer use:

1. data structure type
2. ordering + duplicates
3. time complexity
4. internal DS used
5. best use case
6. trap edge case
