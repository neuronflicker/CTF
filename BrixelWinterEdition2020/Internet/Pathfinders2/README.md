# Pathfinders #2
**Category:** [Internet](../README.md)

**Points:** 20

**Description:**

It seems they updated their security. can you get the password for their admin section on their new site?

http://timesink.be/pathfinder2/

<sub><sup>oh yeah, let's assume they are running a php version below 5.3.4 here...</sup></sub>

## Write-up
The solution to this is based on the solution to Pathfinders #1, where the admin authentication details could be accessed due to a file inclusion by *index.php*:
```
curl http://timesink.be/pathfinder/index.php?page=admin/.htpasswd
```
However, in Pathfinders #2, the website has been updated to only allow PHP files to be included, so if we try the same for Pathfinders #2, we get:
```
> curl http://timesink.be/pathfinder2/index.php?page=admin/.htpasswd
file not ending in .php, terminating.
```

The hint in the CTF description is that we are running a *PHP version below 5.3.4*. A quick Google search for `php 5.3.4 vulnerabilities` and a little bit of looking through the results bought up [this page](https://ctf-wiki.org/web/php/php/).

After translating the page, it can be seen that we can try using `%00.php` in a URL to get past the `.php` ending check, and it will truncate the `%00.php` from the end of the URL when the file is included. We wrote a `get_flag.sh` script to test this:
```bash
#!/bin/sh
curl http://timesink.be/pathfinder2/index.php?page=admin/.htpasswd%00.php
```
Running this worked and gave us the flag

