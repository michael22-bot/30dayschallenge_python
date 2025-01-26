Creating a Python script for scanning local networks for open ports, web services, and basic vulnerability scanning using Nmap is a great way to practice and understand networking and cybersecurity principles. Below is a basic step-by-step guide and Python script that uses Nmap for network scanning and vulnerability assessment.

### Prerequisites:
1. **Install Nmap**: Ensure Nmap is installed on your machine. You can install it by running the following command:
   ```bash
   sudo apt install nmap  # For Ubuntu/Debian-based systems
   ```
   For Windows, you can download it from the [official Nmap website](https://nmap.org/).

2. **Install Python Nmap library**: The `python-nmap` library provides a Python interface to Nmap. Install it via pip:
   ```bash
   pip install python-nmap
   ```

### Script Outline:
1. **Scan Local Network for Open Ports**: We'll use Nmap to discover open ports on the target IP range.
2. **Identify Web Services**: We'll scan for services like HTTP (port 80), HTTPS (port 443), etc.
3. **Perform Basic Vulnerability Scanning**: We can use Nmap's vulnerability scanning options to identify known vulnerabilities.

### Python Script:

```python
import nmap
import sys

# Initialize Nmap scanner
nm = nmap.PortScanner()

def scan_ports(target_ip):
    print(f"Scanning {target_ip} for open ports...\n")
    
    try:
        # Perform a simple port scan (common ports)
        nm.scan(hosts=target_ip, arguments='-p 1-1024')  # Scanning ports 1-1024
        print(f"Host: {target_ip}")
        print(f"Scan Info: {nm[target_ip].state()}")

        for proto in nm[target_ip].all_protocols():
            print(f"Protocol: {proto}")

            # Get the open ports
            open_ports = nm[target_ip][proto].keys()
            for port in open_ports:
                print(f"Port: {port}, State: {nm[target_ip][proto][port]['state']}, Service: {nm[target_ip][proto][port]['name']}")

    except Exception as e:
        print(f"Error scanning {target_ip}: {e}")

def scan_web_services(target_ip):
    print(f"Scanning {target_ip} for common web services (HTTP, HTTPS)...\n")
    
    try:
        # Scan for HTTP/HTTPS services
        nm.scan(hosts=target_ip, arguments='-p 80,443')
        if '80' in nm[target_ip]['tcp']:
            print(f"HTTP Service found on {target_ip}: Port 80")
        if '443' in nm[target_ip]['tcp']:
            print(f"HTTPS Service found on {target_ip}: Port 443")
    except Exception as e:
        print(f"Error scanning for web services on {target_ip}: {e}")

def scan_vulnerabilities(target_ip):
    print(f"Scanning {target_ip} for known vulnerabilities...\n")
    
    try:
        # Use Nmap's vulnerability scanning script
        nm.scan(hosts=target_ip, arguments='--script vuln')
        if nm.all_hosts():
            for host in nm.all_hosts():
                print(f"Host: {host}")
                if 'vuln' in nm[host]:
                    print(f"Vulnerabilities found: {nm[host]['vuln']}")
                else:
                    print("No vulnerabilities found.")
    except Exception as e:
        print(f"Error scanning for vulnerabilities on {target_ip}: {e}")

def main():
    if len(sys.argv) < 2:
        print("Usage: python3 scan_network.py <target_ip>")
        sys.exit(1)

    target_ip = sys.argv[1]

    scan_ports(target_ip)
    scan_web_services(target_ip)
    scan_vulnerabilities(target_ip)

if __name__ == "__main__":
    main()
```

### Explanation:
1. **scan_ports(target_ip)**: This function scans the target IP for open ports between 1 and 1024 using Nmap. The ports and services are printed.
   
2. **scan_web_services(target_ip)**: This function scans for common web services such as HTTP (port 80) and HTTPS (port 443) on the target IP.

3. **scan_vulnerabilities(target_ip)**: This function uses Nmap's `--script vuln` option to scan the target for known vulnerabilities. Nmap's vulnerability scanning scripts can identify issues like outdated software versions, open ports with potential exploits, etc.

4. **main()**: This function takes the target IP as a command-line argument and runs all three scans on that IP.

### Running the Script:
1. Save the script to a file, e.g., `scan_network.py`.
2. Open a terminal and run the script:
   ```bash
   python3 scan_network.py <target_ip>
   ```
   Replace `<target_ip>` with the IP address of the local machine or network you want to scan.

### Example Output:
```
Scanning 192.168.1.1 for open ports...

Host: 192.168.1.1
Scan Info: up

Protocol: tcp
Port: 22, State: open, Service: ssh
Port: 80, State: open, Service: http

Scanning 192.168.1.1 for common web services (HTTP, HTTPS)...

HTTP Service found on 192.168.1.1: Port 80

Scanning 192.168.1.1 for known vulnerabilities...

Host: 192.168.1.1
No vulnerabilities found.
```

### Notes:
- **Nmap Arguments**: The `arguments` field in the `scan()` function specifies the Nmap options. You can customize this for more advanced scans (e.g., scanning more ports or using different Nmap scripts).
- **Permissions**: Scanning networks and performing vulnerability assessments might require root or admin privileges in some environments.
- **Ethical Considerations**: Always ensure that you have permission to scan the network and devices you are testing. Unauthorized scanning may be illegal or against the terms of service.

This script can be extended with more sophisticated vulnerability checks, logging, and error handling as you continue learning.
