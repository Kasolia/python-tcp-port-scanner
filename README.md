# python-tcp-port-scanner
A Python-based TCP port scanner that identifies open ports and performs basic service enumeration for authorized security testing.

port_scanner.py
README.md
requirements.txt (optional for now)

import socket
import sys
from datetime import datetime

def scan_port(target, port):
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        socket.setdefaulttimeout(1)
        result = sock.connect_ex((target, port))
        sock.close()
        return result == 0
    except:
        return False

def main():
    if len(sys.argv) != 4:
        print("Usage: python port_scanner.py <target> <start_port> <end_port>")
        sys.exit()

    target = sys.argv[1]
    start_port = int(sys.argv[2])
    end_port = int(sys.argv[3])

    try:
        target_ip = socket.gethostbyname(target)
    except socket.gaierror:
        print("Hostname could not be resolved.")
        sys.exit()

    print(f"\nScanning Target: {target_ip}")
    print(f"Scanning Ports: {start_port} - {end_port}")
    print("Start Time:", datetime.now())
    print("-" * 50)

    for port in range(start_port, end_port + 1):
        if scan_port(target_ip, port):
            print(f"Port {port}: OPEN")

    print("\nScan Complete.")

if __name__ == "__main__":
    main()
