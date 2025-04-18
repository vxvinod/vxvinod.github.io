---
layout: post
title:  "Log Rotator"
date:   2025-04-18 
desc: "In this blog we will be learning about reading files and iterating line by line, seperate the logs as per the data and store it in seperate files."
---

# Log File Rotator with Date Split in Python

## Overview

Managing server logs efficiently is crucial for debugging, monitoring, and maintaining applications. One common task is to **split a large log file by date**, so that logs for each day are stored separately. This helps with performance, organization, and makes log analysis easier.

In this blog, we'll walk through a simple Python script that reads a `server.log` file, extracts the date from each log entry, and writes each entry into a separate file named after the date.

---

## Sample Log Format

The input log file (`server.log`) is expected to look like this:

```
[INFO] 2025-04-16 09:15:23 - User login
[ERROR] 2025-04-17 10:12:00 - Database down
[WARNING] 2025-04-16 11:00:00 - CPU usage high
```

We want to split this into files like:

- `server_2025-04-16.log`
- `server_2025-04-17.log`

---

## Python Code: LogRotator Class

Here’s the full Python script:

```python
import os
from collections import defaultdict

class LogRotator:

    def __init__(self, file):
        self.file = file
        self.counts = defaultdict(int)

    def rotate_logs(self):
        os.makedirs("rotated_logs", exist_ok=True)
        with open(self.file, 'r') as file:
            for line in file:
                if not line.strip():
                    continue
                parts = line.split()
                if len(parts) < 2:
                    continue
                date = parts[1]
                file_name = f"rotated_logs/server_{date}.log"
                with open(file_name, 'a') as f:
                    f.write(line)
                self.counts[date] += 1

        print("\nLog rotation completed.\nSummary:")
        for date, count in self.counts.items():
            print(f"{date}: {count} lines")

if __name__ == '__main__':
    lr = LogRotator('server.log')
    lr.rotate_logs()
```

---

## Key Features

- **Handles blank/malformed lines** safely.
- **Automatically creates output directory** (`rotated_logs/`).
- **Prints a summary** of how many lines were written per date.

---

## Enhancements You Can Try

- Add CLI arguments using `argparse`
- Compress rotated logs
- Filter by log levels (e.g., only `[ERROR]` logs)
- Support custom log formats

---

## Conclusion

This log rotation script is a small but powerful utility that you can adapt to your real-world projects. Whether you're running microservices or backend systems, clean log organization is a step toward better observability.

Happy coding!