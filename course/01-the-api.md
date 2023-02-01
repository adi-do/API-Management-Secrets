Synthesized intro about APIs
============================ 

API = Application Programming Interface offered by a software application to perform communications with another software. 

Examples: 
- [GitHub - public-apis/public-apis: A collective list of free APIs](https://github.com/public-apis/public-apis)
- [Free Public APIs for Developers](https://rapidapi.com/collection/list-of-free-apis)
- [Big List of Free and Open Public APIs (No Auth Needed)](https://mixedanalytics.com/blog/list-actually-free-open-no-auth-needed-apis/)


TWO MAIN ACTORS 
---------------
In a software-to-software communication there are always two actors involved:
- [a client](#the-client) 
- [and a server](#the-server)


### THE CLIENT 
Is the software that initializes the communication. Examples sorted by type: 
- Command line: curl, wget 
- Visual (with GUI): Postman, Insomnia, SOAP UI 
- Other software build with backend or frontend specific languages: Python, PHP, Perl, Java, C, Javascript, Go, etc. 

The term `client` refers to the software that makes the invocation (sends the request). Other accepted names are `application` and `consumer`, although the latter also refers to the human people involved in the actual development and run of the client app. They are also referred as `developers`. 
 

### THE SERVER
Usually is a piece of (backend) software, built with specific purposes that acts upon a `REQUEST` which receives from the client and returns an answer, also called `RESPONSE`.

Even though the server is a software which is build and maintained by a group of developers, we are never name them as "developers" since this label is used to point to the client app developers. Server app developers and maintainers are called `providers`.

Client and server can: 
- co-exist on the same machine: often used in lower environments (dev, testing, etc) during the built of a micro-services shaped application  
- exist on different machines, across different network zones: most common usage is in the communication between two different softwares built and maintained by different teams/projects/organizations. 

 
THE COMMUNICATION
-----------------
The client initializes the communication with the server, therefore it sends a REQUEST to the server, building the connection between the two points. This connections remains open and will be used to transport the RESPONSE back 

 

### THE REQUEST 
Is a block of text (string) composed by multiple parts, each with a different purpose. I will present and example found in the previous [free & public APIs lists](#synthesized-intro-about-apis)

This is one basic and not quite complete example of a request: 
    
    POST /submit HTTP/1.1
    Host: v2.jokeapi.dev
    Content-Type: application/json
    User-Agent: MyApp/1.0
    Cache-Control: no-cache
    Content-Length: 399

    {
        "formatVersion": 3,
        "category": "Programming",
        "type": "twopart",
        "setup": "How many programmers does it take to change a light bulb?",
        "delivery": "None – It’s a hardware problem",
        "flags": {
            "nsfw": false,
            "religious": false,
            "political": false,
            "racist": false,
            "sexist": false,
            "explicit": false
        },
        "lang": "en"
    }

> Above request belong to the JokeAPI found in the above free API lists. Since it has a purely educational purpose, [I recommend it's documentation](https://v2.jokeapi.dev/) for more details about the structure and usage of an API request.

This request is a POST request, which is being sent to the "https://v2.jokeapi.dev/submit" endpoint. It includes several headers:

- Host: The domain where the request is sent.
- Content-Type: The format of the data in the request body.
- User-Agent: The client software and version that is making the request.
- Cache-Control: Specifies that the response should not be cached.
- Content-Length: The length of the request body.
- The request body contains a JSON object that includes the the joke string in two parts, and other related information like flags, language, type and category.

Other important fields that are used in normal usage, but not required during the above example:
- Authorization: An access token or a basic authentication that authenticates the client with the server.

#### URL
One basic (but complete) example: `https://v2.jokeapi.dev/joke/Programming?blacklistFlags=racist`

Components:
- schema (or protocol) : `https`
- host : `v2.jokeapi.dev`
    - subdomain: `v2`
    - domain with extension: `jokeapi.dev`
    - port (implicit in this case): `443`
- basepath: ``
- resource (or path): `/joke/Programming`
- query search: `?blacklistFlags=racist`
    - query parameter name: `blacklistFlags`
    - query parameter value: `racist`

Important facts about URLs:

1. Some parts are case sensitive and some are not:
    - The scheme (e.g., "http", "https", "ftp") of a URL is case-insensitive, meaning "http" and "HTTP" are considered the same.
    - The domain name (e.g., "example.com") of a URL is case-insensitive in the DNS system, meaning "example.com" and "EXAMPLE.COM" will point to the same IP address. However, the domain name is case-sensitive in some web server configurations, so it's possible that "example.com" and "EXAMPLE.com" are treated as different sites.
    - The path (e.g., "/path/to/resource") of a URL is case-sensitive in most web servers, meaning that "example.com/path" and "example.com/Path" are treated as different resources.
    - The query parameters (e.g., "?param1=value1&param2=value2") are case-sensitive, meaning that "param1=value1" and "param1=Value1" are treated as different values.

> /!\ During the update of an API, you must decide wisely which parts should be modified. Unless there is a security breach, you will want to continue the service for your consumers without having them to make any kind of modification. For example, altering the case in the path of the parameters, you will force them to modify the requests in their client apps.

2. Cannot contain spaces and some other special characters. If they must exist inside it, they must be encoded in HTML Entities. Use the next list to find the preferred tool for it: 
    - https://www.htmlpurifier.org/
    - https://www.freeformatter.com/html-escape.html
    - https://www.url-encode-decode.com/
    - https://www.w3schools.com/jsref/tryit.asp?filename=tryjsref_unescape
    - https://www.htmlentities.com/

3. The first plain space or new-line character will mark the end of the URL in a request


##### Scheme (protocol)
Even though there are a lot more schemas available, the two more popular are `http` and `https`, and these are used to access sites in a browser (user-to-machine comm.) but also to make a request to an API (machine-to-machine comm.). 
Others might be as: `ftp`/`sftp`, `mq`, `imap`/`smtp`, etc. But not all of them are used to serve an API. I've seen `http[s]` API's that will forward the request to a `mq` queue. But that doesn't mean that the API is a queue, but rather that somebody built an `http[s]` API as an interface to put or consume data into/from a queue.

Difference between `http` and `https` is that the data sent and received via `https` is encrypted while the `http` transports the information in plain text format, therefore anyone who can sniff the network can have access to sensitive data in the headers and payload content. 

More about encryption in future chapters of this course. But for the moment, just remember that "s" stands for "secure", hence `https` is the encrypted one. 

##### Host
The `host` or `hostname` is the part of the URL that points to a specific machine, that is able to contact with the API application. 

The `hostname` is the human friendly address that can be read and remembered by human beings. But in reality, the network address is an IP code compose by four groups of digits in a 0-255 range, separated by a dot. Eg: `192.168.0.1`. 

A hostname (human friendly address) pointing to an IP (machine friendly address) is known as **a record**, and it is stored in a service that can be interrogated and called **DNS** (**Domain Name Service**). So usually (unless you cache the response), for each website visited and for each request sent to an API, the first step performed by the client application would be to interrogate a DNS, translating the `v2.jokeapi.dev` hostname into the `172.67.202.132` IP. This action is called **resolving an address**.

The DNSs (Domain Name Services), which are 
- spread all over the internet,
- occasionally inside your own intranet 
- also they exist locally inside your machine.

The DNS can be provided by multiple agents such as:
- public entities like Google, CloudFlare, etc. (for internet-wide public domains)
- your ISP (Internet Service Provider) (for internet-wide public domains)
- your network managers (for internal networks)
- yourself (for a local machine).

How can you gain privileges to manage the record in a DNS?
- using a "domain provider" service, which is able to put your new record in a public DNS
- asking your network admins to make the addition/update in your network's DNS
- yourself can do it locally (altering the `host` file in your machine)

You can seek the IP of a hostname in multiple ways 

    $ nslookup v2.jokeapi.dev
    Server:         192.168.0.100
    Address:        192.168.0.100#53

    Non-authoritative answer:
    Name:   v2.jokeapi.dev
    Address: 172.67.202.132
    Name:   v2.jokeapi.dev
    Address: 104.21.76.251

In the above example, the 192.168.0.100 is the IP of an internal DNS that I have set up in my network.
During the `nslookup` command, I can change the DNS to be interrogated by using a second argument:

    nslookup v2.jokeapi.dev 127.0.0.1

to start searching first in the local `/etc/hosts` file. If I want to search for Google's or CloudFlare's DNS answer, I would have to input their DNS IP instead:

    nslookup v2.jokeapi.dev 8.8.8.8 # Google DNS

    nslookup v2.jokeapi.dev 1.1.1.1 # CloudFlare DNS

> list of public DNS's: [https://www.google.com/search?q=public+dns+ip+list](https://www.google.com/search?q=public+dns+ip+list)

It's possible that you are located in a private subnet of a network which has no access to internet by default, therefore the `nslookup` command will not be able to interrogate external DNSs. You can accomplish this step in two different ways:

1. via a website like https://dns.google and inserting the `hostname`
2. or via an API request like the following:

    $ curl --request GET 'https://dns.google.com/resolve?name=v2.jokeapi.dev'
    {"Status":0,"TC":false,"RD":true,"RA":true,"AD":true,"CD":false,"Question":[{"name":"v2.jokeapi.dev.","type":1}],"Answer":[{"name":"v2.jokeapi.dev.","type":1,"TTL":300,"data":"104.21.76.251"},{"name":"v2.jokeapi.dev.","type":1,"TTL":300,"data":"172.67.202.132"}],"Comment":"Response from 108.162.192.246."}
    # you might need to use a proxy if you are behind a firewall.

Debugging the connectivity between your machine (or client app's machine) and the server can be done in multiple ways. The most simple method is sending a `ping` to the IP or hostname address: 

    $ ping -c 3 v2.jokeapi.dev
    PING v2.jokeapi.dev (104.21.76.251): 56 data bytes
    64 bytes from 104.21.76.251: icmp_seq=0 ttl=55 time=28.816 ms
    64 bytes from 104.21.76.251: icmp_seq=1 ttl=55 time=27.594 ms
    64 bytes from 104.21.76.251: icmp_seq=2 ttl=55 time=28.930 ms

    --- v2.jokeapi.dev ping statistics ---
    3 packets transmitted, 3 packets received, 0.0% packet loss
    round-trip min/avg/max/stddev = 27.594/28.447/28.930/0.605 ms

##### Subdomain
By having a look at the `v2.jokeapi.dev` hostname, we can identify the subdomain `v2`

##### Domain with extension
By having a look at the `v2.jokeapi.dev` hostname, we can identify the original domain as  `jokeapi.com`. This is important, because it gives you a clue about the owner and the location of this API. 

A **public domain** is a paid service and can belong to only one entity that will manage it. The IP resolved by public DNS will be a public IP (internet IP).

A **private domain** is free of charge and only exists inside your network. CloudFlare, Google and other public DNS services will never know about it's existence. The resolved IP is an internal IP existing in your network.

Once that the client app has the IP, it will forward the request to this address, trying to establish a connection with this machine. 

##### Port
The server machine must have one open port, listening for incoming connections. There are two standard ports, which can be omitted in the full URL. 
- port 80 for http connections
- port 443 for https connections (used in this case)

If the server has a custom port number opened for this service, it should be mentioned in the url by concatenating a colon (:) and the number right after the domain extension and before the first slash (/). In this example, can also be used as: 

    https://v2.jokeapi.dev:443/....

##### Basepath
The `host`is a very specific address, like your street and number. But still, there can be multiple applications listening in one machine, the same way as you have multiple families living in your building, at different floors and doors. Therefore, we need a way to point to a specific app installed in a machine. 

This is done via the "basepath", which is the following string after the schema + hostname + port. It starts with a slash (/) and can have multiple levels, same like a directory structure in linux machine. Often they represent exactly that: a folder hierarchy, but sometimes they represent a logical route defined in one server or application. 

Normally, the basepath is common for all the resources of the API. With an API that has only one resource is hard to tell if you are looking at a basepath or a resource. 

The basepath is used:
- to identify the API
- to route requests to the appropriate resources
- for versioning of the API (as different versions of an API can have different basepaths)

It's worth noting that the basepath is not always present in the URL, and it depends on the design of the API and the routing mechanism used by the server. For such a simple API like JokeAPI, it made no sense to have a basepath. Even more so, when the versioning was resolved via a subdomain (`v2`). Next, we will continue this course with some exercises that will help you understand the value and the looks of a basepath.

> The basepath is an important part of the API design and should be chosen carefully to ensure that it is consistent with the API's overall architecture, and that it is easily understandable and discoverable by API clients.


**Exercise one:**
The well known archive.org service is offering multiple different APIs, as seen at https://archive.readme.io/reference/getting-started:
- Item
    - https://archive.org/metadata/{id}
- Wayback Machine
    - https://archive.org/wayback/available?url=example.com
- Books
    - https://archive.org/services/img/{itemid}
    - https://archive.org/services/loans/beta/loan/index.php
    - https://archive.org/services/book/v1/do_we_have_it/?isbn={isbn}&debug={debug}&include_unscanned_books={include_unscanned_books}
- etc. 

Right away you can spot out that the https://archive.org (hostname) is similar for all the APIs, but the following element is distinct depending on the API (`/metadata`, `/wayback`, `/services`). These would be the `basepaths` of each API provided by archive.org. Still, they remain the same for multiple resources that are available within each API (`/img...`, `/loans...`, `/book...` in 'Books' API).

Note how `https://archive.org/services/book/v1/do_we_have_it/?isbn={isbn}` is using

##### Resource (or path)
One API can provide access to one or multiple resources. For the Joke API example, let's assume that no basepath was used, therefore the API is consumed directly from root (/). In this case the `/joke/Programming`is the full resource string. As you can see, there are two different elements, `joke` and `Programming`. If we look in the documentation, we can note that `/joke` is a fix element, while `/Programming` is variable and can be replaced by a different string (like `Christmas` or `Any`)

These elements are also called **path parameters**.

In the documentation, the variable parameters are often represented by a string in curly brackets: `https://archive.org/services/img/{itemid}`

**Exercise two:**
In this case, JokeAPI is a purely didactic API, therefore it has no special needs. But let's imagine that this API is part of a larger service offer of a big company and it has a major success. Now, the superiors will want to have a new API to hold and serve catch phrases. It will have different needs, different database, different code, different folder... different basepath. Oh, wait! JokeAPI has no basepath. (Here is were it would have been nice to have a basepath in the original design of the JokeAPI.)
In order to maintain an homogeneous environment, same hostname will be used. An example of a possible endpoint for the CatchphraseAPI : 
    `https://v2.jokeapi.dev/catchphrase/query/{category}?author={name}` for fetching phrases,
    `https://v2.jokeapi.dev/catchphrase/submit` for catchphrase submissions.
    `https://v2.jokeapi.dev/catchphrase/authors` to list/add/modify known authors.
    `https://v2.jokeapi.dev/catchphrase/categories` to list/add/modify categories.

The string `/catchphrase` is a basepath if:
- it's common for all the resources of this new API (`query`, `submit`, `authors`, `categories`, etc.)
- is specific to CatchphraseAPI code only


##### Query search
Made popular by the PHP, the first popular backend language for creating websites with dynamic content, the `?` separator is widely used as a common standard by multiple other languages and technologies. 

Everything that appears in the URL, after the `?` is known as `query string` or `query search`. This string is not mandatory occurrence but when it appears it can be composed by one or multiple `query parameters` separated by the `&` character. 

A `query parameter` is formed by a key/value pair. Think of them as key being the name of the variable and it's value comes right next to it, after the `=` sign. Similarly as in the execution of a shell command, these arguments are used to alter the processing in specific way.

> /!\ Notice that URLs cannot contain spaces. A space marks the end of the URL. Therefore, no spaces are allowed between the `key`, `=` and the `value`.

> /!\ Both key and value are case sensitive. `Name=Adrian`, `name=Adrian`, `name=adrian` and `Name=adrian` are not the same thing.

For the 'Joke API' example the query parameter name is `blacklistFlags` and it's value is `racist`. As explained in the documentation, this will trigger a specific filter in the API processing, that will filter out the racist jokes from the response. 

#### METHOD
An API request method, also known as an 'HTTP method', is a type of request that a client sends to a server through an API. The most common API request methods are:
- **GET**: This method is used to retrieve information from the server. A GET request typically retrieves a resource from the server, such as a web page or an image.
- **POST**: This method is used to send information to the server. A POST request typically creates a new resource on the server, such as a new user or a new blog post.
- **PUT**: This method is used to update an existing resource on the server. A PUT request typically replaces an existing resource on the server with new data.
- **DELETE**: This method is used to delete a resource on the server. A DELETE request typically removes a resource from the server.
- **PATCH**: This method is used to partially update an existing resource on the server. A PATCH request typically modifies only a subset of the fields of an existing resource on the server.

These are the most commonly used API request methods, but others exist as well such as "HEAD", "OPTIONS", "CONNECT" and "TRACE".

It's important to note that API request methods are part of the HTTP protocol, which is the underlying protocol used by the World Wide Web. Therefore, the API request methods are not specific to any particular programming language or framework, but rather, they are a standard part of the web.

> /!\ Also, it's worth mentioning that the API request method used should match the intended action on the resource, **following the principles of RESTful API design**.

#### HEADERS
HTTP headers are name-value pairs that are included in an HTTP request or response. They provide additional information about the request or response, such as the format of the data, authentication information, and caching instructions.

There are many different types of HTTP headers, but some of the most commonly used include:

Content-Type: This header specifies the format of the data in the request or response body. Examples include "application/json" for JSON data and "text/html" for HTML data.

Authorization: This header is used to provide authentication information to the server. The most common type of authentication is Basic Authentication, in which a username and password are encoded and included in the Authorization header.

Accept: This header is used to specify the format of the data that the client is expecting to receive. For example, a client may send an Accept header with a value of "application/json" to indicate that it expects to receive JSON data.

User-Agent: This header is used to identify the client software and version that is making the request. This information can be used by the server to provide different responses for different types of clients.

Cache-Control: This header is used to specify caching instructions for the request or response. For example, a client may send a Cache-Control header with a value of "no-cache" to indicate that the response should not be cached.

Set-Cookie: This header is used to set a cookie on the client's browser. Cookies are small text files that are stored on the client's computer and are used to maintain state between requests.

These are just a few examples of the many different types of HTTP headers that exist. There are many more headers that can be used to provide additional information and control the behavior of the request and response.

It's worth mentioning that headers are case-insensitive, meaning that "Content-Type" and "content-type" are the same. However, conventionally headers are written in the format of "Header-Name" and the field-values are case-sensitive.



#### PAYLOAD 






### THE RESPONSE 
Even though we are talking about a client and a server, the request (also the response) will pass through a higher number of networking devices (gateways, proxies, load balancers, firewalls, etc.). The data travels by jumping from a device to another therefore are also called "hops". 

The request can be stopped in any of these devices and this action implies: 
- it will not be forwarded to the next hop anymore. 
- the device that makes the interruption will return the response, not the real expected server 
- therefore, for debugging, we must understand and quickly identify which device returned the response and why. 

  

#### STATUS CODE
Usually a three-digit number (established via an open standard) with ranges in the hundreds, that will offer an initial clue upon the reason of the fail/success.  

1. Informational responses (100 – 199) 
2. Successful responses (200 – 299) 
3. Redirection messages (300 – 399)
4. Client error responses (400 – 499) 
5. Server error responses (500 – 599) 

> "404" is the most known status code, that points to a "Not found" exception. Even though the response was returned by the server, the bad request was sent by the client to an unknown resource, therefore being the client's fault it will be in the range of 400-499: Client error responses. 

More info: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status 

Since we are "API Managers", we must make sure that we comply with the standards at all time, as far as depends on us. 

 

#### HEADERS
Metadata about the response, with info about the: 
- payload content (content-type), 




#### PAYLOAD








THE CONNECTION 
--------------
Communication is done using HTTP Protocol which is build upon a TLS Connection, on top over a TCP Connection 



API TYPES 
---------







THE SECURITY
------------




