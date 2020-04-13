# NodeMCU
## Introduction to NodeMCU and IOT
### What is NodeMCU?
![nodemcu](/images/nodemcu.jpg)
**NodeMCU** is an open source development board and firmware based in the widely used ESP8266-12E WiFi module. With just a few lines of code you can establish a WiFi connection and define input/output pins according to your needs exactly like arduino, turning your ESP8266 into a web server and a lot more. It is the WiFi equivalent of ethernet module. Now you have Internet of Things (IOT) real tool.
### What is IOT?
**The Internet of Things** is actually a pretty simple concept, it means taking all the things in the world and connecting them to the internet. So,The Internet of Things, or IOT, refers to the billions of physical devices around the world that are now connected to the internet, all collecting and sharing data.
## Setting up the NodeMCU Board on your laptop
1. Open Aurdino IDE.
2. Open the File and click on the **Preferences** as shown in the figure.
3. In the Additional Boards Manager enter URL- http://arduino.esp8266.com/stable/package_esp8266com_index.json. As highlighted in the figure and enter OK.

![boards_manager](/images/boards.jpg)


4. Now open the tools in that select **Board: “Arduino/Genuino Uno”** and click on the **Boards Manager** as shown in the figure.

![selecting_boards](/images/bs.jpg)

5. The Boards Manager window opens, and then type in the search box-'ESP8266'. Once we get it, select that module and select version and click on the Install button. When it is installed it shows Installed in the module as shown in the figure and then close the window.

![package_installation](/images/package.jpg)

6. Select the **Board: NodeMCU 1.0 (ESP-12E Module)**.

![selecting_12E](/images/12e.jpg)

7. Connect NodeMCU to your computer through USB cable.
8. Then select the **port** and **upload the code**.
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
Then, we send the request by calling the GET method on the http object. This method will return the status of the operation, which is important to store for error handling. If the value is greater than 0, then it’s a standard HTTP code. If the value is less than 0, then it’s a client error, related with the connection. 
```
int httpCode = http.GET();
```
So, if the code is greater than 0, we can get and print the response payload, by calling the getString method on the http object.
```
String payload = http.getString();
Serial.println(payload);
```
Finally, we call the end method. This is very important to close the TCP connection and thus free the resources.
```
http.end();
```
## Logging Up The Data In a Spreadsheet
### Creating Google Script in Google Sheet for Data Logging
1. Login to the Gmail with your Email ID and Password.
2. Go to the App Icon In Top Right Corner Highlighted in Green Circle and Click on Docs.
![spread](/images/spread1.png)
3. The Google Docs screen will appear. Now choose Sheets in the right sidebar.
4. Create a New Blank Sheet.
5. The Blank Sheet will be created with an “Untitled Spreadsheet”. Lets assume we rename it to ‘ESP8266_Temp_Logger’. You can add multiple sheets and can rename it to your choice. Here,we have changed the name of the sheet to ‘TempSheet.
6. After renaming the created Spreadsheet Project and Sheet name, now its time to create a Google script. Now go to ‘Tools’ marked in green circle and click on “<> Script Editor” option marked on red circle.
![spread2](/images/spread2.png)
7. The new Google Script is created with “Untitled project”. You can rename this Google Script File to any name you want. In my Case I have renamed to “Untitled project” > “TempLog_Script”.
8. See the code in this file (link to the code file) and understand and try to tinker with it.
9. Edit the sheet name and the sheet ID from the sheet URL just like shown below. https://docs.google.com/spreadsheets/d/xxxxxxxxyyyyyyzzzzzzzzzz/edit#gid=0, where “xxxxxxxxyyyyyyzzzzzzzzzz” is your Sheet ID.
10. Save the file. If you want to make your own sheet then change your credentials such as Sheet ID, Sheet Name and Sheet Project Name.
11. Now we have finished the Setting up Google Script in Spreadsheet. Now it’s time to get the major credential i.e. Google Script ID which will be written in the Arduino Program. If you make mistake in the copying Google Script ID then the data won’t reach to Google Sheet.


