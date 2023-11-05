# Nginx Log Monitor 

### Count Total Requests by HTTP Status Code:
> To count the total number of requests for each HTTP status code, you can use the following command:

```
awk '{status[$9]++} END {for (code in status) print code, status[code]}' access.log

```
--- 
### Find the Most Requested URLs:
```
awk '{url[$7]++} END {for (u in url) print u, url[u]}' access.log | sort -k2 -nr | head
```
> This command counts the occurrence of each URL and sorts them in descending order to show the top URLs.

### Calculate Total Bandwidth Consumed:
```
awk '{sum += $10} END {print sum}' access.log

```
---
### Identify the Most Active IP Addresses:
```
awk '{ip[$1]++} END {for (address in ip) print address, ip[address]}' access.log | sort -k2 -nr | head

```
---
### Analyze Access by Date and Time:
```
awk '{print $4, $1}' access.log | cut -c2-12 | sort | uniq -c

```
---
### Identify Frequent User Agents:
```
awk -F\" '{print $6}' access.log | sort | uniq -c | sort -rn | head

```
### Find Requests with Specific Criteria:
```
awk '$9 == "404"' access.log

```
---

###  parse Nginx access logs for getting **IP addresses list**
```
$ sudo cat /var/log/nginx/access.log | awk '{ print $1}' | sort | uniq -c | sort
---
```
### parse Nginx access logs for getting ***accessed file list***
```
$ sudo cat /var/log/nginx/access.log | awk '{ print $7}' | sort | uniq -c | sort
```
---
### parse Nginx access logs for counting requests per second
```
$ sudo cat /var/log/nginx/access.log | awk '{print $4}' | uniq -c | sort -rn | head
```
---
###  parse Nginx access logs for getting response codes
```
$ sudo cat /var/log/nginx/access.log | cut -d '"' -f3 | cut -d ' ' -f2 | sort | uniq -c | sort -rn

```
---
### parse Nginx access logs using online analyzer tools
```
$ sudo apt install goaccess
$ goaccess /var/log/nginx/access.log

```
### find out which links are broken now 
```
awk '($9 ~ /404/)' access.log | awk '{print $7}' | sort | uniq -c | sort -rn
```
---
### find Bad Gateway
```
awk '($9 ~ /502/)' access.log | awk '{print $7}' | sort | uniq -c | sort -r
```
---
### Who are requesting broken links (or URLs resulting in 502)
```
awk -F\" '($2 ~ "/wp-admin/install.php"){print $1}' access.log | awk '{print $1}' | sort | uniq -c | sort -r

```
---
### 404 for php files
```
awk '($9 ~ /404/)' access.log | awk -F\" '($2 ~ "^GET .*\.php")' | awk '{print $7}' | sort | uniq -c | sort -r | head -n 20

```
