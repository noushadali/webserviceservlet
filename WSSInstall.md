# How to get WebServiceServlet up and running? #

Here are step by step instructions how to get WebServiceServlet (wss) up and running in Eclipse Web Application Project on Google App Engine version 1.4. In here I assume that you are familiar with Eclipse and creating Google App Engine Web Application Projects with it.


## Required libraries ##

You need to download following libraries before you start

  * **wss-0.7b.jar** (this one you can find at this site downloads)
  * **serializer.jar, xalan.jar, xercesImpl.jar, xml-apis.jar** (these libraries you can download at http://xml.apache.org/xalan-j/ )


## Steps to go ##

You need to do following steps to get everything work properly.

  1. Create Web Application project
  1. Add required libraries into WEB-INF/lib folder
  1. Create normal java class which public methods wss will treat as web service methods - this will be your web service class.
  1. Create schema file of all objects your web service class use
  1. Tell to wss where your web service class locates in your application classes (WEB-INF/classes folder). This one you will do in web.xml
  1. Tell to wss where the schema file created of objects locates (in WEB-INF/classes folder)
  1. Test your environment


### 1. Creating Web Application Project ###

In here I assume you already know how to do this


### 2. Adding libraries into WEB-INF/lib ###

After you have created your project add the libraries you downloaded into your project's WEB-INF/lib folder.


### 3. Creating web service class ###

You can create these classes where ever you want in your Web Application project. In this example I created these classes into org.vnetcon.test package. Into this package I created two java classes:

  * WebService.java (this is my web service)
  * Employee.java (this is my object that my web service use)

Below are these files source code

```

package org.vnetcon.test;

public class WebSerivce {

	public String setEmployee(Employee emp){
		return "ok: " + emp.getName() + " set.";
	}
	
	public Employee getName(){
		Employee emp = new Employee();
		emp.setName("Servers says hello");
		return emp;
	}
		
}

```

```

package org.vnetcon.test;

import javax.xml.bind.annotation.XmlRootElement;
import javax.xml.bind.annotation.XmlType;

@XmlRootElement
@XmlType(name="Employee") // this type name must be same then class name
public class Employee {

	private String name;
	
	public String setName(String name){
		this.name = name;
		return "ok: " + name;
	}
	
	public String getName(){
		return "ok: getName called: " + this.name;
	}
	
}

```

When you compile your project you can find these classes in your WEB-INF/classes folder (this is something you cant see in Eclipse, to see this folder you must look at it using e.g. in windows file explorer)


### 4. Creating schema file for wss ###

WebService serlvet will use schema file and your class information when giving information to outer world as response to http://xxx.xxx.xxx/paht/to/webservice?wsdl (wsdl) and http://xxx.xxx.xxx/paht/to/webservice?xsd=1 (schema information for client side). Some of this information is retrieved from your WebService class by wss (for this wss uses reflection) and some of this information is parsed from schema file. To create your schema file do following steps (I assume you are working with windows here)

  1. open dos command prompt
  1. navigate to your web application projects src folder using cd c:\path\to\your\src\folder
  1. type there into dos prompt: schemagen org.vnetcon.test.Employee

Step three will create a schema1.xsd file into your projects src folder. Next you need to copy or move this file into your WEB-INF/classes folder (this one you can't see at your Eclipse project). In this example I copied the file into to war\WEB-INF\classes\org\vnetcon\test folder.

Please note that Eclipse will always clean this folder when compiling project so you need to copy this file every time after compile or alternatively you can add schema1.xsd file into your src package.


### 5. Tell to wss where your WebService class locates ###

This you will do by editing web.xml file. Below is the example configuration from web.xml. (see the comments in example)

```
<servlet>
	<servlet-name>WebServiceServlet</servlet-name>
	<servlet-class>org.vnetcon.xml.ws.servlet.WebServiceServlet</servlet-class>
	<init-param>
		<!-- This is the param name wss uses -->
		<param-name>WebServiceClass</param-name>
		<!-- This is your web service class -->
		<param-value>org.vnetcon.test.WebSerivce</param-value>
	</init-param>
	<init-param>
		<param-name>SchemaFilePath</param-name>
		<param-value>/org/vnetcon/test/schema1.xsd</param-value>
	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>WebServiceServlet</servlet-name>
	<url-pattern>/test/ws/*</url-pattern>
</servlet-mapping>

```


### 6. Tell to wss where schema1.xsd locates ###

This you will do by editing web.xml file. Below is the example configuration from web.xml. (see the comments in example)

```
<servlet>
	<servlet-name>WebServiceServlet</servlet-name>
	<servlet-class>org.vnetcon.xml.ws.servlet.WebServiceServlet</servlet-class>
	<init-param>
		<param-name>WebServiceClass</param-name>
		<param-value>org.vnetcon.test.WebSerivce</param-value>
	</init-param>
	<init-param>
		<!-- This is the param name wss uses -->
		<param-name>SchemaFilePath</param-name>
		<!-- This is the location of schema1.xsd in WEB-INF/classed folder -->
		<param-value>/org/vnetcon/test/schema1.xsd</param-value>
	</init-param>
</servlet>
<servlet-mapping>
	<servlet-name>WebServiceServlet</servlet-name>
	<url-pattern>/test/ws/*</url-pattern>
</servlet-mapping>

```


### 7. Test your environment ###

After these steps you can test your that everything is correct by starting your web application and pointing your browser at

http://localhost:8888/test/ws?wsdl

and / or

http://localhost:8888/test/ws?xsd=1