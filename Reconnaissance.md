# Web Hacking Reconnaissance Guide

This guide outlines various techniques and tools used for web hacking reconnaissance to identify vulnerabilities and gather critical information.

## 1. Manually Walking Through the Target
Explore the target website manually to understand the layout and features, noting potential areas of interest or vulnerability.

## 2. Google Dorking
Utilize advanced Google search techniques to uncover hidden information about the target that might be indexed.

## 3. Scope Discovery
### WHOIS and Reverse WHOIS
- Use tools like ViewDNS.info to find personal information of the website owner.

### nslookup
- Find the IP address of a domain.
- 
### IP-to-ASN
- Identify if a company owns a dedicated IP range.

### Certificate Parsing
- Use online databases like crt.sh, Censys, and Cert Spotter to find certificates for a domain.

### Subdomain Enumeration
- After domain enumeration, use tools like Sublist3r, SubBrute, Amass, and Gobuster to find as many subdomains as possible.
- Use Commonspeak2 or resources like [SecLists](https://github.com/danielmiessler/SecLists/) to generate a wordlist.
- Combine and deduplicate wordlists:

- Use Altdns for automated subdomain enumeration when some subdomains are already known.

### Service Enumeration
- Use nmap for active scanning or tools like Shodan, Censys, and Project Sonar for passive scanning.

### Directory Brute-Forcing
- Employ Dirsearch or Gobuster for brute-forcing directories.
- Use tools like EyeWitness or Snapper to verify hosted pages.

### Spidering the Site
- Utilize web crawlers built into tools like ZAP and Burp Suite to discover directories.

### Third-Party Hosting
- Search for hidden endpoints, logs, or credentials in Amazon S3 buckets using:

- Tools like GrayhatWarfare for keyword searches, and Lazys3/Bucket Stream for brute-forcing bucket names.

### Accessing S3 Buckets
- Install awscli and access buckets:
- If access is granted, check for interesting files:

### GitHub Recon
- Use tools like Gitrob and TruffleHog to automate the search for keys, secrets, or passwords on GitHub.
- Validate credentials using KeyHacks.

### Other Techniques
- Search for employee information on LinkedIn.
- Check Pastebin and use PasteHunter for leaks.
- Use Wayback Machine and tools like [waybackurls](https://github.com/tomnomnom/waybackurls/) to extract old endpoints.


