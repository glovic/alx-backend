# 0x00. Pagination

This project explores the implementation of pagination techniques using Python, focusing on tasks like creating helper functions, simple pagination, and hypermedia pagination.

## Tasks :page_with_curl:

* **0. Simple helper function**
  * **[0-simple_helper_function.py](./0-simple_helper_function.py):** In this task, we create a simple helper function `index_range` that takes two integer arguments: `page` and `page_size`. The function returns a tuple containing the start and end index of the pagination.
  * **Usage:**
    ```python
    print(index_range(1, 10))
    print(index_range(3, 15))
    ```
    **Output:**
    ```python
    (0, 10)
    (30, 45)
    ```

* **1. Simple pagination**
  * **[1-simple_pagination.py](./1-simple_pagination.py):** We implement a basic pagination function that retrieves items from a dataset based on the page and page size.
  * **Usage:**
    ```python
    dataset = [i for i in range(1, 101)]
    print(get_page(dataset, 1, 10))
    print(get_page(dataset, 2, 20))
    ```
    **Output:**
    ```python
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    [21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40]
    ```

* **2. Hypermedia pagination**
  * **[2-hypermedia_pagination.py](./2-hypermedia_pagination.py):** This task introduces hypermedia pagination by extending the previous pagination system. It provides a comprehensive response including the current page, page size, data, and metadata about the dataset.
  * **Usage:**
    ```python
    dataset = [i for i in range(1, 101)]
    print(get_hyper(dataset, 2, 10))
    ```
    **Output:**
    ```python
    {
      'page_size': 10,
      'page': 2,
      'data': [11, 12, 13, 14, 15, 16, 17, 18, 19, 20],
      'next_page': 3,
      'prev_page': 1,
      'total_pages': 10
    }
    ```

* **3. Deletion-resilient hypermedia pagination**
  * **[3-hypermedia_del_pagination.py](./3-hypermedia_del_pagination.py):** In this task, we make the hypermedia pagination system resilient to deletions by ensuring that the pagination remains accurate even if items are removed from the dataset.
  * **Usage:**
    ```python
    dataset = [i for i in range(1, 101)]
    del dataset[5:10]  # Simulating deletion
    print(get_hyper(dataset, 2, 10))
    ```
    **Output:**
    ```python
    {
      'page_size': 10,
      'page': 2,
      'data': [16, 17, 18, 19, 20, 21, 22, 23, 24, 25],
      'next_page': 3,
      'prev_page': 1,
      'total_pages': 9
    }
    ```
