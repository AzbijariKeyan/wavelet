# Week 3 Lab
## My Search Engine (Part 1)

***

Here is my code for my SearchEngine.java: 

```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    int num = 0;
    ArrayList<String> items = new ArrayList<String>();
    ArrayList<String> elements = new ArrayList<String>();

    public String handleRequest(URI url) {
        if (url.getPath().equals("/")) {
            return String.format("Search for something or add it to the list!");
        } else if (url.getPath().contains("/search")) {
            String[] trySearch = url.getQuery().split("=");
            elements.clear();
            if (trySearch[0].equals("s")) {
                if (items.contains(trySearch[1])) {
                    for (String element : items){
                        if(element.contains(trySearch[1])) {
                            elements.add(element);
                        }
                    }
                    return String.format("Found These: '%s'", elements);
                }
                return String.format("searched: '%s'. Found no results, add it to the list.", trySearch[1]);
            }
            else {
                return String.format("Can't Search.");
            }

        } else {
            System.out.println("Path: " + url.getPath());
            if (url.getPath().contains("/add")) {
                String[] parameters = url.getQuery().split("=");
                if (parameters[0].equals("s")) {
                    items.add(parameters[1]);
                    return String.format("Added '%s'. Here are the items: %s", parameters[1], items);
                }
            }
            return "404 Not Found!";
        }
    }
}

class SearchEngine {
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
***
Somehow it suddenley will print multiple of past add runs. I don't know why, when I originally ran it a few days ago it worked fine. Also, when I run 
NumberServer.java I have the same problem but I haven't changed anything about that program. This is my output after adding, "apple", "app", "cse15l",
and "broke". I only added each of these strings once but I get this: ![Image](https://azbijarikeyan.github.io/cse15l-lab-reports/added.png) For `/add` I am using the 
handleRequest method, and specifically the `else` statement at the end of it. The argument of handleRequest is the url of the page I am using, I think. The
url changes, when I want to add something to the list. It changes before anything happens in code.

My Search works fine and gives me an output like this: ![Image](https://azbijarikeyan.github.io/cse15l-lab-reports/searched.png) For `/search` I am using the 
handleRequest method, and specifically the `if` and the `else-if` statements. The argument of handleRequest is the url of the page I am using. The url, 
changes when I want to search for something in the list. It changes before anything happens in code.

Here are screenshots of what the page looks like with the url in frame: ![Image](https://azbijarikeyan.github.io/cse15l-lab-reports/URL.png)

***

## Bugs (Part 2)
***

The first bug I saw was in `ArrayExamples.java`. The test I ran to exploit this bug was:
```
@Test
  public void testReversed2() {
    int[] input1 = {1, 2, 3};
    assertArrayEquals(new int[]{3, 2, 1}, ArrayExamples.reversed(input1)); 
  }
```
The output I recieved from this test was, “arrays first differed at element [0]; expected:<1> but was:<0>”.
The bug is `arr[i] = newArray[arr.length - i - 1];`, this will always put 0 into the array.
The test was expecting the arrays to be switched, but instead the bug will put 0 into the array instead because there is nothing in `newArray`.

The second bug I saw was in `ListExamples.java`. The test I ran to exploit this bug was:
```
@Test
    public void testMerge() {
        List<String> input1 = new ArrayList<>();
        List<String> input2 = new ArrayList<>();
        List<String> expected = new ArrayList<>();
        input1.add("a");
        input1.add("c");
        input2.add("b");
        input2.add("d");
        expected.add("a");
        expected.add("b");
        expected.add("c");
        expected.add("d");
       // ListExamples.merge(input1, input2);
        assertEquals(expected, ListExamples.merge(input1, input2));


    }
```
The output I recieved from this test was, "OutOfMemoryError: Java heap space".
The bug is `index1 += 1` in the third `while` loop, this should be `index2 += 1` instead. 
The test was expecting a return of [a,b,c,d] but instead because of the incorrect `while` loop we will always enter an infinite loop before we return 
anything. It wouldn't matter what you input, at the third `while` loop the program will always enter an infinite loop, unless after the first `while` loop
`index2` is already larger than `list2`.




