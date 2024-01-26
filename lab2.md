My ChatServer.java file contains the following code:

```java
import java.io.IOException;
import java.net.URI;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    String string = "";

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return string;
        } else {
            if (url.getPath().contains("/add-message")) {
              String[] parameters = url.getQuery().split("=");
                if (parameters.length > 2 && parameters[0].equals("s")) {
                  String[] params2 = parameters[1].split("&");
                  //[s, messageString&user, userString]
                  if (params2.length > 1 && params2[1].equals("user")) {
                    String line = parameters[2] + ": ";
                    line += params2[0] + "\n";
                    string += line;
                  }  
                  return string;
                }
            }
            return "404 Not Found!";
        }
    }
}

class ChatServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```

Here are the screenshots showing my server working properly as a chat server:
![Screenshot 1](https://github.com/nshie/cse15l-lab-reports/blob/main/lab2-screenshot-test1.png)
This first screenshot shows the handleRequest function, taking in the URL I supplied at the top of the screenshot (http://localhost:2000/add-message?s=hey%20look,%20it%20works!&user=test1). The local `string` field is being updated from
```
Nathan: How are you
Nate: decent
n:
```
to
```
Nathan: How are you
Nate: decent
n:  
test1: hey look, it works!
```
This adds the line `test1: hey look, it works!`.

![Screenshot 2](https://github.com/nshie/cse15l-lab-reports/blob/main/lab2-screenshot-test2.png)
This first screenshot shows the handleRequest function, taking in the URL I supplied at the top of the screenshot (http://localhost:2000/add-message?s=yeah,%20it%20does!&user=test2). The local `string` field is being updated from
```
Nathan: How are you
Nate: decent
n:  
test1: hey look, it works!
```
to
```
Nathan: How are you
Nate: decent
n:  
test1: hey look, it works!
test2: yeah, it does!
```
This adds the line `test2: yeah, it does!`.
