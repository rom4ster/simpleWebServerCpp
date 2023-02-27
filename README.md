Simple web server based on https://gist.github.com/laobubu/d6d0e9beb934b60b2e552c2d03e1409e

The server has now been converted to C++ instead of being in C, which should alleviate linking the horrendous linking issues that can be faced for this thing. 


ONLY COMPATIBLE ON *NIX SYSTEMS


To use, make sure ```main()``` has a call to the ```serve_forever(<port>)``` function specifying a port to run on

Next, implement the ```route()``` function. 


The route function should look like this

```cpp

void route() {
// Initialization work

ROUTE_START() //NO ; allowed

// REQUEST_METHOD may be GET, or POST currently
ROUTE_<REQUEST_METHOD>("<route_name_here>") {
  
  // handle request

}


ROUTE_<REQUEST_METHOD>("<route_name_here>") {
  
  // handle request

}

//...

// Fill in as many Routes as required


ROUTE_END() //NO ; allowed


}

```


Here are some helpful variables that can be used inside a route 

```cpp
extern char    *method,    // "GET" or "POST"
*uri,       // "/index.html" things before '?'
*qs,        // "a=1&b=2"     things after  '?'
*prot;      // "HTTP/1.1"

extern char    *payload;     // for POST
extern int      payload_size;

char *request_header(const char* name);

``` 

Use the ```printf()``` function to send a response back to the user
You will need to know the structure of your response as well as the required headers
Remember to use ```"\r\n``` to differentiate headers and ```"\r\n\r\n"``` to end the header
section and move on to the body of the response.



Here is a sample makefile for this app 


```
all: server

clean:
	@rm -rf *.o
	@rm -rf server

server: main.o httpd.o
	g++ -o server $^

main.o: main.c httpd.h
	g++ -c -o main.o main.c

httpd.o: httpd.c httpd.h
	g++ -c -o httpd.o httpd.c
```






