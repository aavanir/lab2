# Lab Report 2
## Part 1:

Code:

In StringServer.java file:
```
import java.io.IOException;
import java.net.URI;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

class StringHandler implements URLHandler {
  List<String> lines;
  String path;
  StringHandler(String path) throws IOException {
    this.path = path;
    this.lines = Files.readAllLines(Paths.get(path));
  }
  public String handleRequest(URI url) throws IOException {
    String query = url.getQuery();
    if(url.getPath().equals("/add-message")) {
      if(query.startsWith("s=")) {
        String toAdd = query.split("=")[1];
        this.lines.add(toAdd);
        return String.format("%s added, there are now %s lines\n", toAdd, this.lines.size());
      }
      else {
        return "/add requires a query parameter s\n";
      }
    }
    else {
      return String.join("\n", lines) + "\n";
    }
  }
}

class StringServer {
  public static void main(String[] args) throws IOException {
    if(args.length == 0){
      System.out.println("Missing port number! Try any number between 1024 to 49151");
      return;
    }
    if(args.length == 1){
      System.out.println("Missing file path! Give a path to a text file as the second argument.");
      return;
    }

    int port = Integer.parseInt(args[0]);

    Server.start(port, new StringHandler(args[1]));
  }
}
```

In Server.java file:
```
import java.io.IOException;
import java.io.OutputStream;
import java.net.InetSocketAddress;
import java.net.InetAddress;
import java.net.URI;

import com.sun.net.httpserver.HttpExchange;
import com.sun.net.httpserver.HttpHandler;
import com.sun.net.httpserver.HttpServer;

interface URLHandler {
    String handleRequest(URI url) throws Exception;
}

class ServerHttpHandler implements HttpHandler {
    URLHandler handler;
    ServerHttpHandler(URLHandler handler) {
      this.handler = handler;
    }
    public void handle(final HttpExchange exchange) throws IOException {
        // form return body after being handled by program
        try {
            String ret = handler.handleRequest(exchange.getRequestURI());
            // form the return string and write it on the browser
            exchange.sendResponseHeaders(200, ret.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(ret.getBytes());
            os.close();
        } catch(Exception e) {
            String response = e.toString();
            exchange.sendResponseHeaders(500, response.getBytes().length);
            OutputStream os = exchange.getResponseBody();
            os.write(response.getBytes());
            os.close();
        }
    }
}

public class Server {
    public static void start(int port, URLHandler handler) throws IOException {
        HttpServer server = HttpServer.create(new InetSocketAddress(port), 0);

        //create request entrypoint
        server.createContext("/", new ServerHttpHandler(handler));

        //start the server
        server.start();
        System.out.println("Server started at http://" + InetAddress.getLocalHost().getHostName() + ":" + port);
        System.out.println("(Or, if it's running locally on this computer, use http://localhost:" + port + " )");
    }
}
```

![Image](Screen Shot 2023-05-07 at 2.33.59 PM.png)
- The methods in my code that are called are handleRequest and main method in StringServer.java. 
- The main method takes in the port number and text file as arguments. The relevant argument that handleRequest takes in is in the form URI url where the relevant argument/value would be http://localhost:1025/add-message?s="string" where "Hello" (without the quotes) is the string. Relevant fields of the class are this.path which is set equal to path (the parameter of the class StringHandler) and this.lines which is set equal to Files.readAllLines(Paths.get(path)). 
- The field this.path changes based on how the value changes based on the argument given to the method handleRequest. The value of the field this.lines changes based on how the path field changes. In this case, the path field changes based on how "/add-message=Hello" changes and this.lines field will change accordingly.

![Image](Screen Shot 2023-05-07 at 2.34.08 PM.png)
- The methods in my code that are called are handleRequest and main method in StringServer.java. 
- The main method takes in the port number and text file as arguments. The relevant argument that handleRequest takes in is in the form URI url where the relevant argument/value would be http://localhost:1025/add-message?s="string" where "How are you" (without the quotes) is the string. Relevant fields of the class are this.path which is set equal to path (the parameter of the class StringHandler) and this.lines which is set equal to Files.readAllLines(Paths.get(path)).
- The field this.path changes based on the argument given to the method handleRequest. The value of the field this.lines changes based on how the path field changes. In this case, the path field changes based on how "/add-message=How are you" changes and this.lines field will change accordingly.
 
![Image](Screen Shot 2023-05-07 at 2.34.16 PM.png)


## Part 2:



Failure inducing input:
```
@Test
  public void testReversed() {
    int[] input = {4, 5, 6};
    assertArrayEquals(new int[]{6, 5, 4}, ArrayExamples.reversed(input));
  }
 ```

Input that doesn't induce failure:
```
@Test
  public void testReversed() {
    int[] input1 = { };
    assertArrayEquals(new int[]{ }, ArrayExamples.reversed(input1));
  }
```
Symptom/Ouput: ![Image](Screen Shot 2023-04-24 at 11.20.33 PM.png)

Code with bug:
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = newArray[arr.length - i - 1];
    }
    return arr;
  }
```

Code without bug:
```
static int[] reversed(int[] arr) {
    int[] newArray = new int[arr.length];
    for(int i = 0; i < arr.length; i += 1) {
      newArray[i] = arr[arr.length - i - 1];
    }
    return newArray;
  }
 ```
 
The fix addresses this issue because arr and newArray need to be switched so that ```newArray[i] = arr[arr.length - i - 1]``` and not the other way around (```arr[i] = newArray[arr.length - i - 1]```).

## Part 3:
Something that I learned fromlab in week 2 is how to work build and run a server and work with code that corresponds to it. I learned about how the The URLHandler Interface works as well as what ports are and what the localhost is.  
