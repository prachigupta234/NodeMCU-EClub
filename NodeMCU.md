# NodeMCU
## Introduction to NodeMCU and IOT
### What is NodeMCU?
![NodeMCU](/images/nodemcu.jpg)

**NodeMCU** is an open source development board and firmware based in the widely used ESP8266-12E WiFi module. With just a few lines of code you can establish a WiFi connection and define input/output pins according to your needs exactly like arduino, turning your ESP8266 into a web server and a lot more. It is the WiFi equivalent of ethernet module. Now you have Internet of Things (IOT) real tool.
### What is IOT?
**The Internet of Things** is actually a pretty simple concept, it means taking all the things in the world and connecting them to the internet. So,The Internet of Things, or IOT, refers to the billions of physical devices around the world that are now connected to the internet, all collecting and sharing data.
## Setting up the NodeMCU Board on your laptop
1. Open Aurdino IDE.
2. Open the File and click on the **Preferences** as shown in the figure.

![prefrences](/images/pref.jpg)

3. In the Additional Boards Manager enter URL- http://arduino.esp8266.com/stable/package_esp8266com_index.json. As highlighted in the figure and enter OK.

![boards](/images/rsz_boards.jpg)

4. Now open the tools in that select **Board: “Arduino/Genuino Uno”** and click on the **Boards Manager** as shown in the figure.

![sel](/images/bs.jpg)

5. The Boards Manager window opens, and then type in the search box-'ESP8266'. Once we get it, select that module and select version and click on the Install button. When it is installed it shows Installed in the module as shown in the figure and then close the window.

![12e](/images/package.jpg)

6. Select the **Board: NodeMCU 1.0 (ESP-12E Module)**.

![12](/images/12e.jpg)

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

![spread1](/images/spread1.png)

3. The Google Docs screen will appear. Now choose Sheets in the right sidebar.
4. Create a New Blank Sheet.
5. The Blank Sheet will be created with an **Untitled Spreadshee**. Lets assume we rename it to **ESP8266_Temp_Logger**. You can add multiple sheets and can rename it to your choice. Here,we have changed the name of the sheet to **TempSheet**.
6. After renaming the created Spreadsheet Project and Sheet name, now its time to create a Google script. Now go to **Tools** marked in green circle and click on **<> Script Editor** option marked on red circle.

![spread2](/images/spread2.png)

7. The new Google Script is created with **Untitled project**. You can rename this Google Script File to any name you want. In our Case we have renamed to **TempLog_Script**.

![spread3](/images/spread3.png)

8. See the code in [this file](/code.md) and understand and try to tinker with it.
9. Edit the sheet name and the sheet ID from the sheet URL just like shown below. https://docs.google.com/spreadsheets/d/xxxxxxxxyyyyyyzzzzzzzzzz/edit#gid=0, where “xxxxxxxxyyyyyyzzzzzzzzzz” is your Sheet ID.

![spread4](/images/spread4.png)

10. Save the file. If you want to make your own sheet then change your credentials such as Sheet ID, Sheet Name and Sheet Project Name.
11. Now we have finished the Setting up Google Script in Spreadsheet. Now it’s time to get the major credential i.e. Google Script ID which will be written in the Arduino Program. If you make mistake in the copying Google Script ID then the data won’t reach to Google Sheet.
### Getting The Google script ID
1. Go to **Publish > Deploy as Web App…**

![spread5](/images/spread5.png)

2. The **Project version** will be **New**. Select **your email id** in the **Execute the app as** field. Choose **Anyone, even anonymous** in the **Who has access to the app** field. And then Click on **Deploy**.  Note that When republishing please select the latest version and then Deploy again.

![spread6](/images/spread6.png)

3. You will have to give the Google permission to deploy it as web app. Just click on **Review Permissions**.

![spread7](/images/spread7.png)

4. Then choose your Email ID here using which you have created spreadsheet.
5. Click on **Advanced**.

![spread8](/images/spread8.png)

6. And then click on **Go to ‘your_script_name’(unsafe)**. Here in my case it is **TempLog_Script**.

![spread9](/images/spread9.png)

7. Click on **Allow** and this will give the permission to deploy it as web app.

![spread10](/images/spread10.png)

8. Now you can see the new screen with a given link and named as **Current web app URL**. This URL contains Google Script ID. Just copy the URL and save it somewhere.

![spread11](/images/spread11.png)

9. Now when you copy the code, the format is like <https://script.google.com/macros/s/____Your_Google _ScriptID___/exec>. 
So here in this case my Google script ID in this link <https://script.google.com/macros/s/AKfycbxy9wAZKoPIpP53AvqYTFFn5kkqK_-av...> is **AKfycbxy9wAZKoPIpP53AvqYTFFn5kkqK_-avacf2NU_w7ycoEtlkuNt**.
Just save this Google Script to some place.
### Programming NodeMCU To Send Data To Google Sheets
1. The library ESP8266WiFi.h is used for accessing the functions of ESP8266, the HTTPSRedirect.h library is used for connecting to Google Spreadsheet Server and DebugMacros.h is used to debug the data receiving.
```
#include <ESP8266WiFi.h>
#include "HTTPSRedirect.h"
#include "DebugMacros.h"
```
2. Set up the WiFi connectivity as described in the previous part of the guide and then Enter the Google server credentials such as host address, Google script ID and port number. The host and port number will be same as attached code but you need to change the Google Scripts ID that we got from the above steps.
```
const char* host = "script.google.com";
const char* GScriptId = "AKfycbxy9wAZKoPIpPq5AvqYTFxxxkkqK_avacf2NU_w7ycoEtlkuNt"; 
const int httpsPort = 443;
```
Define the URL of Google Sheet where the data will be written. This is basically a path where the data will be written.
```
String url = String("/macros/s/") + GScriptId + "/exec?value=Temperature";  
String url2 = String("/macros/s/") + GScriptId + "/exec?cal";
```
Define the Google sheet address where we created the Google sheet.
```
String payload_base =  "{\"command\": \"appendRow\", \
                    \"sheet_name\": \"TempSheet\", \
                       \"values\": ";
```
Define the client to use it in the program ahead.
```
HTTPSRedirect* client = nullptr;
```
Start the serial debugger or monitor at 115200 baud rate.Connect to WiFi and wait for the connection to establish.
Start a new HTTPS connection. Note that if you are using HTTPS the you need to write the line setInscure() otherwise the connection will not establish with server.
```
client = new HTTPSRedirect(httpsPort);
  client->setInsecure();
  Start the respose body i.e. if the server replies then we can print it on serial monitor. 
  client->setPrintResponseBody(true);
  client->setContentTypeHeader("application/json");
```
Connect to host. Here it is "script.google.com".
Connect to host. Here it is "script.google.com".
``` 
 Serial.print("Connecting to ");
 Serial.println(host); 
```
Try connection for five times and if doesn’t connect after trying five times then drop the connection.
```
 bool flag = false;
  for (int i = 0; i < 5; i++) {
    int retval = client->connect(host, httpsPort);
    if (retval == 1) {
      flag = true;
      break;
    }
    else
      Serial.println("Connection failed. Retrying...");
  }
```
We will communicate with server with GET and POST function. GET will be used to read the cells and POST will be used to write into the cells. 
 
Read the data from whichever sensor you are using and then save that data in a variable and then attach it to the payload.We will send this data to the google sheet using the post method.
```
payload = payload_base + "\"" + sheetTemp + "," + sheetHumid + "\"}";
```
If client is connected then simply send the Data to Google Sheet by using POST function. Or save it if the data fails to send and count the failure.
```
  if (client->POST(url2, host, payload)) {
    ;
  }
  else {
    ++error_count;
    DPRINT("Error-count while connecting: ");
    DPRINTLN(error_count);
  }
```
