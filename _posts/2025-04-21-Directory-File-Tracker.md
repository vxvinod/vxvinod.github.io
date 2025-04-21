---
layout: post
title:  "Directory File Tracker"
date:   2025-04-21 
desc: "In this blog we will be learning about to fetch the files information under a directory. I learnt about os library, csv output"
---

# Directory File Tracker in Python

This blog post explains how to build a **Directory File Tracker** in Python that scans a directory, gathers metadata for each file, and exports the results into a CSV file. The script leverages key modules like `os`, `csv`, and `datetime` for file operations and report generation.

---

## What This Project Does

- Recursively scans directories for all files.
- Collects file metadata like:
  - File name
  - File size (in KB)
  - Last modified time
  - File type (extension)
  - Full path
- Generates a CSV report.

---

## Python Code

```python
import csv
import os
from _datetime import datetime

class DirectoyFileTracker:

    def __init__(self, directory):
        self.directory = directory
        self.file_list = []

    def get_all_files(self, file_path):
        for path, _, files in os.walk(file_path):
            for file in files:
                yield os.path.join(path, file)

    def get_file_metadata(self, file_path):
        stat = os.stat(file_path)
        return {
            "file_name": os.path.basename(file_path),
            "file_size_kb": round(stat.st_size/1024,2),
            "last_modified": datetime.fromtimestamp(stat.st_mtime).strftime('%Y-%m-%d %H:%M:%S'),
            "file_type": os.path.splitext(file_path)[1],
            "file_path": file_path
        }

    def scan(self, file_path):
        files = list(self.get_all_files(file_path))
        for path in files:
            meta = self.get_file_metadata(path)
            if meta:
                self.file_list.append(meta)
        return True

    def write_csv_report(self, output_file='file_report.csv'):
        if not self.file_list:
            print("No files to write")
        field_names = ["file_name", "file_size_kb", "last_modified", "file_type", "file_path"]
        with open(output_file, 'w', newline='') as file:
            writer = csv.DictWriter(file, fieldnames=field_names)
            writer.writeheader()
            writer.writerows(self.file_list)
        print("Output file written")

if __name__ == "__main__":
    file_path = "C:\\aws\\AUTOMATION_VM_LINUX_11"
    dft = DirectoyFileTracker(file_path)
    dft.scan(file_path)
    dft.write_csv_report()
```

---

## Key Python Concepts

### 1. `os.walk()`

- Recursively yields `(dirpath, dirnames, filenames)`.
- Used to traverse a directory tree.

### 2. `os.stat()`

- Returns file metadata like size and last modified time.
- Example: `os.stat(file).st_size` gives size in bytes.

### 3. `os.path` Utilities

- `os.path.basename(path)` → just the file name.
- `os.path.splitext(path)` → separates name and extension.

### 4. CSV Writing

- `csv.DictWriter` allows writing a list of dictionaries to CSV.
- `writer.writeheader()` adds column headers.
- `writer.writerows(data)` writes all rows.

---

## Sample Output (CSV)

| file_name | file_size_kb | last_modified | file_type | file_path |
|-----------|---------------|----------------|-----------|------------|
| report.csv | 15.2 | 2024-12-01 10:22:30 | .csv | C:\aws\... |

---

## Summary

This script is a powerful tool for generating detailed reports on any file system directory using Python. It can be extended to filter file types, track changes, or even monitor in real time.
