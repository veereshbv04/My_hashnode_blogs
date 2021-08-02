## Understanding Asynchronous JavaScript

**JavaScript** is one of the most popular programming language, it is the language that can be executed in the browser. It is interpreted, single threaded and has dynamic data type. It is having some unique behavior. By word single threaded means, it can execute only one statement at any given time. But you might have seen many instances where JavaScript code is executed in the background, as if two statements are executed in parallel. This is done using a concept called Asynchronous property of JavaScript. Let's understand this now.

### Consider the below JavaScript code

```
console.log("1");
setTimeout(()=>{
     console.log("2");
},2000);
console.log("3");
``` 
What do you think the output of the above code snippet ?

Is that "1","2","3" or "1","3","2 ?

Well the output of the above code is

```
1
3
2
``` 
Let's deep dive into the code now, and understand how and why we get that.

As mentioned earlier JavaScript is Single threaded language, meaning it can execute only **one statement at any given time**. The JavaScript interpreter, scans the code line by line.

The first statement in the code is 
> console.log("1");
 
This line of code is pushed to the stack. Stack work on the principle of First In and First Out(FIFO).


![Process (2).jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1627829488095/UKzB-l25e.jpeg)

This statement is first executed and we get the output "1". Then, that statement is popped out of the stack.

Then the next statement in our code is
> setTimeout(()=>{
       console.log("2");
},2000);

This statement is pushed to the stack. 
![Untitled.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1627830769463/QhULtnHOY.jpeg)

** setTimeout()** is not actually a part of JavaScript and it cannot be executed in call stack. Because it is a part of the **Web API** just like **DOM** .So, this statement is popped out of the stack and is pushed inside the **Web API** as shown in the below diagram.

![Untitled (1).jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1627830836430/QNfD9Pu8O.jpeg)

Now, **setTimeout()**  is inside the web API, and this can be executed by it. The second parameter 200 in the setTimeout() means that the first parameter console.log(2) is to be executed after 2 seconds. So the web API simply waits for 2 seconds. Meanwhile, the call stack is empty so the next statement in our code is pushed to it.

![Untitled (2).jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1627831153163/IhFXwlZI5.jpeg)

This line of code inside the call stack is executed and we get the output "3".

After 2 seconds the WebAPI executes the code inside it. It contains a call back. In simple words call back is something which is executed after a certain activity is done. So, the WebAPI push this call back function to call back queue.

![Untitled (3).jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1627831489712/upJGJSjW6.jpeg)

Call back queue works on the principle of First Come First Serve. If there were any statements before console.log("2") those statements will be served first. For now we have only one statement in our call back queue.
Call back queue is just a place to store on first come first serve basis and it cannot execute any code.

We have another important part called **Event Loop** . What does this do ? 

The Event Loop constantly keep checking stack and the call back queue. If the stack is empty it goes to the call back queue and check if there is any call back that is needed to be executed. For now we have console.log("2") in the call back queue. So now the event loop pop that statement and push it to the stack. Now the statement gets executed and we get the output "2".

After executing, console.log(2) is popped out of the stack .
Now all the statements in our code is executed and the program is returned.
In this way JavaScript is able to execute Asynchronous Properties. Hope you understood the concept.

Feel free to ask your queries in the comment section, connect with me on  [LinkedIn](https://www.linkedin.com/in/veereshbv04/) .
Happy Learning! Thank You 

**Veeresh B V**
 









