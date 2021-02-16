# Ray Note

* Ray does not schedule more tasks concurrently than there are CPUs.
  * Ray computes the number of CPUs using `psutil.cpu_count()`
* `ray.init(num_cpus=4, ignore_reinit_error=True)`
  * The argument `ignore_reinit_error=True` just ignores errors if the cell is run multiple times.

### Remote function
* Add the `@ray.remote` decorator to turn a Python function into a remote function
  * When implementing a remote function, the function should expect a regular Python object regardless of whether the caller passes in a regular Python object or an object ID.
  * Use `remote_function.remote()` to call remote function
    * **object IDs** can also be passed into remote functions. When the function is executed, Ray will automatically substitute the underlying Python object that the object ID refers to.
  * `remote_function` immediately returns an object ID (a future) and then creates a task that will be executed on a worker process. 
    * The result can be obtained with `ray.get()`.
      * If we pass in some normal Python objects, the results returned by `ray.get()` should be the same objects.
  * **These task dependencies affect scheduling.** 
    * If the two tasks are scheduled on different machines, the output of the first task  will be copied over the network to the machine where the second task is scheduled.
* Sometimes you need a "worker" process to have "state".
  * remote functions operate on inputs and produce outputs, but they don't change the state of the worker they execute on.

### Actors
* To create an actor, decorate Python class with the `@ray.remote` decorator.
  * Instantiation: `a = Actor.remote(args)` where `a` is called Actor Handles 
    * When an actor is instantiated, a **new worker process** is created by a local scheduler somewhere in the cluster.
    * When we instantiate an actor, a brand new worker is created, and all methods that are called on that actor are executed on the newly created worker.
    * **actors encapsulate state that is shared across actor method invocations.**
  * Method Invocation: `a.method.remote(args)`
    * a remote function (or another actor) can takes an actor handle as an argument
  * Return Values: ray.get(ObjectID)
    * Actor methods are non-blocking. They immediately return an object ID and **they create a task which is scheduled on the actor worker.** 
* with a single actor, no parallelism can be achieved because calls to the actor's methods will be executed one at a time. However, multiple actors can be created and methods can be executed on them in parallel.

### Wait
* After launching a number of tasks, you may want to know which ones have finished executing. This can be done with `ray.wait()`.
  * `ready_ids, remaining_ids = ray.wait(object_ids, num_returns=1, timeout=None)`
    * `ready_ids`: This is a list of object IDs that are available in the object store.
    * `remaining_ids`: This is a list of the IDs that where in `object_ids` but are not in `ready_ids`,
    * the IDs in `ready_ids` and `remaining_ids` together make up all the IDs in `object_ids`.
      * the two lists returned by `ray.wait()` maintains the ordering of the input list.
      * the ObjectIDs of `(ready_list, remain_list)` will be ordered by the argument passed to f above.
    * `object_ids`: This is a list of object IDs.
    * `num_returns`: This is maximum number of object IDs to wait for. The default value is `1`.
    * `timeout`: This is the maximum amount of time in milliseconds to wait for. 
      * `ray.wait()` will block until either `num_returns` objects are ready or until `timeout` milliseconds have passed.
     

