# 0x01. Caching

This project involves creating various caching systems in Python using different caching algorithms. The goal is to implement caching mechanisms with various strategies to understand and apply different eviction policies.

## Tasks :page_with_curl:

* **0. Basic dictionary**
  * **[0-basic_cache.py](./0-basic_cache.py):** Create a class `BasicCache` that inherits from `BaseCaching` and implements a basic caching system. This class does not impose a limit on the number of items in the cache.
    * **Methods:**
      * `put(self, key, item)`: Assigns the item to the cache using the key. Does nothing if key or item is None.
      * `get(self, key)`: Returns the value associated with the key. Returns None if the key does not exist or is None.
    * **Usage:**
      ```python
      my_cache = BasicCache()
      my_cache.put("A", "Hello")
      my_cache.put("B", "World")
      print(my_cache.get("A"))  # Output: Hello
      print(my_cache.get("B"))  # Output: World
      print(my_cache.get("D"))  # Output: None
      ```
      **Expected Output:**
      ```python
      Hello
      World
      None
      ```

* **1. FIFO caching**
  * **[1-fifo_cache.py](./1-fifo_cache.py):** Create a class `FIFOCache` that inherits from `BaseCaching` and implements FIFO (First In, First Out) caching. This class discards the oldest item when the cache exceeds the maximum number of items.
    * **Methods:**
      * `put(self, key, item)`: Adds the item to the cache. Discards the oldest item if the cache exceeds the maximum number of items.
      * `get(self, key)`: Returns the value associated with the key. Returns None if the key does not exist or is None.
    * **Usage:**
      ```python
      my_cache = FIFOCache()
      my_cache.put("A", "Hello")
      my_cache.put("B", "World")
      my_cache.put("C", "Holberton")
      my_cache.put("D", "School")
      my_cache.put("E", "Battery")
      print(my_cache.get("A"))  # Output: None (discarded)
      ```
      **Expected Output:**
      ```python
      None
      ```

* **2. LIFO Caching**
  * **[2-lifo_cache.py](./2-lifo_cache.py):** Create a class `LIFOCache` that inherits from `BaseCaching` and implements LIFO (Last In, First Out) caching. This class discards the most recently added item when the cache exceeds the maximum number of items.
    * **Methods:**
      * `put(self, key, item)`: Adds the item to the cache. Discards the most recently added item if the cache exceeds the maximum number of items.
      * `get(self, key)`: Returns the value associated with the key. Returns None if the key does not exist or is None.
    * **Usage:**
      ```python
      my_cache = LIFOCache()
      my_cache.put("A", "Hello")
      my_cache.put("B", "World")
      my_cache.put("C", "Holberton")
      my_cache.put("D", "School")
      my_cache.put("E", "Battery")
      print(my_cache.get("D"))  # Output: None (discarded)
      ```
      **Expected Output:**
      ```python
      None
      ```

* **3. LRU Caching**
  * **[3-lru_cache.py](./3-lru_cache.py):** Create a class `LRUCache` that inherits from `BaseCaching` and implements LRU (Least Recently Used) caching. This class discards the least recently used item when the cache exceeds the maximum number of items.
    * **Methods:**
      * `put(self, key, item)`: Adds the item to the cache. Discards the least recently used item if the cache exceeds the maximum number of items.
      * `get(self, key)`: Returns the value associated with the key. Returns None if the key does not exist or is None.
    * **Usage:**
      ```python
      my_cache = LRUCache()
      my_cache.put("A", "Hello")
      my_cache.put("B", "World")
      my_cache.put("C", "Holberton")
      my_cache.put("D", "School")
      my_cache.put("E", "Battery")
      print(my_cache.get("B"))  # Output: World
      ```
      **Expected Output:**
      ```python
      World
      ```

* **4. MRU Caching**
  * **[4-mru_cache.py](./4-mru_cache.py):** Create a class `MRUCache` that inherits from `BaseCaching` and implements MRU (Most Recently Used) caching. This class discards the most recently used item when the cache exceeds the maximum number of items.
    * **Methods:**
      * `put(self, key, item)`: Adds the item to the cache. Discards the most recently used item if the cache exceeds the maximum number of items.
      * `get(self, key)`: Returns the value associated with the key. Returns None if the key does not exist or is None.
    * **Usage:**
      ```python
      my_cache = MRUCache()
      my_cache.put("A", "Hello")
      my_cache.put("B", "World")
      my_cache.put("C", "Holberton")
      my_cache.put("D", "School")
      my_cache.put("E", "Battery")
      print(my_cache.get("B"))  # Output: World
      ```
      **Expected Output:**
      ```python
      World
      ```

* **5. LFU Caching**
  * **[100-lfu_cache.py](./100-lfu_cache.py):** Create a class `LFUCache` that inherits from `BaseCaching` and implements LFU (Least Frequently Used) caching. This class discards the least frequently used item when the cache exceeds the maximum number of items. If there are multiple items with the same lowest frequency, it uses the LRU (Least Recently Used) algorithm to discard the least recently used among them.
    * **Methods:**
      * `put(self, key, item)`: Adds the item to the cache. Discards the least frequently used item if the cache exceeds the maximum number of items, using LRU to break ties. Prints `DISCARD:` followed by the discarded `key`.
      * `get(self, key)`: Returns the value associated with the key. Returns None if the key does not exist or is None.
    * **Usage:**
      ```python
      my_cache = LFUCache()
      my_cache.put("A", "Hello")
      my_cache.put("B", "World")
      my_cache.put("C", "Holberton")
      my_cache.put("D", "School")
      my_cache.print_cache()
      print(my_cache.get("B"))  # Output: World
      my_cache.put("E", "Battery")
      my_cache.print_cache()
      my_cache.put("C", "Street")
      my_cache.print_cache()
      print(my_cache.get("A"))  # Output: None (discarded)
      print(my_cache.get("B"))  # Output: World
      print(my_cache.get("C"))  # Output: Street
      my_cache.put("F", "Mission")
      my_cache.print_cache()
      my_cache.put("G", "San Francisco")
      my_cache.print_cache()
      my_cache.put("H", "H")
      my_cache.print_cache()
      my_cache.put("I", "I")
      my_cache.print_cache()
      print(my_cache.get("I"))  # Output: I
      print(my_cache.get("H"))  # Output: H
      print(my_cache.get("I"))  # Output: I
      print(my_cache.get("H"))  # Output: H
      print(my_cache.get("I"))  # Output: I
      print(my_cache.get("H"))  # Output: H
      my_cache.put("J", "J")
      my_cache.print_cache()
      my_cache.put("K", "K")
      my_cache.print_cache()
      my_cache.put("L", "L")
      my_cache.print_cache()
      my_cache.put("M", "M")
      my_cache.print_cache()
      ```
      **Expected Output:**
      ```python
      World
      None
      Street
      I
      H
      I
      H
      I
      H
      ```
