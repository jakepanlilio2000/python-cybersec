# Python Cybersecurity Projects Roadmap

This repository contains a curated list of Python projects designed to build and advance your skills in cybersecurity. The projects are structured as a roadmap, starting with fundamental concepts and progressing to more complex, real-world applications. Each project is a standalone tool that teaches core principles of network security, host-based security, and offensive security techniques.

## Table of Contents

1.  [**Beginner Projects: Mastering the Basics**](https://www.google.com/search?q=%23beginner-projects-mastering-the-basics)
      * [Project 1: Simple Port Scanner](https://www.google.com/search?q=%23project-1-simple-port-scanner)
      * [Project 2: File Integrity Checker](https://www.google.com/search?q=%23project-2-file-integrity-checker)
2.  [**Advanced Projects: Complexity & Design**](https://www.google.com/search?q=%23advanced-projects-complexity--design)
      * [Project 3: Network Packet Sniffer & Analyzer](https://www.google.com/search?q=%23project-3-network-packet-sniffer--analyzer)
      * [Project 4: Keylogger with Email Reporting](https://www.google.com/search?q=%23project-4-keylogger-with-email-reporting)
      * [Project 5: Web Application Vulnerability Scanner](https://www.google.com/search?q=%23project-5-web-application-vulnerability-scanner)
3.  [**How to Use This Roadmap**](https://www.google.com/search?q=%23how-to-use-this-roadmap)

-----

## Beginner Projects: Mastering the Basics

These first two projects are designed to build your foundational skills in Python for cybersecurity. They focus on core concepts like networking, file manipulation, and cryptographic hashing.

### Project 1: Simple Port Scanner

A port scanner is a fundamental tool for network reconnaissance. This project will teach you the basics of socket programming, how devices communicate over a network, and how to handle user input.

  * **Objective:** Create a command-line tool that scans a target IP address for open ports.
  * **Key Learning Concepts:**
      * Socket programming (`socket` library)
      * TCP/IP connections (SYN scans)
      * Handling user input (`input`, `argparse`)
      * Basic multi-threading for performance
  * **Features Roadmap:**
    1.  ✅ Connect to a single port and report its status (open/closed).
    2.  ✅ Accept a target IP address from the user.
    3.  ✅ Loop through a range of ports (1-1024) to scan multiple ports.
    4.  ✅ Implement threading to scan ports concurrently and improve speed.
    5.  ✅ Refine the output to display a clean summary of only the open ports found.

### Project 2: File Integrity Checker

A cornerstone of host-based security. This tool creates a cryptographic "fingerprint" (hash) of important files and later checks if they have been modified, which could indicate a system compromise.

  * **Objective:** Build a script that can create a baseline of file hashes and later verify the integrity of those files against the baseline.
  * **Key Learning Concepts:**
      * File I/O (reading files in binary)
      * Cryptographic hashing (`hashlib` library)
      * Walking directory trees (`os.walk`)
      * Data serialization (saving/loading baselines with `json`)
      * Comparing dictionaries to find changes
  * **Features Roadmap:**
    1.  ✅ Generate an SHA-256 hash for a single file.
    2.  ✅ Create a "baseline" by recursively scanning a directory and saving all file paths and their corresponding hashes to a baseline file.
    3.  ✅ Implement a "check" mode that compares the current file hashes against the baseline.
    4.  ✅ Report on findings, clearly identifying `MODIFIED`, `NEW`, and `DELETED` files.
    5.  ✅ Build a command-line interface (`argparse`) to select modes (`--generate`, `--check`).

-----

## Advanced Projects: Complexity & Design

These projects require more sophisticated logic and introduce professional software design patterns for better code structure, efficiency, and maintainability.

### Project 3: Network Packet Sniffer & Analyzer

Go beyond port scanning to capture and dissect the actual data flowing over your network. This project provides deep insight into network protocols and how they are structured.

  * **Objective:** Build a tool that captures network packets and decodes their layers (Ethernet, IP, TCP/UDP).
  * **Design Pattern:** **Strategy Pattern**. This allows you to define a family of algorithms (protocol parsers), encapsulate each one, and make them interchangeable. Your main sniffer won't need to know the details of how to parse TCP vs. UDP; it will just use the appropriate "strategy" object.
  * **Key Learning Concepts:**
      * Raw sockets for network sniffing
      * Packet structure (Ethernet frames, IP headers, TCP/UDP segments)
      * Unpacking binary data (`struct` module)
      * Object-oriented design and polymorphism
  * **Features Roadmap:**
    1.  ✅ Capture raw packets from a network interface (using a library like `scapy` or raw sockets).
    2.  ✅ Implement a parser for the Ethernet (Layer 2) frame to extract source/destination MAC addresses.
    3.  ✅ Implement a parser for the IP (Layer 3) header to extract source/destination IP addresses and the protocol of the payload.
    4.  ✅ Implement parsers for TCP and UDP (Layer 4) segments to extract source/destination ports, flags, and sequence numbers.
    5.  ✅ Display the decoded information for each packet in a clear, structured format similar to Wireshark.

### Project 4: Keylogger with Email Reporting

> **⚠️ Ethical Warning:** This project is for educational purposes only. Never deploy a keylogger on a system without explicit, authorized permission. Unauthorized monitoring is illegal and unethical.

  * **Objective:** Create a stealthy keylogger that captures keystrokes and periodically emails the log file.
  * **Design Pattern:** **Singleton Pattern**. This ensures that only one instance of your keylogger can ever exist, preventing multiple instances from conflicting or consuming unnecessary resources.
  * **Key Learning Concepts:**
      * Intercepting system-wide events (`pynput` library)
      * Running background tasks (`threading.Timer`)
      * Sending emails programmatically (`smtplib`)
      * Discreet file operations
  * **Features Roadmap:**
    1.  ✅ Capture keyboard events and print them to the console.
    2.  ✅ Log keystrokes to a local file, handling special keys (e.g., `[SHIFT]`, `[ENTER]`).
    3.  ✅ Implement a function to send the contents of the log file via email.
    4.  ✅ Combine the components so the keylogger runs silently and emails its log file at a regular, timed interval.

### Project 5: Web Application Vulnerability Scanner

> **⚠️ Ethical Warning:** Only run this scanner against web applications you own or have explicit, written permission to test. Unauthorized vulnerability scanning is illegal.

  * **Objective:** Build a tool that crawls a web application and tests for common vulnerabilities like SQL Injection (SQLi) and Cross-Site Scripting (XSS).
  * **Design Pattern:** **Factory Method Pattern**. This pattern provides an interface for creating "scanner" objects but lets the main engine decide which type of scanner to instantiate (e.g., `SqlInjectionScanner`, `XssScanner`). This makes the tool modular and easily extensible.
  * **Key Learning Concepts:**
      * Making HTTP requests (`requests` library)
      * Parsing HTML (`BeautifulSoup`)
      * Web crawling and link discovery
      * Understanding basic vulnerability payloads (XSS, SQLi)
      * Automating form submissions
  * **Features Roadmap:**
    1.  ✅ Implement a crawler that takes a starting URL and discovers all links and forms within the same domain.
    2.  ✅ Implement an XSS scanner module that injects payloads into all discovered inputs and checks for reflections in the response.
    3.  ✅ Implement an SQLi scanner module that submits payloads and analyzes the response for signs of an injection (e.g., database errors).
    4.  ✅ Use the Factory pattern to allow the user to specify which scans to run (`--scan xss,sql`).
    5.  ✅ Generate a clean final report summarizing all potential vulnerabilities found, including the URL, vulnerable parameter, and payload used.

-----

## How to Use This Roadmap

1.  **Start with Project 1** and work your way down.
2.  For each step in a project's roadmap, focus on achieving the described functionality.
3.  Research the necessary Python libraries and functions needed to produce the desired outcome.
4.  Once a project is complete, move on to the next to build upon the skills you've learned.