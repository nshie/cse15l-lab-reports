### Lab Report 1

## Command: cd
`cd` without any arguments takes the user back to the home directory
```
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$ 
```
My working directory was `/home/lecture1`, and running `cd` changed my working directory to `/home`

`cd directory` takes the user to that directory if it exists
```
[user@sahara ~]$ cd lecture1
[user@sahara ~/lecture1]$ 
```
My working directory was `/home`, and running `cd lecture1` changed my working directory to `/home/lecture`

`cd file` just provides an error saying that file is not a directory
```
[user@sahara ~/lecture1]$ cd Hello.java
bash: cd: Hello.java: Not a directory
```
This is an error because my working directory cannot change to a file

## Command: ls
`ls` without any arguments prints the files and directories in the working directory
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
```
This listed all the files and directories in my working directory: `/home/lecture1`

`ls directory` prints the files and directories inside the directory provided
```
[user@sahara ~/lecture1]$ ls messages
en-us.txt  es-mx.txt  tr.txt  zh-cn.txt
```
This printed all the files and directories in `/home/lecture1/messages`

`ls file` just prints the name of the file if it exists
```
[user@sahara ~/lecture1]$ ls README
README
[user@sahara ~/lecture1]$ ls nonexistant
ls: cannot access 'nonexistant': No such file or directory
```
The first command provided a file that exists in the directory, so ls prints the file name back
The second command proivded a nonexistant file, so the error notifies the user that the file doesn't exist in the directory

## Command: cat
`cat` without any arguments enters a mode that doesn't allow commands and repeats back whatever you type (exit with `Ctrl+C`)
```
[user@sahara ~]$ cat
haha
haha
hehe
hehe
repeat
repeat
^C
[user@sahara ~]$ 
```
Every line typed is echoed back by the terminal until the user presses `Ctrl + C`

`cat directory` just responds with `directory: Is a directory` if the specified directory exists
```
[user@sahara ~]$ cat lecture1
cat: lecture1: Is a directory
[user@sahara ~]$ cat nonexistant
cat: nonexistant: No such file or directory
[user@sahara ~]$ 
```

`cat file` outputs the contents of the file if the file exists
```
[user@sahara ~/lecture1]$ cat Hello.java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class Hello {
  public static void main(String[] args) throws IOException {
    String content = Files.readString(Path.of(args[0]), StandardCharsets.U
TF_8);    
    System.out.println(content);
  }
}[user@sahara ~/lecture1]$
```
The `Hello.java` file contains that text, so `cat` outputs that text
