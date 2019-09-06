# CoroutinesLearning
This repo is a way I'm learning coroutines

Coroutine is a light weight thread.


Coroutine builders:
1. launch {}: Start a corutine which doesn't have any result. Returns Job.
2. async {}: Returns a single value result using Deferred. It is like Single in Rx as I understand. I've watched Roman Elizarov's video about intro to coroutines. He said that name "Deferred" had taken because it is a synonym of Feature or Promise. That means Deferred contains pending result. It is waiting to be executed. Deferred is also a Job. If you want, you can execute it using .await(). Also, if you want, you can cancel it.
3. runBlocking {}: It starts a coroutine and blocks current thread until coroutines will be executed.

delay() is a special suspending function that doesn't block a thread but block a coroutine.
