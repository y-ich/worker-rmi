# Worker RMI for JavaScript
This is a synmetric library between DOM JS and Web Workers.

## install
    npm install worker-rmi

Since it is an ES2015 module, currently you need browserify and babelify for Web Workers.

## Usage
There are two main components in worker-rmi.

A class WorkerRMI and a function resigterWorkerRMI.

Supposed that you have a class Foo that can only work well on a server.

In a client side, you make a wrapper class based on WorkerRMI as the same name.

    import { WorkerRMI } from './worker-rmi.js';

    class Foo extends WorkerRMI {
        async aMethod(...inputs) {
            return await this.invokeRM('aMethod', inputs);
        }
    }

Methods may also be named same but it is not mandatory. The method invokeRM is a method prepared by WorkerRMI, that request an execution of the first argument.

In a server side, you already have the class Foo, so call resigterWorkerRMI as follows.
(Assuming that the server side is worker side now.)

    resigterWorkerRMI(self, Foo);

That's all and should work.

One important note is that all Transferable ojects in arguments and a return value transfer in transferList. So you cannot access these objects after invoking the remote method or returning a result respectively.

If you want that client side is a worker and server side is a DOM JS, no problems. The client side is same, and in the server side, the code looks like follows.

    const worker = new Worker('js/worker.js');
    resigterWorkerRMI(worker, Foo);

The only difference is, specifying a Woker instance as the first argument rather than self in the case of Worker.

Hope that this tiny library will helpful for you.
