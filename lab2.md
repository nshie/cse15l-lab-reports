# Lab Report 2

### Part 1

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
![Screenshot 1](/lab2-screenshot-test1.png)
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

![Screenshot 2](/lab2-screenshot-test2.png)
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

### Part 2
![Screenshot 3](/lab2-screenshot-private-key.png)
The absolute path of my private SSH key for logging into ieng6 is:
`c/Users/njshi/.ssh/id_rsa`

![Screenshot 4](/lab2-screenshot-public-key.png)
The absolute path of my public SSH key for logging into ieng6 is:
`/home/linux/ieng6/oce/34/534/nshie/.ssh/authorized_keys`

### Part 3
In the week 2 lab, I learned that you could write HTTP servers in Java. I don't know why I thought you couldn't, but that was a capability of Java that I didn't expect. I also learned that there are some files that you have to compile at the same time as other files because of dependencies inside the code. I didn't realize that javac took more than 1 argument for compiling multiple files at the same time.
