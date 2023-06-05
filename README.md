# Memory Leaks

A memory leak is a situation when the memory that is not required by an application anymore is keep being occupied and not returned to the pool of free memory.

JavaScript is one of the so called ***garbage collected*** languages. Garbage collected languages help developers manage memory by periodically checking which previously allocated pieces of memory can still be "reached" from other parts of the application. These way may reduce the chance of leaking memory. However, only developers can make it clear whether a piece of memory will be required in the future.

Most garbage collectors use an algorithm known as mark-and-sweep. The algorithm consists of the following steps:

   1. The garbage collector builds a list of "roots". Roots usually are global variables to which a reference is kept in code. In JavaScript, the "window" object is an example of a global variable that can act as a root. The window object is always present, so the garbage collector can consider it and all of its children to be always present (i.e. not garbage).

   2. All roots are inspected and marked as active (i.e. not garbage). All children are inspected recursively as well. Everything that can be reached from a root is not considered garbage.

   3. All pieces of memory not marked as active can now be considered garbage. The collector can now free that memory and return it to the OS.

The main cause for leaks in garbage collected languages are unwanted references. In the context of JavaScript, unwanted references are variables kept somewhere in the code that will not be used anymore and point to a piece of memory that could otherwise be freed.

In this example (see [DEMO LINK](https://volodymyrchuyko.github.io/js_memory_leaks/)), a new `p` element is created and appended to the `div` container every second. Though the container's `innerHTML` is cleaned every time before appending new elements, they are not removed from the DOM tree. This causes a memory leak.

