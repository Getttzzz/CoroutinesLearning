# CoroutinesLearning
This repo is a way I'm learning coroutines

Coroutine is a light weight thread.


Coroutine builders:
1. launch {}: Start a corutine which doesn't have any result. Returns Job.
2. async {}: Returns a single value result using Deferred. It is like Single in Rx as I understand. I've watched Roman Elizarov's video about intro to coroutines. He said that name "Deferred" had taken because it is a synonym of Feature or Promise. That means Deferred contains pending result. It is waiting to be executed. Deferred is also a Job. If you want, you can execute it using .await(). Also, if you want, you can cancel it.
3. runBlocking {}: It starts a coroutine and blocks current thread until coroutines will be executed.

delay() is a special suspending function that doesn't block a thread but block a coroutine.

Every coroutine builder is an extension function for CoroutineScope.
Coroutines inside GlobalScope have a lifetime like the whole app.

What is a CoroutineDispatcher?
It is a traffic controller for coroutines. It can dispatch(send) coroutines to another thread or thread pool.

Dispatchers:
1. Dispatcher.Default: coroutine builders use this dispatcher by default. It uses common pool of shared background threads. Better use for computation. It uses CPU a lot.
2. Dispatcher.IO: uses shared pool of thread that designed for file IO intensive blocking operations or socket IO intensive blocking operations.
3. Dispatcher.Unconfined: this is unrestricted dispatcher. It doesn't have something special as I understand. It shouldn't normally used in code.
