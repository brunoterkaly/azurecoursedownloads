## Source Code for doGet() in HelloAzureFromWeb

### Hands-On: Hello World with a Java Application and a Java Dynamic Web Application (Hands-On)


```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
	// TODO Auto-generated method stub
	// Send text / html to the browser
	response.setContentType("text/html");;
	
	// PrintWriter is used to send HTML to the browser
	PrintWriter printWriter  = response.getWriter();
	
	// Get the Java Version
	String  javaVersion = "JRE version = " + System.getProperty("java.version");
	
	// Get the current date
	String currDate = "The time is " + new java.util.Date().toString();
	
	// Show the Date and Java Version
	printWriter.println("<h1>" + currDate + "<br/>" + javaVersion +  "</h1>");
	
	
	
}
```
