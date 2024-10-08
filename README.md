# HTB-CheatSheet

Now Updating...

# Basics

## Nmap - Port Scanning

### [Nmap Cheat Sheet - TutorialsPoint](https://www.tutorialspoint.com/nmap-cheat-sheet)

### TCP and UDP Scan Examples

```bash
# Basic scan
sudo nmap -v -sS -sC -sV --top-ports 3500 <target_ip>

# Full scan
sudo nmap -v -sS -A -p- <target_ip>

# Use if no hosts respond to ping
sudo nmap -v -sS -Pn <target_ip>

# Use if SYN scan takes too long
nmap -v -sT -p- --min-rate 5000 --max-retries 1 <target_ip>

# UDP scan
nmap -v -sU -T4 --top-ports 100 <target_ip>
```

### NSE (Nmap Scripting Engine)

```bash
# Update system file database and search for NSE scripts
sudo updatedb
locate .nse | grep <script_name>

# Full vulnerability scan
# Warning: --script-args=unsafe=1 may cause issues on target systems
sudo nmap -v -sS -Pn --script=vuln --script-args=unsafe=1 <target_ip>
```

# Network Services Pentesting

## 80,443 - HTTP/HTTPS

### Directory Brute Forcing

```bash
# Fast scan using a smaller wordlist for quick results
gobuster dir -e -l -t 50 -u http://<target_host> -w /usr/share/wordlists/dirb/big.txt

# Full scan using a more comprehensive wordlist
gobuster dir -e -l -t 50 -u http://<target_host> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# Additional useful wordlists
/usr/share/seclists/Discovery/Web-Content/big.txt
/usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt

# Gobuster options
-e                          # Print full URLs in output
-l                          # Include the length of the body in the output
-t <number>                 # Number of concurrent threads (default 10)
-b <status>                 # Exclude status codes, comma separated list of codes and ranges
--exclude-length <lengths>  # Exclude content lengths, comma separated list of codes and ranges
-x <extensions>             # File extensions to search for, comma-separated
-o <filename>               # Output file to write results to

# Common File Extensions
-x php,asp,aspx,jsp,html,js,txt,old,bak,cgi,pl,py,sh,do

# Common Picture Extensions
-x png,jpg,jpeg,gif,bmp,svg,ico
```

### Subdomain Brute Forcing

```bash
# Fast scan using a smaller wordlist for quick results
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://<target_host>/ -H "Host: FUZZ.<target_host>" -H "User-Agent: PENTEST" -c -t 100 -ac -v

# Full scan using a more comprehensive wordlist
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt -u http://<target_host>/ -H "Host: FUZZ.<target_host>" -H "User-Agent: PENTEST" -c -t 100 -ac -v

# Scan using a custom request file
# - Add "FUZZ" keyword in the Host header of request.txt to inject subdomains
# - Useful for complex requests or when specific headers/body are required
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://trickster.htb -request request.txt -c -ac -v

# Additional useful wordlists
/usr/share/wordlists/seclists/Discovery/DNS/dns-Jhaddix.txt
/usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt
/usr/share/wordlists/dirbuster/dns-names.txt

# ffuf options
-H <header: value>   # Add custom headers
-t <number>          # Number of concurrent threads (default 40)
-ac                  # Automatically calibrate filtering options
-fc <status>         # Filter HTTP status codes from response, comma separated list of codes and ranges
-fs <size>           # Filter response size
-r                   # Follow redirects
-request <filename>  # Load a custom HTTP request from a file containing the raw http request
-o <filename>        # Write output to file
-of <format>         # Output file format (json, ejson, html, md, csv, ecsv)
```

