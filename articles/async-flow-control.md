    Title: Async flow control
    Date: 24 Jun 2012
    Status: public
    Tags: nodejs, javascript, async

Some sync-programmers a.k.a. node haters loves to repeat as mantra: "async
programming is poor because nested callbacks looks like a crap". That's true,
sync-programmer produces crappy async code because the sync way of thinking is
not acceptable in async programming.

Forget about throwing errors and returning result from functions. Just need to
start thinking differently and not write nested-callbacks-mess. There are only
few techniques in async flow control need to use and understand. No additional
flow control libraries as dependencies needed, because it's too simple to be
dependency. No fibers and sync-style precompilers. Only pair of techniques which
can be implemented in few lines of code: async map and async queue.

## Async queue

In sync programming, for example in PHP the way of interacting with IO usually
looks as:

    $data = readFile('filename');
    $result = do_something_with_data($data);
    writeFile($result, 'another-filename');
    // do something else

When sync-programmer tries to implement it in node the result looks like:

    readFile('filename', function (err, data) {
        if (!err) {
            doSomethingWithData(data, function (err, result) {
                if (!err) {
                    writeFile(result, 'another-filename', function () {
                        // do something else
                    });
                }
            });
        }
    });

Looks like a nightmare, right? Let's start article "Nodejs is wrong"? Nope.
Let's start to use node correctly:

    asyncQueue(
        readInput,
        doSomethingWithData,
        writeOutput,
        doSomethingElse
    );

    function readInput(done) {
        readFile('filename', done);
    }

    function writeOutput(result, done) {
        writeFile(result, 'another-filename', done);
    }

This is how it **should** look like. Looks better then php, right? And it's much
better because it's truly async, and even more readable.

`asyncQueue` is actor method which arranges async flow control. It calls his
actions one after another. Each action accepts callback as last param - `done`
callback, which should be called when async action is completed. There's a
simple convention of callbacks: **each callback accepts two arguments: error and
result**

I strongly recommend to write your own implementation of `asyncQueue` actor
method, depending on your needs. For my needs sometimes I use `asyncQueue` method
which pass all parameters except first (error) to the next method, and only call
next method when error param is null, when it's not null - call last method in
queue with proper error. This is one of strategies for passing params between
async actions. It's good enough in simple cases. In most advanced cases I prefer
to use another strategy: actor object.

Actor object is an instance of actor class, which have number of actions and
methods for flow control: map, queue. Data exchange between actions works
through the object state change. Example

    var webCrawlerActor = {
        loadIndexPage: function () {},
        loadTagsPage: function () {},
        grabDataFromIndex: function () {},
        grabDataFromTags: function () {},
        asyncQueue: ...,
        asyncMal: ...
    };

    webCrawlerActor.asyncQueue('loadIndexPage', 'grabDataFromIndex');
    webCrawlerActor.asyncQueue('loadTagsPage', 'grabDataFromTags');

Each of `grabDataFrom*` method uses data from `load*Page` method which passed by
actor state changing. Of course it's not how it should be implemented in real,
just an example to get idea.

## Async map

Async map is another flow control method which performs number of actions
simultaneously and wait for each async action to be done. Few usecases: do
something async for each of array elements, do some different async jobs and
wait for all of them to be done. It's almost the same as queue, just executed at
the same time.

The most common pattern looks like:

    function asyncMap(els, action, done) {
        var wait = els.length;
        els.forEach(function (el) {
            action(el, actionDone);
        });
        var errors = [];
        var results = [];
        function actionDone(err, result) {
            if (err) {
                errors.push(err);
            } else if (result) {
                results.push(result);
            }
            if (--wait === 0) {
                done(errors, results);
            }
        }
    }

## Why async is better

I like async programming because it requires thinking in terms that may look
excessive from the sync-programming point of view, but they are always necessary
in fact. It's harder than traditional sync style, but it's harder in right
place: in place where you should stop coding and start thinking. I came to
nodejs from php and ruby where you code as fast as you think, and it's ok to
produce tons of code that mix actors, controllers, actions in it, everything in
single 1000-lines method. You able to do this crap in php and ruby, but you
never will write this code in nodejs.

Yeah, I know, that method with more than 30 lines is a reason for thinking in
any programming language. But async programming gives you obvious and right
place for splitting your methods. In sync programming, especially for newcomers,
non-return point is not so obvious as in async.

This coding style is side-effect of async programming that we always need to
keep in mind, even when writing code in sync languages.
