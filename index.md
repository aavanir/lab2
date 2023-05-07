# Lab Report 2
## Part 1:

Code:
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

```

[Image](Screen Shot 2023-05-07 at 2.33.59 PM.png)
- The methods in my code are called handleRequest, and there is a main method. 
- The relevant parameter for handleRequest is url which takes in an argument in the form of /add-message?s=<string>. In this case, the relevant argument would be "Hello" (without the quotes). Relevant fields of the class are this.path which is set equal to path (the parameter of the class StringHandler) and this.lines which is set equal to Files.readAllLines(Paths.get(path)). In this case, the argument that path is set equal to is "Hello".
- The value of the field this.path changes based on the argument given to the class StringHandler. The value of the field this.lines changes based on how the path field changes. In this case, the this.path field is set equal to the argument "Hello", so this.lines field will chaneg accordingly.

[Image](Screen Shot 2023-05-07 at 2.34.08 PM.png)
- The methods in my code are called handleRequest, and there is a main method. 
- The relevant parameter for handleRequest is url which takes in an argument in the form of /add-message?s=<string>. In this case, the relevant argument would be "How are you" (without the quotes). Relevant fields of the class are this.path which is set equal to path (the parameter of the class StringHandler) and this.lines which is set equal to Files.readAllLines(Paths.get(path)). In this case, the argument that path is set equal to is "How are you".
- The value of the field this.path changes based on the argument given to the class StringHandler. The value of the field this.lines changes based on how the path field changes. In this case, the this.path field is set equal to the argument "How are you", so this.lines field will chaneg accordingly.
 
[Image](Screen Shot 2023-05-07 at 2.34.16 PM.png)


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
