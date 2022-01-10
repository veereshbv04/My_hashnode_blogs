## Microtask in JavaScript

#### Before we begin
I assume you have the basic understanding of Asynchronous JavaScript and the use of setTimeout API. If not refer this [article](https://veereshbv04.hashnode.dev/understanding-asynchronous-javascript).

We know **JavaScript is synchronous single threaded language**. But it is able to manifest itself asynchronously through different ways like using callbacks, promises etc.

Let's take a small and simple code snippet

![carbon.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640528723420/QR7uqQDkg.png)

Let's understand the flow of the above code. JavaScript executes line by line. First, the console.log("start") statement is pushed onto the callstack, and we get "start" printed in the console. The callstack can execute only one statement at any given time.The second statement we have is *setTimeout*, **setTimeout is not a part of JavaScript, it is one of the many APIs which the browser provides**. setTimeout conatins a callback function which needs to be executed after 5 Seconds. 

But our callstack does not have access to timer and it has **no power to delay the execution**. Callstack executes whatever is pushed onto it immediately and have no power to stop, delay or perform any other operation. As a result JavaScript Engine access the webAPI to complete this operation. In the mean time the third statement console.log("End") is pushed onto the stack and we get "End" in our console.

After 5 Seconds the callback function from *setTimeout* is pushed onto callback queue. Now the event loop take this and pass onto callstack to execute it. So, our final output for the above code is 

![Screenshot 2021-12-26 195637.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1640528824348/woI5QqGfW.jpeg)

The primary function of event loop is to check is callstack  empty and if it is then, push the callback functions, to execute them.

Callstack, web APIs and event loop all have independent works, there is a good coordination between them and this gives JavaScript super powers.

 *Let's take another example and I want you to predict the output*


![carbon (1).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640529256584/7F3ShY49T.png)

Applying our knowledge, we can expect output to be 

1. Start
2. End
3. In setTimeout
4. In fetch

or

1. Start
2. End
3. In fetch
4. In setTimeout

**But which one is right ! .
**
You might consider the first one to be the output as per the flow of the code. *fetch* is called only after *setTimeout*. But it is not true.
The correct output is the later one. That's where the **Microtask Queue** in JavaScript kicks in.

![Screenshot 2021-12-26 201149.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1640529737170/4gAlG2B7v.jpeg)

We know the JavaScript engine contains a call stack where one JavaScript statement is executed at a time, and callbacks are sent to the callback queue for later execution. **But, not all callback code is put in the call back queue. Some callback statements have higher priority than others.** Let's understand what it mean.

The above code snippet contain four statements 

- Two *console.log* statements
- *setTimeout* method that have callback function setTimeoutcallback
- *fetch* method with callback function fetchCallback

The first console.log statement is pushed onto callstack and they get executed, so we get "Start" in console.

The second and third statements, *setTimeout* and *fetch* are the webAPIs and both have a callback function to execute. *Both of them are pushed out of the main flow and control moves to fourth statement.* Again callstack execute this statement, now we have "start" , "End" in our console.

As per the order in code snippet, the JS engine considers *setTimeout* and sets a timer for 5 seconds and after which the callback function is sent into callback queue.

The third statement which is a fetch call, forms the part of webAPIs. We get a [**Promise**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)  after executing the fetch method, depending upon the state of the promise [**.then**](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/then) handlers are executed.
For now, let us consider the Promise to be in fullfilled state. Now we need to take care of the callback function *fetchCallback*. This time JS engine won't push the callback function to callback queue. It push fetchCallback to **Microtask Queue**. The below figure, representing JavaScript runtime environment helps you to understand better.

![JavaScript Engine.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640530689480/SFbOtXSxv.png)

> **Microtask Queue is very similar to Callback Queue, the only difference is that Microtask Queue have higher priority than Callback Queue** 

#### What does "Higher Priority" mean here ?

It means that suppose we have callback functions in Microtask Queue and Callback Queue, then Event loop takes callback functions  from microtask queue and push them to callstack for execution, Callback functions from Callback Queues are taken only after all callback functions in microtask queue is exhausted. 
Event loops is very partial towards Microtask Queue. Even though there is 1 callback in microtask queue and 10 callback in callback queue, Event loop looks for callback functions from microtask queue.

![Microtask1.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640531690440/_69QgGMQP.png)

**But, what kind of callbacks are pushed onto Microtask Queue ?**

Only callbacks from promises and [Mutation Observers](https://developer.mozilla.org/en-US/docs/Web/API/MutationObserver) are sent into *Microtask Queue*.
Mutation observer observers the DOM events and executes callback.
This is one of the important feature in JS, we need to know.

Let's take another example 

![carbon (2).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640529958104/Ro7KVnvTm.png)

Apply the concepts which we learnt so far and predict the output for the above code snippet.
Now, you are aware of Microtask Queue,and *Fetch* callbacks are given higher priority over *setTimeout*.

Hence, the output for the above code snippet looks like this

![Screenshot 2021-12-26 202248.jpg](https://cdn.hashnode.com/res/hashnode/image/upload/v1640530393785/ceCA4XxSWL.jpeg)

I request you to try out these examples, tweak around them by yourself, only then you get a clear idea.

If you have any question, feel free to comment below. I’ll reply each one of you as soon as possible.

Hope you found the article helpful.😊
If yes, consider connecting with me on  [Twitter](https://twitter.com/veereshbv04) , [LinkedIn](https://www.linkedin.com/in/veereshbv04/).
If  No, please leave a feedback below. I would love to read them and improve.

Till next Time, Happy Learning 😀

**Veeresh B V
**




