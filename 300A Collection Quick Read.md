# 🔥 Java Collections Framework (JCF)

## ✅ Definition

* JCF = standard set of interfaces + implementations + algorithms to store and manage groups of objects dynamically
* Covers List, Set, Queue, Map + Collections utility methods
* 💡 Memory shortcut: “JCF = Data structures toolbox of Java”

## 🔹 Must-Remember Points

### Core parts

* Interfaces: List, Set, Queue, Map
* Implementations: ArrayList, HashSet, HashMap, PriorityQueue
* Algorithms: Collections.sort(), reverse(), shuffle()

### Hierarchy

* Iterable → top root for iteration
* Collection → root for List, Set, Queue
* Map → separate hierarchy

> 🔥 Important
> Map does NOT extend Collection
> because it stores key-value pairs, not single objects

### Generics

* compile-time type safety
* removes manual casting
* avoids ClassCastException

### Fail-Fast

* structural modification during iteration → ConcurrentModificationException
* internally uses modCount

### Legacy replacements

* Vector → ArrayList
* Stack → Deque
* Hashtable → HashMap

## ⚡   Quick Comparison

| Topic       | Key Point                             |
| ----------- | ------------------------------------- |
| Collection  | Root interface for object collections |
| Collections | Utility class for algorithms          |
| Map         | Separate key-value hierarchy          |
| Fail-Fast   | Throws exception on unsafe modify     |
| Fail-Safe   | Iterates on snapshot copy             |

## ⚠️ Common Traps

* ❌ Saying Map extends Collection
* ❌ Confusing Collection vs Collections
* ❌ Using Stack instead of Deque
* ❌ Modifying list inside for-each loop
* ❌ Forgetting generics in   examples

> 👉   Tip: Always say “code to interface, not implementation”

---

---

# 🔥 Java Collection Interface

## ✅ Definition

* Collection = root interface of JCF for single-object containers
* Parent of List, Set, Queue
* Defines common operations: add, remove, contains, iterate
* 💡 Memory shortcut: “Collection = common contract, behavior only”

## 🔹 Must-Remember Points

### Root hierarchy

* Iterable → Collection → List / Set / Queue

### Core methods

* add(), remove(), contains()
* size(), isEmpty(), clear()
* iterator()
* bulk: addAll(), removeAll(), retainAll()

> 🔥 Important
> No get() method
> because index access is not valid for all collections
> Set, Queue, PriorityQueue are not index-based

### Bulk ops

* addAll() → merge
* removeAll() → subtract
* retainAll() → intersection

### Safe traversal

* modify during loop → use Iterator.remove()
* direct remove in for-each → fail-fast exception

> 🔥 Important
> contains() complexity depends on implementation
> HashSet → avg O(1)
> ArrayList → O(n)

## ⚡   Quick Comparison

| Method      | Purpose       |   Memory |
| ----------- | ------------- | ---------------- |
| add()       | single insert | one object       |
| addAll()    | bulk insert   | batch jobs       |
| remove()    | remove one    | single cleanup   |
| removeAll() | bulk delete   | fraud cleanup    |
| retainAll() | keep common   | SQL INTERSECT    |

## ⚠️ Common Traps

* ❌ Assuming Collection supports get()
* ❌ Modifying collection directly inside for-each
* ❌ Saying contains() is always O(1)
* ❌ Forgetting equals() + hashCode() for HashSet

> 👉   Tip: Always say behavior depends on implementation

---

---

# 🔥 Java List Interface

## ✅ Definition

* List = ordered collection
* preserves insertion order
* allows duplicates
* supports index-based access
* 💡 Memory shortcut: “List = sequence + duplicates + index”

## 🔹 Must-Remember Points

### Best use cases

* transaction history
* audit logs
* paginated REST DTOs
* EMI schedules

### Main implementations

* ArrayList → dynamic array
* LinkedList → doubly linked list
* Vector → legacy synchronized list

> 🔥 Important
> Default choice = ArrayList
> best for read-heavy APIs
> get(index) → O(1)

### LinkedList

* better for queue / insertion-heavy workflows
* head/tail ops fast
* get(index) → O(n)

### ArrayList growth

* grows by 1.5x
* default first capacity = 10
* 🔥 pre-size for batch loads to avoid GC spikes

### Thread safety

* ArrayList ❌ not thread-safe
* use CopyOnWriteArrayList / synchronized wrapper if needed

## ⚡   Quick Comparison

| Feature       | ArrayList     | LinkedList         | Vector        |
| ------------- | ------------- | ------------------ | ------------- |
| Internal DS   | dynamic array | doubly linked list | dynamic array |
| Read          | O(1)          | O(n)               | O(1)          |
| Middle insert | O(n)          | better             | O(n)          |
| Thread-safe   | No            | No                 | Yes           |
| Modern use    | ✅ default     | specific cases     | ❌ legacy      |

## ⚠️ Common Traps

* ❌ Saying LinkedList.get() is O(1)
* ❌ Using Vector in modern code
* ❌ Forgetting ArrayList resize cost
* ❌ Confusing size vs capacity
* ❌ Using LinkedList for read-heavy APIs

> 👉   Tip: For 90% backend APIs → ArrayList

## 🎯 30-Second Spoken Answer

> “List is used when order, duplicates, and index-based access matter. ArrayList is the default implementation because it provides O(1) random access and excellent cache locality, making it ideal for read-heavy APIs like banking statements. LinkedList should be chosen only for true insertion-heavy or queue-based workflows such as failed transaction retry engines. Vector is legacy and generally avoided.”

---

---

# 🔥 Java Set Interface 

## ✅ Definition

* Set = collection of unique elements
* used when duplicates are not allowed
* best for fast membership checks
* 💡 Memory shortcut: “Set = uniqueness first”

## 🔹 Must-Remember Points

### Main implementations

* HashSet → fastest, no order guarantee
* LinkedHashSet → preserves insertion order
* TreeSet → sorted order

> 🔥 Important
> HashSet internally uses HashMap
> stores element as key
> value = dummy PRESENT object

### Complexities

* HashSet → O(1) avg lookup
* TreeSet → O(log n)

### Null rules

* HashSet → one null allowed
* TreeSet → ❌ no null

### Custom objects

* must override equals() + hashCode()
* TreeSet needs Comparable or Comparator

> 🔥 Critical trap
> mutable fields in hashCode() = broken lookup / duplicate bugs

## ⚡   Quick Comparison

| Feature     | HashSet   | LinkedHashSet         | TreeSet        |
| ----------- | --------- | --------------------- | -------------- |
| Order       | random    | insertion             | sorted         |
| Internal DS | HashMap   | HashMap + linked list | Red-Black Tree |
| Lookup      | O(1) avg  | O(1) avg              | O(log n)       |
| Null        | 1 allowed | 1 allowed             | ❌ no           |

## ⚠️ Common Traps

* ❌ Forgetting equals() + hashCode()
* ❌ Assuming HashSet preserves order
* ❌ Adding null in TreeSet
* ❌ Using mutable fields in equality logic
* ❌ inconsistent compareTo() in TreeSet

> 👉   Tip: Uniqueness = logical equality, not reference equality

## 🌍 Real-World Example

### 🏦 Banking

* HashSet<String> → duplicate settlement prevention
* LinkedHashSet<String> → unique onboarding sequence
* TreeSet<Customer> → sorted VIP customer report

## 🎯 30-Second Spoken Answer

> “Set is used when uniqueness is the primary requirement. HashSet is the default choice because it uses HashMap internally and provides O(1) average lookup using hashCode and equals. LinkedHashSet adds insertion order, while TreeSet keeps data sorted using comparison logic. In banking systems, Set is commonly used for duplicate transaction prevention

---

---

# 🔥 Java Map Interface

## ✅ Definition

* Map stores key → value pairs
* keys must be unique, values can repeat
* best for fast business-key lookup
* 💡 Memory shortcut: “Map = find value by unique key fast”

## 🔹 Must-Remember Points

### Default choice

* HashMap → O(1) avg lookup
* best for caches, registries, token stores

### Important variants

* LinkedHashMap → preserves insertion/access order → LRU cache
* TreeMap → sorted by key → O(log n)
* ConcurrentHashMap → thread-safe shared cache
* Hashtable → ❌ legacy

### 🔥 Important internals

* bucket array + Node<K,V>
* collisions → linked list / Red-Black Tree (Java 8+)
* treeification when:

  * bucket size > 8
  * capacity >= 64

### Defaults

* initial capacity = 16
* load factor = 0.75
* threshold = capacity × loadFactor

> 🔥 Critical rule
> custom key → must override equals() + hashCode()
> prefer immutable keys

## ⚡   Quick Comparison

| Map Type          | Best Use             | Complexity |
| ----------------- | -------------------- | ---------- |
| HashMap           | cache / lookup       | O(1) avg   |
| LinkedHashMap     | LRU / recent history | O(1) avg   |
| TreeMap           | sorted reports       | O(log n)   |
| ConcurrentHashMap | shared cache         | O(1) avg   |
| Hashtable         | legacy only          | slower     |

## ⚠️ Common Traps

* ❌ Saying HashMap is thread-safe
* ❌ Using mutable object as key
* ❌ Forgetting load factor / rehashing
* ❌ Assuming same hash = same key
* ❌ Using Hashtable in modern microservices

> 👉   Tip: Same hash only means same bucket, not duplicate key

## 🌍 Real-World Example

### 🏦 Banking

* HashMap<AccountNo, Account> → account cache
* ConcurrentHashMap<DeviceId, RiskScore> → fraud cache
* LinkedHashMap<Mobile, OTPCount> → recent OTP attempts (LRU)
* TreeMap<Date, Txn> → sorted reconciliation report

## 🎯 30-Second Spoken Answer

> “Map is used for unique key-value based lookup, which is one of the most common backend requirements. HashMap is the default implementation because it provides O(1) average lookup using hashing and bucket arrays, with Java 8 treeification improving collision-heavy cases. LinkedHashMap is ideal for LRU caches, ConcurrentHashMap for shared caches, and TreeMap when sorted reports are needed. In banking systems, Map is heavily used for account caches, fraud registries, and token stores.”

---

---
