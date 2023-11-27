# Gobuster

It is a tool to find routes and paths of a website.

```bash
# Use a word list to find subpath of website
gobuster -w ./wordlists/big.txt -u <URL>

# You can customize the HTTP response codes
# Now for example only 200 is valid
gobuster -w ./wordlists/big.txt -s "200" -u <URL>

# To specify the number of threads
gobuster -w ./wordlists/big.txt -t 30 -u <URL>

# To search directories
gobuster dir -w ./wordlists/big.txt -u <URL>
# Search the php file 
gobuster dir -x php -w ./wordlist/big.txt -u <URL>

# Find subdomains
gobuster vhost -u http://example.com -w subdomains.txt
```


