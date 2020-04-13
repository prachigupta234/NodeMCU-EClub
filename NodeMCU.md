## Setting Up Internet Connectivity
First, we need to include some libraries, which should be available after the installation of the ESP8266 libraries for the Arduino IDE.
```
#include <ESP8266HTTPClient.h>
#include <ESP8266WiFi.h>
```
In the setup function, we will just initialize the Serial connection, in order to print the results of our application. Besides that, we need to establish a connection to an AP (Access Point), so the NodeMCU will be able to make the HTTP Requests. The code is specified below.
```
void setup() {
 
  Serial.begin(115200);             	//Serial connection
  WiFi.begin("yourSSID", "yourPASS");   //WiFi connection
 
  while (WiFi.status() != WL_CONNECTED) {  //Wait for the WiFI connection completion
 
    delay(500);
    Serial.println("Waiting for connection");
 
  }
 
}
```
## Setting Up Post Request
First of all, we need to declare an object of the class HTTPClient, from which we will call various methods to prepare the headers and content of the request, send it and check for the result. We will call this object simply “http”.
```
HTTPClient http;
```
After that, we call the begin method on the http object and pass the URL that we want to connect to and make the post request. In this case, I’m sending the post request to an application running on my local network, which is why I’m sending it the format seen below (Host IP:Port/Path).
```
http.begin("http://192.168.1.88:9999/hello");
```
Nevertheless, we can send the request to a website by specifying it’s domain name, as seen below (the destination website specified implements a dummy REST API for testing and prototyping).
```
http.begin("http://jsonplaceholder.typicode.com/users");
```
Next, we can define headers with the addHeader method. In this case, we are specifying the content-type as “text/plain”, since we will just send a simple string in the body.
```
http.addHeader("Content-Type", "text/plain");
```
The body of the request is specified as a parameter when calling the POST method on the HTTPClient object. In this case, we will simply send a string saying “Message from ESP8266”. The return value of this method corresponds to the HTTP response code and thus is important to check for error handling.
```
int httpCode = http.POST("Message from ESP8266");
```
We can now get the payJust to handle any possible WiFi connection errors, we will include a validation of the connection status before making the request. For debugging purposes, we will print both the response payload and the HTTP code.

load by calling the getString method, which will return the response payload as a string.
```
String payload = http.getString();
```
In the end, we need to call the end() method on the object to guarantee that the TCP connection is closed. This is very important to free the resources.
```
http.end();
```
Just to handle any possible WiFi connection errors, we will include a validation of the connection status before making the request. For debugging purposes, we will print both the response payload and the HTTP code.

## Setting Up Get Requests

The code for the request will be specified in the main loop function. First, we declare an object of class HTTPClient, which we will simply call http. This class provides the methods to create and send the HTTP request.
```
HTTPClient http;
```
After that, we call the begin method on the http object and pass the URL that we want to connect to and make the GET request. The destination website specified here implements a dummy REST API for testing and prototyping.
```
http.begin("http://jsonplaceholder.typicode.com/users/1");
```
