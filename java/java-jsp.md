### JSP
- JSP Elements
```jsp
<%-- JSP Comments --%>

<h3>JSP Scriptlet</h3>
<% 
	int a= 10;
	int b = 20;
	out.print(a+b);
%>

<h3>JSP Expressions</h3>
<p>Current Date info : <%= new java.util.Date() %></p>

<h3>JSP Declarations</h3>
<%! private int pageviews; %>
<%! public void incrementPageviews() 
{ 
	++pageviews; 
} 
%> 

<h3>JSP Page Directives</h3>
<%-- import package java.sql.* --%>
<%@page import="java.sql.*" %>
<%@page session="false" %>

<h3>JSP Include Directives</h3>
<%@include file="test.html" %>
<%--
import - Used to import class, interface, package
extends - Just like extends keyword of a class, we can tell the classname that the servelt will extend
info -  information of the JSP page using getServletInfo()
buffer - output buffer size
language - scripting language used. Default is Java
isELIgnored - If true, EL is ignored
contentType - Defines the MIME(Multipurpose Internet Mail Extension) type of the HTTP response.The default value is "text/html;charset=ISO-8859-1".
isThreadSafe - If false, web container will serialize the request and process one at a time
autoFlush -  indicates the container to flush the data or not when the buffer gets filled to be sent to client.
session - If false, container does not take initiative to maintain sessions
pageEncoding - lets us specify the char encoding being used
errorPage - if an exception occers in current page, container will take you to the given error page
isErrorPage - used to tell the container that the current page is an error page
--%>

<h3>JSP Taglib Directive</h3>
```
