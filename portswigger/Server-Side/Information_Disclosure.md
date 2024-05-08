# Information Disclosure


-Failure to remove internal content from public content. For example, developer comments 
in markup are sometimes visible to users in the production environment.

-Insecure configuration of the website and related technologies. For example, failing to disable debugging and diagnostic features 
can sometimes provide attackers with useful tools to help them obtain sensitive information. Default configurations can also leave websites vulnerable, 
for example, by displaying overly verbose error messages.


-Flawed design and behavior of the application. For example, if a website returns distinct responses when different error states occur, this can also allow attackers to enumerate 
sensitive data, such as valid user credentials.

# Where to watch: 
```
/robots.txt , /sitemap.xml , /.git
```

If a website returns distinct responses when different error states occur, this can also allow attackers to enumerate sensitive data

-Try to use **TRACE** to get useful responses. This behavior is often harmless, but occasionally leads 
to information disclosure, such as the name of internal authentication headers that may be appended to requests by reverse proxies. 

-Search for custom http headers, and to login with that header. 
example: X-Custom-IP-Authorization: 82.61.96.18

# Source code disclosure via backup files.
Go to `/backup`.
Temporary files are usually indicated in some way, such as by appending a tilde (~) to the filename or adding 
a different file extension. Requesting a code file using a backup file extension can sometimes allow you to read the contents of the file in the response. 
