# CoroutinesLearning
This repo is a way I'm learning coroutines

Everything begins from coroutines builders.

**Coroutines builders:**
1. launch {}: Start a coroutine which doesn't have any result. Returns Job.
2. async {}: Returns a single value result using Deferred. It is like Single in Rx as I understand. I've watched Roman Elizarov's video about intro to coroutines. He said that name "Deferred" had taken because it is a synonym of Feature or Promise. That means Deferred contains pending result. It is waiting to be executed. Deferred is also a Job. If you want, you can execute it using .await(). Also, if you want, you can cancel it.
3. runBlocking {}: It starts a coroutine and blocks current thread until coroutines will be executed.

delay() is a special suspending function that doesn't block a thread but block a coroutine.

It's possible to run coroutine inside another coroutine. Hence, they will be linked with relation "Parent-Child"

Every coroutine builder is an extension function for CoroutineScope.
Coroutines inside GlobalScope have a lifetime like the whole app.

What is a CoroutineDispatcher?
It is a traffic controller for coroutines. It can dispatch(send) coroutines to another thread or thread pool.

Dispatchers:
1. Dispatcher.Default: coroutine builders use this dispatcher by default. It uses common pool of shared background threads. Better use for computation. It uses CPU a lot.
2. Dispatcher.IO: uses shared pool of threads which designed for file IO intensive blocking operations or socket IO intensive blocking operations.
3. Dispatcher.Unconfined: this is unrestricted dispatcher. It doesn't have something special as I understand. It shouldn't normally used in code.

launch (Dispatcher.Default) {} uses the same dispatcher as GlobalScope.launch {}.

What is a Continuation?

What is a Job?
It is a thing that user can cancel. It contains a child-parent hierarchy. 

What is a Channel?
It is like Observable. It can produce a stream of data. Deferred emits a single value, Channel a stream.

So what is a Flow? and Sequence???

Flow can be cold. It's like rx...


Nice example of using coroutines.
```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val service = RetrofitFactory.makeRetrofitService()
    CoroutineScope(Dispatchers.IO).launch {
        val response = service.getPosts()
        withContext(Dispatchers.Main) {
            try {
                if (response.isSuccessful) {
                    //Do something with response e.g show to the UI.
                } else {
                    toast("Error: ${response.code()}")
                }
            } catch (e: HttpException) {
                toast("Exception ${e.message}")
            } catch (e: Throwable) {
                toast("Ooops: Something else went wrong")
            }
        }
    }
}
}
```

**How does coroutine work under the hood?**
Android Studio executes code generation to transform code in the coroutine builder to Java class.
All the code inside coroutine builder is separated to parts where Suspended function are the dividers. See example:
```java
class GeneratedCoroutineClass extends Continuation
    int label;
    String url;
    
    void invokeSuspend() {
        switch(label) {
            case 0: {
                url = buildUrl(); //all the code before suspended function
                label = 1;
                download(url, **this**) //suspended function. Here we pass "this" as an Continuation interface
                return;
            }
            case 1: {
                toast("File is downloaded, url=" + url); //all the code after suspended function
                return;
            }
        }
    }
