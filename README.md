# Distributed Systems Project
Java RMI frameworks to develop a remote, asynchronous dictionary lookup service.

## Project Overview
A JSP page should provide users with the ability to specify a string which will be checked against the dictionary. The HTML form information should be dispatched to a servlet that adds the client request to an in-queue and then returns a job ID to the web client. The web client should poll the web server periodically (every 10 seconds) and query if the request has been processed. Client requests in the inQueue should be periodically removed and processed (every 10 seconds). The processing of a client request will require a RMI method invocation to a remote object which implements an interface called DictionaryService. The remote object which implements DictionaryService should check if the string received exists in the dictionary, and return the dictionary definition of the string if it does exist in the dictionary, or "String not found" if it does not exist in the dictionary. Once the result of the dictionary lookup has been computed by the remote object, the returned response should be added to the outQueue on the Tomcat server and returned to the original web client when they next poll the server.

## Project Requirements
The following are the minimum requirements for this project.
+ A web client request should be placed in a message queue to await processing. Each request should be allocated a job number. The job number should be added to an inQueue (Map) along with the request string. The servlet handler should return the job number to the client which in turn should poll the server every 10 seconds for a response. When a response is received with a completed task, the result of the dictionary lookup should be displayed in the browser.
+ An interface called DictionaryService should expose a remote method with the following signature:

	```
	public String lookup(String s) throws RemoteException;
	```

	Where s is the string to lookup in the dictionary, and the string returned is either the dictionary definition of s or the text "String not found". In the DictionaryServiceImpl, before looking up the query string in the dictionary the thread should be put to sleep for a time, i.e. Thread.sleep(1000), to slow the service down and simulate a real asynchronous service.
+ A sample dictionary containing ~50 words.


## Running the application
This section will describe how to create and run both the JAR and WAR files.

### Dictionary service
1. Open a terminal and navigate to the "bin" folder of the Eclipse project.
2. Create a JAR with the following command:					`jar –cf dictionary-service.jar *`
3. Start the dictionary service with the following command:	`java –cp ./dictionary-service.jar ie.gmit.sw.ServiceSetup`

### Web application
1. Open a terminal and navigate to the "job-server" folder.
2. Next create a WAR. The easiest way to do this is by opening the project in Eclipse, right click on the project, go to Export -> WAR.
3. Download Tomcat if you haven't done so already.
4. Copy the WAR to the webapps folder under tomcat.
5. Start Tomcat by navigating to bin folder in Tomcat and running startup.
6. In a browser, navigate to [http://localhost:8080/job-server/](http://localhost:8080/job-server/).


