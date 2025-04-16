---
layout: post
title:  "URL Status Checker - Multi Threaded"
date:   2025-04-16 
desc: "This blog will help you understand about Threading concept by concurrent URL requests implemented using Threads. Which is most important thing to be known for daily task"
---




# Building a Multi-Threaded URL Status Checker in Python

In this blog, we’ll walk through creating a Python script that checks the HTTP status of URLs using **multithreading** and **ThreadPoolExecutor**.

---

## Objective

We will:

- Read a list of URLs from a file.
- Use both `threading` and `ThreadPoolExecutor` to make concurrent HTTP requests.
- Handle exceptions gracefully.

---

## Project Structure

```
project/
│
├── urls.txt               # List of URLs to check
└── url_checker.py         # Python script to perform status checks
```

---

## Python Concepts Used

- `threading.Thread` for manual thread control
- `concurrent.futures.ThreadPoolExecutor` for efficient thread pooling
- Exception handling with `try-except`
- File I/O for reading URLs

---

## Python Code

```python
import threading
import requests
from concurrent.futures import ThreadPoolExecutor

def get_urls():
    with open("urls.txt") as file:
        urls = [line.strip() for line in file.readlines() if line.strip()]
        return urls

def hit_url(url):
    try:
        response = requests.get(url)
        print(response)
    except requests.exceptions.RequestException as e:
        print(f"url--{url}---Error: {e}")

def execute(urls):
    threads = []
    for url in urls:
        threads.append(threading.Thread(target=hit_url, args=(url,)))
    for thread in threads:
        thread.start()
    for thread in threads:
        thread.join()

def execute_with_thread_pool(urls, max_thread=5):
    with ThreadPoolExecutor(max_workers=max_thread) as executor:
        executor.map(hit_url, urls)

if __name__ == "__main__":
    urls = get_urls()
    # execute(urls)  # Uncomment to use manual threading
    execute_with_thread_pool(urls)
```

---

## How It Works

1. **get_urls()**: Reads and cleans up URLs from a file.
2. **hit_url(url)**: Makes an HTTP GET request and prints the response or error.
3. **execute()**: Uses manual threads to run `hit_url()` for each URL.
4. **execute_with_thread_pool()**: Uses a thread pool to efficiently manage threads.

---

## Sample `urls.txt`

```
https://www.google.com
https://www.github.com
https://nonexistent123456.com
```

---

## Output

```
<Response [200]>
<Response [200]>
url--https://nonexistent123456.com---Error: ...
```

---

## Conclusion

This script gives you a solid starting point to build more advanced tools using Python's threading capabilities. Try enhancing it i will be trying with:

- Timeout handling
- Retry logic using decorators

Happy coding!
