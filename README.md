# Python Cybersecurity Project Roadmap

This roadmap provides a structured path for you to learn and apply Python in the field of cybersecurity. It's designed to progressively build your skills, starting with foundational concepts and moving towards more complex topics. Each project includes detailed steps, key concepts, and recommended libraries to guide your progress.

---

## Project 1: Network Scanner (Beginner - Mastering the Basics)

**Goal:** To build a fundamental network scanner that identifies open ports and basic service information on target hosts. This project will solidify your understanding of Python's core features, network communication, and error handling.

**Key Concepts to Master:**

* **Python Basics:** Variables, data types, loops, conditionals, functions, classes (optional but good practice).
* **Networking Fundamentals:** TCP/IP, ports, sockets, client-server model.
* **Error Handling:** `try-except` blocks for robust code.
* **Concurrency:** Basic understanding of threading for performance.
* **Command-Line Interface (CLI):** Parsing arguments for user input.

---

### Detailed Steps:

#### Phase 1: Basic Port Scanner (Single Target, Single Port)

* **Setup:**
    * Create a new Python file (e.g., `port_scanner.py`).
    * Import the `socket` module: `import socket`.
* **Core Logic:**
    * Define a target IP address (e.g., `target_ip = "127.0.0.1"` or a known test IP).
    * Define a specific port (e.g., `port = 80`).
    * Create a socket object: `s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)`.
        * `socket.AF_INET`: Specifies IPv4 address family.
        * `socket.SOCK_STREAM`: Specifies TCP protocol.
    * Set a timeout for the connection: `s.settimeout(1)`. This prevents the scanner from hanging indefinitely on closed ports.
    * Attempt to connect: `result = s.connect_ex((target_ip, port))`.
        * `connect_ex()` returns `0` if successful, otherwise an error code.
    * Check the result:
        * If `result == 0`: Print "Port X is open".
        * Else: Print "Port X is closed".
    * Close the socket: `s.close()`.

#### Phase 2: Multi-Port Scanner (Range of Ports)

* **User Input for Ports:**
    * Modify your script to accept a range of ports (e.g., `start_port`, `end_port`).
    * Use a `for` loop to iterate through the port range.
    * Inside the loop, reuse the connection logic from Phase 1 for each port.
* **Function Encapsulation:**
    * Create a function `scan_port(ip, port)` that encapsulates the socket creation, connection, and result printing for a single port.
    * Call this function within your port range loop.

#### Phase 3: Multi-Host Scanner (Range of IPs - Optional but Recommended)

* **IP Range Input:**
    * Extend your script to accept a range of IP addresses (e.g., `192.168.1.1-192.168.1.254`).
    * You'll need logic to parse this range and generate individual IP addresses.
    * Nest your port scanning loop inside an IP address loop.

#### Phase 4: Service Banner Grabbing

* **Receive Data:**
    * When a port is found open, attempt to receive data from the service.
    * After `connect_ex()` returns `0`, use `s.recv(1024)` to receive up to 1024 bytes of data.
    * Decode the received data (e.g., `data.decode('utf-8', errors='ignore')`).
    * Print the received banner. This often reveals the service type and version (e.g., "Apache/2.4.41 (Ubuntu)").
    * Add `try-except` around `s.recv()` as some services might not send a banner immediately or might close the connection.

#### Phase 5: Adding Threading for Speed

* Import threading: `import threading`.
* **Thread Function:**
    * Modify your `scan_port` function to be the target for a thread.
* **Thread Creation:**
    * Instead of directly calling `scan_port` in your loop, create a new `threading.Thread` object for each port: `t = threading.Thread(target=scan_port, args=(ip, port))`.
    * Start the thread: `t.start()`.
* **Important:** You'll need a way to manage the number of active threads to avoid overwhelming your system or the target. Consider using a `threading.Semaphore` or a thread pool. A simpler approach for beginners is to just let them run and observe the speed increase.

#### Phase 6: Command-Line Interface (CLI)

* Import argparse: `import argparse`.
* **Parser Setup:**
    * Create an `ArgumentParser` object: `parser = argparse.ArgumentParser(description="A simple network port scanner.")`.
    * Add arguments:
        ```python
        parser.add_argument("-t", "--target", help="Target IP address or hostname", required=True)
        parser.add_argument("-p", "--ports", help="Port(s) to scan (e.g., 80, 1-1024)", required=True)
        ```
    * Parse arguments: `args = parser.parse_args()`.
* **Argument Processing:**
    * Parse `args.ports` to handle single ports or port ranges.
    * Use `socket.gethostbyname(args.target)` to resolve hostnames to IP addresses.

#### Phase 7: Output to File (Optional)

* **File Writing:**
    * Add an option (e.g., `-o`, `--output`) to save results to a text file.
    * Open the file in write mode (`'w'`) or append mode (`'a'`) and write the scan results.

---

### Recommended Libraries:

* **`socket`**: For low-level network communication.
* **`threading`**: For concurrent scanning.
* **`argparse`**: For building a user-friendly command-line interface.

---

## Project 2: Simple Web Vulnerability Scanner (Advanced)

**Goal:** To develop a basic tool that can identify common web vulnerabilities like Reflected Cross-Site Scripting (XSS) and simple SQL Injection (SQLi) in web applications. This project will deepen your understanding of HTTP, web parsing, and common attack vectors.

**Key Concepts to Master:**

* **HTTP Protocol:** Requests (GET, POST), headers, cookies, status codes.
* **Web Scraping/Parsing:** Extracting information from HTML.
* **Regular Expressions:** Pattern matching for vulnerability detection.
* **Common Web Vulnerabilities:** XSS (Reflected), SQL Injection (Error-based).
* **Session Management:** Handling cookies and maintaining sessions.

---

### Detailed Steps:

#### Phase 1: HTTP Request/Response Handler

* **Setup:**
    * Create a new Python file (e.g., `web_scanner.py`).
    * Install and import the `requests` library: `pip install requests`, then `import requests`.
* **Basic GET Request:**
    * Define a target URL (e.g., a simple test site or a local vulnerable application like DVWA or OWASP Juice Shop).
    * Perform a GET request: `response = requests.get(url)`.
    * Print `response.status_code` and `response.text` (the HTML content).
* **Basic POST Request:**
    * Simulate a form submission. Identify a form on a test site.
    * Determine the form's action URL and input field names.
    * Create a dictionary for the form data: `data = {'username': 'test', 'password': 'password'}`.
    * Perform a POST request: `response = requests.post(url, data=data)`.
* **Handling Headers and Cookies:**
    * Add custom headers to requests: `headers = {'User-Agent': 'MyScanner/1.0'}`, then `requests.get(url, headers=headers)`.
    * Access cookies: `response.cookies`.
    * Maintain a session: `session = requests.Session()`. Use `session.get()` and `session.post()` to automatically handle cookies across requests.

#### Phase 2: Basic XSS Scanner (Reflected XSS)

* **Identify Input Points:**
    * Manually inspect a target web page for input fields (search bars, comment sections, URL parameters).
    * For a simple scanner, focus on URL parameters (e.g., `http://example.com/search?query=`).
* **Craft Payloads:**
    * Create a list of simple XSS payloads (e.g., `<script>alert(1)</script>`, `"><script>alert('XSS')</script>`).
* **Injection Logic:**
    * For each URL parameter or input field, inject a payload.
    * If scanning URL parameters: `test_url = f"{base_url}?param={payload}"`.
    * Perform a GET request to `test_url`.
* **Detection:**
    * Check if the payload (or a part of it, like `alert(1)`) is reflected directly in the `response.text` of the page without proper encoding.
    * Use `if payload in response.text:` for a basic check.
    * **Improvement:** Use regular expressions (`import re`) for more robust pattern matching, looking for the payload within specific HTML contexts.

#### Phase 3: Basic SQL Injection Scanner (Error-Based)

* **Identify Input Points:**
    * Focus on URL parameters or form fields that likely interact with a database (e.g., `id=`, `category=`).
* **Craft Payloads:**
    * Create a list of simple error-based SQLi payloads (e.g., `'`, `''`, `" " OR 1=1--`, `admin'--`, `1' UNION SELECT NULL,NULL,NULL--`).
* **Injection Logic:**
    * Similar to XSS, inject payloads into URL parameters or form fields.
    * Perform GET/POST requests with the injected payloads.
* **Detection:**
    * Look for common SQL error messages in `response.text` (e.g., "SQL syntax error", "You have an error in your SQL syntax", "mysql_fetch_array()").
    * Use regular expressions to search for these error patterns.

#### Phase 4: Form Submission and Session Handling

* **Login Functionality:**
    * Implement a function to log into a target application (if required) using `requests.Session()`.
    * This will allow your scanner to access authenticated parts of the application.
* **Automated Form Discovery (Optional, More Advanced):**
    * Install and import BeautifulSoup4: `pip install beautifulsoup4`, then `from bs4 import BeautifulSoup`.
    * Parse the HTML of a page: `soup = BeautifulSoup(response.text, 'html.parser')`.
    * Find all `<form>` tags.
    * For each form, identify `<input>` fields and their `name` attributes to dynamically build data payloads.

#### Phase 5: Reporting

* **Structured Output:**
    * Print clear messages indicating found vulnerabilities (e.g., "XSS found at URL: [URL] with payload: [Payload]").
    * Consider saving results to a file (JSON or text).

---

### Recommended Libraries:

* **`requests`**: For making HTTP requests.
* **`BeautifulSoup4`** (or `lxml`): For parsing HTML and extracting data (especially for form discovery).
* **`re`**: For regular expression matching to detect patterns and errors.

---

## Project 3: Malware Analysis Sandbox (Advanced)

**Goal:** To build a simplified, controlled environment (a "sandbox") in Python that can execute a program and monitor its interactions with the file system, processes, and simulated network activity. This project will introduce you to dynamic malware analysis concepts. **Note: This will be a simulated sandbox for learning purposes, not a full-fledged secure environment.**

**Key Concepts to Master:**

* **Process Management:** Spawning, monitoring, and terminating processes.
* **File System Interaction:** Tracking file creation, deletion, modification.
* **Network Simulation:** Intercepting or logging simulated network calls.
* **System Calls (Conceptual):** Understanding how programs interact with the operating system.
* **Virtual Environments:** Important for isolating your Python project dependencies.

---

### Detailed Steps:

#### Phase 1: Basic File System Monitor

* **Setup:**
    * Create a new Python file (e.g., `sandbox.py`).
    * Import `os`, `shutil`, `time`, `subprocess`.
* **Monitoring Directory:**
    * Define a specific directory to monitor (e.g., a temporary folder).
    * Before executing the target program, record the initial state of this directory (list of files, their sizes, modification times).
    ```python
    initial_files = {f: os.path.getmtime(os.path.join(monitor_dir, f)) for f in os.listdir(monitor_dir)}
    ```
* **Execute Program (Isolated):**
    * Use `subprocess.Popen()` to run the target program. This allows non-blocking execution.
    * `process = subprocess.Popen([target_program_path], cwd=monitor_dir)` (run in the monitored directory).
    * Wait for the program to complete or for a timeout: `process.wait(timeout=10)`.
    * Handle `subprocess.TimeoutExpired` if the program runs too long.
* **Post-Execution Analysis:**
    * After the program finishes, re-scan the monitored directory.
    * Compare the current state with the `initial_files`:
        * **New files:** Files present now but not in `initial_files`.
        * **Deleted files:** Files in `initial_files` but not present now.
        * **Modified files:** Files with changed modification times or sizes.
    * Print a report of file system changes.

#### Phase 2: Basic Process Monitor

* **Process Creation Tracking:**
    * When you execute the target program using `subprocess.Popen()`, you get a process object.
    * Record its PID: `process.pid`.
    * You can't easily track child processes spawned by the target program using just `subprocess` alone.
    * **Advanced (Optional - requires psutil):** Install `pip install psutil`.
    * Use `psutil.Process(process.pid).children()` to get child processes.
    * You'd need to poll `psutil.process_iter()` before and after execution to find new processes, but this is more complex and less reliable for a simple sandbox. Stick to just monitoring the main process for now.
* **Process Termination:**
    * After `process.wait()`, check `process.returncode` to see if it exited normally.
    * If it timed out, you might need to `process.terminate()` or `process.kill()`.
* **Print Process Info:**
    * Report the PID, return code, and execution duration of the main process.

#### Phase 3: Network Activity Logger (Simulated)

* **Concept:** A true network sandbox involves redirecting network traffic through a proxy. For a Python-only sandbox, you'll simulate this by looking for specific patterns or by requiring the "malware" to use your custom network functions.
* **Simulated DNS/HTTP Requests:**
    * **Method A (Simple String Search):** If the "malware" is a Python script, you can modify it to use your custom requests or socket wrappers that log activity. This is for controlled testing.
    * **Method B (Proxying - Conceptual):** For external executables, you'd need to set up a local proxy server (e.g., using Python's `http.server` or `socketserver` modules) and configure the target program's environment to use it. This is significantly more complex and outside the scope of a "basic" sandbox.
    * For this project, focus on Method A or simple logging:
        * Create a placeholder function `log_network_activity(destination, type)` that just prints or logs the activity.
        * If you're analyzing a Python script, make it call this function instead of `requests.get()` directly.
        * Alternatively: If you're running a simple script, you can try to capture its `stdout` and `stderr` and search for network-related strings.
            ```python
            stdout, stderr = process.communicate(timeout=10)
            ```
        * Search `stdout` for URLs, IP addresses, etc.

#### Phase 4: Report Generation

* **Consolidate Findings:**
    * Combine the file system changes, process information, and simulated network activity into a single, readable report.
    * Print to console or save to a text file.
* **Example:**

    ```
    --- Sandbox Analysis Report ---
    Target Program: my_malware.exe
    Execution Time: 5 seconds
    Return Code: 0

    [File System Changes]
    New Files:
      - C:\temp\new_file.txt (created at 2023-10-27 10:30:00)
    Modified Files:
      - C:\temp\existing_log.txt (modified at 2023-10-27 10:30:05)
    Deleted Files:
      - C:\temp\temp_data.tmp

    [Process Activity]
    Main Process PID: 12345
    Child Processes: (None tracked in this basic version)

    [Simulated Network Activity]
    - Connected to example.com (HTTP GET)
    - DNS lookup for malicious.domain
    -----------------------------
    ```

#### Phase 5: Simple "Sandbox" Environment

* **Temporary Directory:**
    * Use `tempfile.mkdtemp()` to create a unique temporary directory for each analysis run.
    * Copy the target program into this temporary directory before execution.
    * Ensure the temporary directory is deleted after analysis using `shutil.rmtree(temp_dir)`.
* **Environment Variables (Optional):**
    * You can pass a modified environment to the subprocess: `subprocess.Popen([target_program_path], env={'PATH': '/sanitized/path'})`. This can restrict what the program can execute.

---

### Recommended Libraries:

* **`os`**: For file system operations.
* **`subprocess`**: For executing external programs and managing processes.
* **`sys`**: For system-specific parameters (less critical here).
* **`shutil`**: For high-level file operations (copying, deleting directories).
* **`time`**: For delays and measuring execution time.
* **`tempfile`**: For creating temporary directories.
* **`psutil`** (Optional, for more advanced process monitoring): Provides cross-platform access to process and system utilization.

---

## General Tips for All Projects:

* **Version Control (Git):** Use Git from day one. Commit frequently, write meaningful commit messages. This is crucial for tracking your progress and collaborating.
* **Documentation:** Write comments in your code explaining complex logic. Create a `README.md` file for each project explaining its purpose, how to run it, and any dependencies.
* **Testing:** As you add features, test them thoroughly. For cybersecurity tools, this often means testing against known-good and known-bad targets (e.g., deliberately vulnerable web applications like DVWA, or safe malware samples in a VM).
* **Ethical Hacking Principles:** Always ensure you have explicit permission to scan or analyze any target system. Use these tools only in controlled, legal, and ethical environments (e.g., your own virtual machines, intentionally vulnerable test environments).
* **Continuous Learning:** Cybersecurity is a rapidly evolving field. Stay updated with new vulnerabilities, tools, and techniques. Read blogs, follow security researchers, and participate in CTFs (Capture The Flag) competitions.

This roadmap provides a solid foundation. As you complete each project, you'll gain practical experience and a deeper understanding of cybersecurity concepts, preparing you for more advanced challenges. Good luck!
