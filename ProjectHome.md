# WebServiceServlet (wss) #

WebServiceServlet enable developers to turn they standard java classes into WebServices that can be run on Google App Engine and other servlet engines like Tomcat, Glassfish etc.

Developing WebServices using wss is slightly different than standard web service development. For Clients wss webservices looks like standard web services. Client classes can be generated e.g. using java's wsimport.

**How to use wss?** For this please see the wiki pages.

More detailed information about wss you can found from Downloads -> WebServiceServlet\_UserGuide.pdf



This project is part of our EdifactAPI project which you can find at http://code.google.com/p/edifactapi/

If you want to see wss in real action running on Google App Engine, please click the following link:

http://www.vnetcon.org/invoice?wsdl

This will show you EdifactAPI project's invoice spcification part served as WebService, powered by wss.

Please note that Google Chrome will show only blank page.
To see the generated wsdl you need to use Mozilla or IE.



WSS is also used in BlobDB JDBC/SQL like database project as WebService interface for
BlobDB clients. BlobDB can be run also in **gateway mode** so you might be interested of this too. You can find this project at

http://code.google.com/p/blobdb/


## Simple WebService example ##

Below is a simple webservice class for wss to run. It is a normal java class
which public methods are treated as web service method by wss.

```
package org.vnetcon.wss.test;

import org.vnetcon.wss.test.dao.Employee;

public class MyWebService {

	public MyWebService(){
	}
	
	public String addEmployee(Employee emp){
		//TODO implement actual behaviour
		return "ok: employee added";
	}
	
}
```



## Simple WebService client example ##

Below is a simple client that call web service described above.
Required web service client java classes for this client have been generated
using wsimport (command line tool that comes with java sdk).


```
package org.vnetcon.wss.testclient;

import org.vnetcon.wss.test.Employee;
import org.vnetcon.wss.test.MyWebService;
import org.vnetcon.wss.test.MyWebServiceService;

public class WSSExampleClient {

	public static void main(String[] args){
		
		try{
			MyWebServiceService mwss = new MyWebServiceService();
			MyWebService myws = mwss.getMyWebServicePort();
			Employee emp = new Employee();
			String s = myws.addEmployee(emp);
			System.out.println("WebService returned: " + s);
		}catch(Exception e){
			e.printStackTrace();
		}
		
	}
	
}
```

Please note that if you want to **call** web service on GAE I suggest you to investigate apache axis. I think they have solution where they use URLConnection for
communication with web service and this should be ok on gae too.


## Using WebServiceServlet in commercial applications? ##
Please visit at http://www.vnetcon.org. With this we try to be as fair as possible. For us this mean that you give us your price and thats quite much about it :) We think our commercial license more like a donation.  Please note that this will apply only if you don't want to deliver your own source code with your application that uses WebServiceServlet.
