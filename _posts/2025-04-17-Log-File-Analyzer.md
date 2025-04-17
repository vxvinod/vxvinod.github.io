---
layout: post
title:  "Log File Analyzer"
date:   2025-04-17 
desc: "In this blog we will be learning about reading files and iterating line by line, use dictionary to maintain the count, with good class structured."
---


# Python Log File Analyzer

In this blog, we’ll walk through building a simple but powerful **Log File Analyzer** using Python. This project helps you practice working with:

- File I/O
- String parsing
- Dictionary counting
- Object-Oriented Programming (OOP)
- JSON output

## Problem Statement

You are given a log file `server.log` with lines like:

```
[INFO] 2025-04-16 09:15:23 - User login: alice
[ERROR] 2025-04-16 09:16:00 - Database connection failed
[WARNING] 2025-04-16 09:16:45 - Low disk space on server
```

Your task is to:

1. Read the file line by line
2. Extract the log level from each line (`INFO`, `ERROR`, `WARNING`, etc.)
3. Count occurrences of each log level
4. Save the result to a JSON file

---

## Project Structure

We encapsulate the logic inside a `LogAnalyzer` class with methods for reading, parsing, and exporting data.

## Final Python Script

```python
import json

class LogAnalyzer:
    def __init__(self):
        self.file_content = None
        self.log_levels_count = {}

    def read_log(self, file_path):
        with open(file_path, 'r') as file:
            self.file_content = file.readlines()

    def print_content(self):
        print(self.file_content)

    def extract_count_from_log_file(self):
        for line in self.file_content:
            if line.startswith('['):
                log_level = line.split(']')[0][1:]
                self.log_levels_count[log_level] = self.log_levels_count.get(log_level, 0) + 1

    def save_to_json(self, out_file='log_stats.json'):
        with open(out_file, 'w') as json_file:
            json.dump(self.log_levels_count, json_file, indent=4)

if __name__ == '__main__':
    la = LogAnalyzer()
    la.read_log('server.log')
    la.extract_count_from_log_file()
    print(la.log_levels_count)
    la.save_to_json()
```

---

## Sample `server.log`

```
[INFO] 2025-04-16 09:15:23 - User login: alice
[ERROR] 2025-04-16 09:16:00 - Database connection failed
[WARNING] 2025-04-16 09:16:45 - Low disk space on server
[INFO] 2025-04-16 09:17:12 - File uploaded: report.pdf
[DEBUG] 2025-04-16 09:18:34 - Cache refresh triggered
[INFO] 2025-04-16 09:19:10 - User logout: alice
[ERROR] 2025-04-16 09:20:22 - API timeout on /users
[WARNING] 2025-04-16 09:21:01 - High memory usage
[INFO] 2025-04-16 09:22:30 - User login: bob
[DEBUG] 2025-04-16 09:23:10 - Session cleanup
```

---

## Output

Printed in terminal:

```
{'INFO': 3, 'ERROR': 2, 'WARNING': 2, 'DEBUG': 2}
```

Saved in `log_stats.json`:

```json
{
    "INFO": 3,
    "ERROR": 2,
    "WARNING": 2,
    "DEBUG": 2
}
```

---

## What We have Learned

- Reading files line by line
- Extracting structured data from raw text
- Using dictionaries to count values
- Writing JSON output
- Structuring code using a class

Happy Coding!!!