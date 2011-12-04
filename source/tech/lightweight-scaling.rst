===================
Lightweight Scaling
===================

**Synopsis**:
An overview of Coroutines_ and `Non-blocking I/O`_/`Asynchronous Socket Programming`_, all related to the `C10K problem`_, focusing on the Python_ ecosystem, mainly on Linux_.


Standing on the shoulders of giants
===================================

gevent
------

Introduction
~~~~~~~~~~~~

gevent_ is a coroutine_-based Python_ networking library that uses greenlet_ to provide a high-level synchronous API on top of the libevent_ `event loop`_. The "greenlet_" package is a spin-off of Stackless_, a version of CPython_ that supports microthreads_ called "tasklets_". libevent_ is based on the epoll_/Kqueue_ kernel interface.


Description
~~~~~~~~~~~

The description of Stackless_ gets to the point:

Stackless_ Python is an enhanced version of the Python programming language. It allows programmers to reap the benefits of thread-based_ programming without the performance and complexity problems associated with conventional threads_. The microthreads_ that Stackless_ adds to Python are a cheap and lightweight convenience which can if used properly, give the following benefits:

- Improved program structure.
- More readable code.
- Increased programmer productivity.


Estimation
~~~~~~~~~~

- EVE Online - "a massive multiplayer online roleplaying space game" uses Stackless_, see `Stackless Python In Eve`_.
- Second Life uses Eventlet_, see `eventlet@secondlife`_.
- Gevent_ is used at Omegle_, Spotify_ and sitesupport_.

+1 for gevent_ based on numbers from `Benchmark of Python WSGI Servers`_ by `Nicholas Piël`_ [sitesupport_], the maximum comfort by monkeypatching_ the Python Standard Library as well as other rumors.



The giants
==========


The Problems
------------

with Threads
~~~~~~~~~~~~

.. note:: copied verbatim from: `The Problem with Threads`_, `Edward A. Lee`_, 2006.

Threads are a seemingly straightforward adaptation of the dominant sequential model of computation to concurrent systems. Languages require little or no syntactic changes to support threads, and operating systems and architectures have evolved to efficiently support them. Many technologists are pushing for increased use of multithreading in software in order to take advantage of the predicted increases in parallelism in computer architectures.

In this paper, I argue that this is not a good idea. Although threads seem to be a small step from sequential computation, in fact, they represent a huge step. They discard the most essential and appealing properties of sequential computation: understandability, predictability, and determinism. Threads, as a model of computation, are wildly nondeterministic, and the job of the programmer becomes one of pruning that nondeterminism. Although many research techniques improve the model by offering more effective pruning, I argue that this is approaching the problem backwards.

Rather than pruning nondeterminism, we should build from essentially deterministic, composable components. Nondeterminism should be explicitly and judiciously introduced where needed, rather than removed where not needed. The consequences of this principle are profound. I argue for the development of concurrent coordination languages based on sound, composable formalisms. I believe that such languages will yield much more reliable, and more concurrent programs.

[...]

"We have argued that threads provide a hopelessly unusable extension of the core abstractions of computation. Yet in practice, many programmers today write multi-threaded programs that work." ;]

with Callbacks
~~~~~~~~~~~~~~

.. note:: copied verbatim from: `Weightless background`_, `Erik J. Groeneveld`_, 2008.

The problems with call-backs are well-known I suppose. In short, call-backs obfuscate the flow of a program which makes it very hard to write correct code. Also, the code is often hard to maintain and extend. This is the very reason why threads are often used as a alternative: they linearize the code flow again, but also bring their own problems. Call-backs and threads are two different ways to deal with potentially blocking I/O operations, each with their own problems. 


The Fuzz: Continuations, Coroutines and Generators
--------------------------------------------------

Continuations
~~~~~~~~~~~~~

.. todo:: write about callcc et al.
.. todo:: Task (C#), goroutine (Go), callcc (Ruby, Lisp), actor (Erlang, Scala)


Coroutines
~~~~~~~~~~

.. note:: copied verbatim from: `Coroutines In Python`_, `Sam Rushing`_, 2000.

A coroutine_ is a generalization of the subroutine. Forget for a moment everything you've been taught about calling functions, and stacks, etc... Think back to BASIC, and the evil GOTO statement. Imagine a more powerful GOTO that could jump back and forth between functions, passing arguments, and you begin to get the idea

Coroutines_ are a bit of ancient forgotten computer-science lore; stamped out of the collective memory by the hegemony of C. But they are useful in a wide variety of situations that can only be clumsily solved using 'standard' tools like threads_ and processes.

Coroutines_ can be used to simplify just about any difficult state-machine programming problem. Any time you find yourself tempted to build a complex state machine to solve what should be a simple problem, you've actually been pining for coroutines_. With them, you can usually turn an 'inside-out' problem into a 'right-side-out' problem, and replace a few pages of hairy code with a single function.

A popular application of coroutines is in the construction of `lightweight threading`_ libraries (in fact, on Win32 coroutines_ are called 'fibers_'). Coroutine_ threads are more scalable than OS threads_ - a desktop machine can easily juggle tens of thousands of them without gobbling up the entire virtual memory space.

Efficient coroutines_ hold out the possibility of building powerful, stateful internet servers that are massively scalable.


.. note:: copied verbatim from: Coroutines_, Wikipedia.

Coroutines_ are useful to implement the following:

- State machines within a single subroutine, where the state is determined by the current entry/exit point of the procedure; this can result in more readable code.
- Actor model of concurrency, for instance in computer games. Each actor has its own procedures (this again logically separates the code), but they voluntarily give up control to central scheduler, which executes them sequentially (this is a form of `cooperative multitasking`_).
- Generators_, and these are useful for input/output and for generic traversal of data structures.



Generators
~~~~~~~~~~

.. note:: copied verbatim from: `JavaScript\: Iterators and generators`_, Mozilla Developer Network, 2007.

Generators_ provide a powerful alternative to custom iterators: they allow you to define an iterative algorithm by writing a single function which can maintain its own state.

A generator_ is a special type of function that works as a factory for iterators. A function becomes a generator_ if it contains one or more yield expressions.



Throw Non-Blocking I/O into the mix
-----------------------------------

.. note:: copied verbatim from: asyncore_, `Sam Rushing`_, 1996.

There are only two ways to have a program on a single processor do “more than one thing at a time.” Multi-threaded_ programming is the simplest and most popular way to do it, but there is another very different technique, that lets you have nearly all the advantages of multi-threading_, without actually using multiple threads_. It’s really only practical if your program is largely I/O bound. If your program is processor bound, then `preemptive`_ scheduled threads_ are probably what you really need. Network servers are rarely processor bound, however.

If your operating system supports the select() system call in its I/O library (and nearly all do), then you can use it to juggle multiple communication channels at once; doing other work while your I/O is taking place in the “background.” Although this strategy can seem strange and complex, especially at first, it is in many ways easier to understand and control than multi-threaded_ programming.


Throw a scheduler into the mix
------------------------------
Your asynchronous programming library/framework will bring an `event loop`_,
which does the scheduling_ of the coroutines_. See gevent.core_, `gevent event loop`_, tornado.ioloop_ and others.

.. todo:: enhance!

In the meanwhile, some enhancements since Python 2.5 make generators_ usable as simple coroutines_:

`PEP 342 -- Coroutines via Enhanced Generators`_ shows at Example 3.:

    A simple co-routine scheduler or "trampoline" that lets
    coroutines "call" other coroutines by yielding the coroutine
    they wish to invoke.




History, facts and credits
--------------------------

.. note:: Be welcome to fill the gaps and correct the mistakes! ;]


Python articles and PEPs
~~~~~~~~~~~~~~~~~~~~~~~~

.. csv-table::
    :header: "Date", "Title", "About", "Author"
    :widths: 5, 15, 20, 10

    1999, `Continuations and Stackless Python`_, "\- or 'How to change a Paradigm of an existing Program' ... introduces lightweight, `cooperative multitasking`_ aka. coroutines_/fibers_ into Python.`", "`Christian Tismer`_"
    2000, `Stackless Python and Korea`_, "Continuations and Stackless_ Python - Where Do You Want To Jump Today?", `Christian Tismer`_
    2000, `Charming Python\: Inside Pythons implementations`_, "Interviews with the creators of Vyper and Stackless_ Python.", `David Mertz`_
    2000, `Coroutines In Python`_, "A coroutine_ is a generalization of the subroutine. [...]", `Sam Rushing`_
    2000, `Introduction to Stackless Python`_, "Stackless_ Python is an alternative implementation of Python created by independent developer Christian Tismer. He started with the conventional Python language processor managed by the language's inventor, `Guido van Rossum`_, and patched his own Stackless_ invention in place of a small but central part of Python's internals. Stackless Python_ is the result. This article introduces Tismer's technology and its significance. In future articles, you'll be able to read about how to make your own start at programming Stackless_ Python, as well as the prospects for a merger between Stackless_ and the main Python distribution.", `Cameron Laird`_
    2000, `Programming Stackless Python`_, "You can import ``continuation_`` and do other Stackless_ Python-based programming yourself with only a bit of effort. This article explains how to get started: where to find the files you'll need, how to install them, and how to verify that your installation is working properly.", `Cameron Laird`_
    2001, `PEP 255 -- Simple Generators`_, "This PEP introduces the concept of generators_ to Python, as well
    as a new statement used in conjunction with them, the 'yield' statement.", "Neil Schemenauer, Tim Peters, Magnus Lie Hetland"
    2001, `Charming Python\: Iterators and simple generators`_, "Python 2.2 introduces a new construct accompanied by a new keyword. The construct is generators_; the keyword is yield. Generators_ make possible several new, powerful, and expressive programming idioms, but are also a little bit hard to get one's mind around at first glance.", `David Mertz`_
    2002, `Charming Python\: Implementing "weightless threads" with Python generators`_, "The power of microthreads_. In a related `Charming Python\: Iterators and simple generators`_ installment, David introduces a way of simulating full-fledged coroutines_ with generators_ and a simple scheduler_. It is possible to extend this scheduler_ in straightforward ways to allow extremely `lightweight threading`_ of multiple processes. Much as with Stackless Python microthreads_, pseudo-coroutine '`weightless threads`_' require almost none of the context switch and memory overhead of OS -- or even userland -- threads. Here David introduces `weightless threads`_ as an elegant solution for problems whose natural solutions involve large numbers of cooperating processes.", `David Mertz`_
    2005, `PEP 342 -- Coroutines via Enhanced Generators`_, "This PEP proposes some enhancements to the API and syntax of
    generators, to make them usable as simple coroutines.
    It is basically a combination of ideas from the PEPs `PEP 288 -- Generators Attributes and Exceptions`_ (`Raymond Hettinger`_)
    and `PEP 325 -- Resource-Release Support for Generators`_ (`Samuele Pedroni`_), which
    may be considered redundant if this PEP is accepted.", "`Guido van Rossum`_, `Phillip J. Eby`_"
    2005, `Unsung Heroes of Python\: asynchat/asyncore`_, "", Tim Lesher
    2007, `PEP\: XXX - Standard Microthreading Pattern`_, "See uthreads_, a microthreading_ library layered on top of Twisted_, esp. `StandardMicrothreadingPattern - Description of a proposed standard microthreading programming pattern`_", `Dustin J. Mitchell`_
    2009, `PEP 380 -- Syntax for Delegating to a Subgenerator`_, "A syntax is proposed for a generator to delegate part of its
    operations to another generator. This allows a section of code
    containing 'yield' to be factored out and placed in another
    generator. Additionally, the subgenerator is allowed to return with a
    value, and the value is made available to the delegating generator.", `Gregory Ewing`_
    2009, `PEP\: XXX - Cofunctions`_, "A syntax is proposed for defining and calling a special type of generator
    called a 'cofunction'.  It is designed to provide a streamlined way of
    writing generator-based coroutines, and allow the early detection of
    certain kinds of error that are easily made when writing such code, which
    otherwise tend to cause hard-to-diagnose symptoms.", `Gregory Ewing`_
    2009, `Asynchronous Servers in Python`_, "A look at a selection of asynchronous servers implemented in Python together with a Ping Pong benchmark, which measures the raw socket performance.", `Nicholas Piël`_  [sitesupport_]
    2009, `Experimental HTTP server using Stackless Python`_, "This blog post documents my experiment to write a non-blocking_ HTTP server based on coroutines_ (tasklets_) of Stackless_ Python. My goal was to write a minimalistic web server server which can handle cuncurrent requests by using non-blocking_ system calls, multiplexing with select(2) or epoll_(2), returning a simple 'Hello, World' page for each request, using the coroutines_ of Stackless_ Python. I've done this, and measured its speed using ApacheBench_, and compared it to the 'Hello, World' server of Node.js_. Note: This eventually became Syncless_.", `Péter Szabó`_
    2010, `Feature comparison of Python non-blocking I/O libraries`_, "This blog post is a tabular feature comparison of Syncless_ and the 6 most popular event-driven_ and coroutine-based_ non-blocking_ (asynchronous) networking I/O libraries for Python. It was inspired by `Asynchronous Servers in Python`_ (published on 2009-11-22), which compares the features and the performance of 14 Python non-blocking_ networking I/O libraries. We're not comparing generator_-based (yield) solutions here. We haven't made performance measurements, so speed-related claims in this comparison are beliefs and opinions rather than well-founded facts.", `Péter Szabó`_
    2010, `Benchmark of Python WSGI Servers`_, "A look at how different WSGI servers perform at the handling of a full HTTP request.", `Nicholas Piël`_  [sitesupport_]
    "2011", `Emulating Stackless and greenlet with each other`_ [EuroPython2011_], "Stackless Python and the greenlet package for CPython are two different implementations of coroutine support for Python. (Coroutines are fundamental building blocks of I/O frameworks like gevent, Eventlet, Concurrence and Syncless to conveniently handle thousands of socket connections at a time without creating threads.) Stackless and greenlet implement a different interface. However, each is powerful enough so that it can be used to emulate the other one. In this talk we explore the differences and discuss design decisions and testing strategies of the emulations we have implemented.", `Péter Szabó`_
    "2011", `Beyond Python Enhanced Generators`_ [EuroPython2011_], "Right after the introduction of PEP342 (Enhanced Generators) we started to decompose programs into generators. It was soon discovered that for real-life problems one would need something like 'yield from', as is described in PEP380. At that time, we already had a similar solution called 'compose', which we adapted to PEP380. (http://weightless.io/compose)

    After 5 years working with 'compose', we found a small set of other features that are essential if you want to use Enhanced Generators not only as a way of lightweight command scheduling, but also a a pipe-line, or parser. Indeed, the latter concepts are what real co-routines are about.

    This talk introduces what is needed on top of PEPs 342 and 380 based on experience with decomposing big enterprise search engines into co-routines. Parts of it have been presented on SPA (2008) and EuroPython (2010). Understanding of Enhanced Generators is a prerequisite.", `Erik J. Groeneveld`_



Python implementations
~~~~~~~~~~~~~~~~~~~~~~

.. csv-table::
    :header: "Date", "Title", "About", "Author"
    :widths: 5, 5, 25, 10

    1996, asyncore_, "asyncore — Asynchronous socket handler. Basic infrastructure for asynchronous socket service clients and servers.", `Sam Rushing`_
    1999, Medusa_, "Medusa is an architecture for very-high-performance TCP/IP servers (like HTTP, FTP, and NNTP). Medusa is different from most other servers because it runs as a single process, multiplexing I/O with its various client and server connections within a single process/thread_.", "`Sam Rushing`_, `Andrew Kuchling`_"
    1999, "| Stackless_, 
    | `Stackless - old page`_", "The Stackless approach » `Continuations and Stackless Python`_ - or 'How to change a Paradigm of an existing Program' « introduces lightweight, `cooperative multitasking`_ aka. coroutines_/fibers_ into Python. (since Python 1.5.2)`", "`Christian Tismer`_"
    2001, Twisted_, "An event-driven networking engine, around for years, a large framework. It uses deferreds_ (an abstraction over callback parameters) aka. futures_. The programming model is mostly callback-based, but there is `Twisted DeferredGenerator`_.", `Glyph Lefkowitz`_
    2005, peak.events_, "Provides an event-driven_ programming framework that supports ultralight microthreads_ implemented via generators_. It can stand alone or can be used atop Twisted_ for a more intuitive approach to asynchronous programming. Can write event-driven code in a more natural, sequential, untwisted style, but without giving up access to Twisted_ 's many great features.", PEAK_ Community
    2006, Eventlet_, "A concurrent networking library. Combines epoll_/Kqueue_ with coroutines_. 2007 open sourced by Linden Labs. Programming model: `lightweight threads`_, making your code feel synchronous.", `Bob Ippolito`_
    2006, Kamaelia_, "Kamaelia is a Python library by BBC Research for concurrent programming using a simple pattern of components that send and receive data from each other. This speeds development, massively aids maintenance and also means you build naturally concurrent software.", 
    2007, peak.events.trellis_, "Event-Driven Programming The Easy Way. What Trellis is *for* is relatively easy to explain: it's for 
    safely and transparently updating things in response to changes.  An 
    event-driven system that truly doesn't suck, it's like a spreadsheet 
    for code. Read more: [1]_", `Phillip J. Eby`_
    2007, Chiral_, "Chiral is a lightweight coroutine-based_ networking framework for high-performance internet and Web services. Coroutines_ in Chiral are based on Python 2.5's generators, as specified in PEP 342.
    The Coroutine_ class wraps around a generator_ and handles scheduling_.", Jacob Potter
    2007, Tornado_, "Scalable, non-blocking_ web server. Proven on-site technology. Tornado's I/O-loop (tornado.ioloop_) is based on epoll_/Kqueue_. 2009 open sourced by Facebook. Programming model: callback-based. See also: `The technology behind Tornado, FriendFeed's web server`_, `Tornado: Facebook's Real-Time Web Framework - With Paul Buchheit`_.", "`Bret Taylor`_, `Paul Buchheit`_ [FriendFeed_]"
    2008, circuits_, "Lightweight Event driven and Asynchronous Application Framework for the Python Programming Language with a strong Component Architecture.", `James Mills`_
    2008, Weightless_, "Weightless supports implementing complete Python programs as coroutines_, including protocol stacks, such as the HTTP protocol. Weightless_ consists of three major parts:
        - compose: program decomposition with coroutines (à la `PEP 380 -- Syntax for Delegating to a Subgenerator`_).
        - observable: component configuration with the observer pattern (DNA)
        - gio: connecting file descriptors to a coroutine

    In use at Meresco_. See also: `Weightless background`_", `Erik J. Groeneveld`_
    2008, cogen_, "Crossplatform asynchronous network oriented python framework based on python 2.5 enhanced generators. cogen_ is a crossplatform library for network oriented, coroutine_ based programming using the `enhanced generators`_ from python 2.5. The project aims to provide a simple straightforward programming model similar to threads_ but without all the problems and costs. cogen's goal is to enable writing code in a seemingly synchronous and easy manner in the form of generators_ that yield calls and receive the result from that yield. These calls translate to asynchronous and fast os calls in cogen's internals. Includes an enhanced WSGI server. See also: `cogen and greenlets`_", `Maries Ionel Cristian`_
    2009, python-multitask_, "`Cooperative multitasking`_ and `asynchronous I/O`_ using Python generators_. python-multitask_ allows Python programs to use generators_ (a.k.a. coroutines_) to perform `cooperative multitasking`_ and `asynchronous I/O`_. Applications written using multitask consist of a set of cooperating tasks that yield to a shared task manager whenever they perform a (potentially) blocking operation, such as I/O on a socket or getting data from a queue. The task manager (scheduler_) temporarily suspends the task (allowing other tasks to run in the meantime) and then restarts it when the blocking operation is complete. Such an approach is suitable for applications that would otherwise have to use ``select()`` and/or multiple threads_ to achieve concurrency.", `Christopher Stawarz`_
    2009, gevent_, "Combines libevent_ with coroutines_ (gevent.core_, `gevent event loop`_), but does it right. See `Comparing gevent to eventlet`_. Programming model: `lightweight threads`_, making your code feel synchronous.", `Denis Bilenko`_ [sitesupport_]
    2009, Concurrence_, "Concurrence_ is a framework for creating massively concurrent network applications in Python. It takes a Lightweight-tasks-with-message-passing approach to concurrency. The goal of Concurrence is to provide an easier programming model for writing high performance network applications than existing solutions (Multi-threading_, Twisted_, asyncore_ etc). Concurrence_ uses `Lightweight tasks`_ in combination with libevent_ to expose a high-level synchronous API to low-level `asynchronous IO`_. Fast low-level IO buffers implemented in Pyrex_. DBAPI 2.0 compatible MySQL driver implementation (native & asynchronous, with optimized protocol support written in Pyrex_). Upcoming: Improved Memcache support (Ketama hashing, Connection Management).", Henk Punt
    2009, Syncless_, "Syncless is a non-blocking_ (asynchronous) concurrent client and server socket network communication library for Stackless_ Python 2.6 (and also for regular Python_ with greenlet_). For high speed, Syncless_ uses libev_ (and libevent_) for event notification, and parts of Syncless_ ' code is implemented in Pyrex_/Cython_ and C_. This alone makes Syncless_ faster than many other non-blocking_ network libraries for Python_. Syncless_ contains an asynchronous DNS resolver (using evdns_) and a HTTP server capable of serving WSGI_ applications. Syncless_ aims to be a coroutine_-based alternative of event-driven networking engines (such as Twisted_, asyncore_, pyevent_, python-libevent_ and FriendFeed's Tornado_), and it's a competitor of gevent_, Eventlet_ and Concurrence_. See also: `Experimental HTTP server using Stackless Python`_", `Péter Szabó`_
    2010, monocle_, "monocle_ - An async programming framework with a blocking look-alike syntax. monocle_ straightens out event-driven_ code using Python's generators_. It aims to be portable between event-driven_ I/O frameworks, and currently supports Twisted_ and Tornado_. It's for Python 2.5 and up.", Greg Hazel and `Steven Hazel`_.


Other implementations/resources
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. todo:: write about .NET Fibers, Scala Actors, et al.

.. csv-table::
    :header: "Date", "Title", "About", "Author"
    :widths: 5, 10, 25, 10

    1977, Icon_, "`Icon generators`_ are a key concept in Icon. Generators_ drive much of the loop functionality in the language, but do so more directly; the programmer does not write a loop and then pull out and compare values, Icon will do all of this for you.", ""
    1993, ACE_, "The ADAPTIVE Communication Environment (ACE) is a freely available, open-source object-oriented (OO) framework that implements many core patterns for concurrent communication software. Esp. see: `ACE Reactor\: event demultiplexing and event handler dispatching`_", ""
    1996, `POE\: Perl Object Environment`_, "POE is a Perl framework for `reactive systems`_, `cooperative multitasking`_, and network applications. It supports 10 different `event loops`_. See also: `POE Whitepaper`_", `Rocco Caputo`_
    1998, JAWS_, "The JAWS Adaptive Web Server - An Application Framework for High Performance Web Systems. Based on ACE_.", ""
    1999, The `C10K problem`_, "`Interesting scalable server implementations`_", `Dan Kegel`_
    2001, `epoll - I/O event notification facility`_, "TODO", `Davide Libenzi`_
    2001, `Coroutines for Ruby`_, "Coroutines allow blocks to run concurrently while you control when the context is switched between them.", `Marc De Scheemaecker`_
    2002, `Linux File AIO`_, "Kernel Asynchronous I/O (AIO) Support for Linux_. AIO enables even a single application thread_ to overlap I/O operations with other processing, by providing an interface for submitting one or more I/O requests in one system call (``io_submit()``) without waiting for completion, and a separate interface (``io_getevents()``) to reap completed I/O operations associated with a given completion group.
    
    The AIO implementation presented here should have similar performance characteristics to the event interface that /dev/epoll uses, as both models have a 1-1 correlation between events being generated and the potential for progress to be made.
    
    One area where AIO poll differs significantly from /dev/epoll stems from readiness vs ready state notification: an async poll is like poll in that the operation completes when the descriptor has one of the specified events pending. However, /dev/epoll only generates an event when the state of the monitored events changes.
    
    See also: `An AIO Implementation and its Behaviour`_ and `Linux Asynchronous I/O Design`_
    ", Benjamin C. R. LaHaise`
    2002, nginx_, "Nginx is a free, open-source, high-performance HTTP server and reverse proxy, as well as an IMAP/POP3 proxy server.
    
    Nginx is known for its high performance, stability, rich feature set, simple configuration, and low resource consumption.

    Nginx is one of a handful of servers written to address the `C10K problem`_. Unlike traditional servers, Nginx doesn't rely on threads_ to handle requests. Instead it uses a much more scalable event-driven_ (asynchronous) architecture. This architecture uses small, but more importantly, predictable amounts of memory under load.
    Even if you don't expect to handle thousands of simultaneous requests, you can still benefit from Nginx's high-performance and small memory footprint. Nginx scales in all directions: from the smallest VPS all the way up to clusters of servers.
    
    Architecture and scalability:
        - One master process and several workers processes
        - The notification methods: kqueue_ (FreeBSD 4.1+), epoll_ (Linux 2.6+), rt signals (Linux 2.2.19+), `/dev/poll`_ (Solaris 7 11/99+), `event ports`_ (Solaris 10), select_, and poll_
        - sendfile_
        - `File AIO`_
        - 10,000 inactive HTTP keep-alive connections take about 2.5M memory
    
    See also: `nginx\: Architecture and scalability`_", `Igor Sysoev`_
    2003, `Java NIO\: New I/O APIs`_, "The new I/O (NIO) APIs introduced in Java v1.4 provide new features and improved performance in the areas of buffer management, scalable network and file I/O, character-set support, and regular-expression matching. Esp. A multiplexed, non-blocking I/O facility for writing scalable servers.", Sun Microsystems
    2004, `Apache Event MPM`_, "Event-based multi-processing module for the `Apache HTTP Server`_ based on epoll_/Kqueue_.", ""
    2004, `Apache MINA`_, "A Multi-purpose Infrastructure for Network Applications. A network application framework which helps users develop high performance and high scalability network applications easily. It provides an abstract event-driven asynchronous API over various transports such as TCP/IP and UDP/IP via Java NIO. See also: `Introduction to MINA`_.", `Trustin Lee`_
    2005, Jetty_, "Jetty 6 brings `Jetty\: Asynchronous Servlets and Continuations`_", ""
    2006, `Java 6\: Enhancements in Java I/O`_, "A new java.nio.channels.SelectorProvider implementation that is based on the Linux epoll event notification facility is included. The epoll facility is available in the Linux 2.6, and newer, kernels. The new epoll-based SelectorProvider implementation is more scalable than the traditional poll-based SelectorProvider implementation when there are thousands of SelectableChannels registered with a Selector. The new SelectorProvider implementation will be used by default when the 2.6 kernel is detected. The poll-based SelectorProvider will be used when a pre-2.6 kernel is detected. See also: `To poll or epoll\: that is the question`_.", Sun Microsystems
    2006, Boost.Coroutine_, "The Boost.Coroutine library contains a family of class templates that wrap function objects in coroutines_. Coroutines_ are a generalization of subroutines that can return and be reentered more than once without causing the destruction of automatic objects. Coroutines_ are useful whenever it is necessary to keep state across a function call, a job usually reserved to stateful function objects.", Giovanni P. Deretta
    2007, `JSR 315\: Java™ Servlet 3.0 Specification`_, "One of the significant enhancements made in 'JSR 315: Java Servlet 3.0', is support for asynchronous processing, see `Asynchronous Support in Servlet 3.0`_.", ""
    2007, Grizzly_, "Writing scalable server applications in the Java™ programming language has always been difficult. Before the advent of the Java New I/O API (NIO), thread management issues made it impossible for a server to scale to thousands of users. The Grizzly NIO and Web framework has been designed to help developers to take advantage of the Java™ NIO API. Grizzly's goal is to help developers to build scalable and robust servers using NIO and we are also offering extended framework components: Web Framework (HTTP/S), Bayeux Protocol, Servlet, HttpService OSGi and Comet.", `Jeanfrancois Arcand`_
    2007, `JavaScript\: Iterators and generators`_, "Iterators and Generators, introduced in JavaScript 1.7, bring the concept of iteration directly into the core language. While custom iterators are a useful tool, their creation requires careful programming due to the need to explicitly maintain their internal state. Generators provide a powerful alternative: they allow you to define an iterative algorithm by writing a single function which can maintain its own state. A generator is a special type of function that works as a factory for iterators. A function becomes a generator if it contains one or more yield expressions.", ""
    2008, Atmosphere_, "Atmosphere is a POJO based framework using Inversion of Control (IoC) to bring push/Comet and Websocket to the masses! The Atmosphere Framework is designed to make it easier to build asynchronous/Comet‐based Web applications that include a mix of Comet and RESTful behavior. The Atmosphere Framework is portable and can be deployed on any Web Server that supports the Servlet Specification 2.3.", `Jeanfrancois Arcand`_
    2008, Netty_, "The Netty project is an effort to provide an asynchronous event-driven network application framework and tools for rapid development of maintainable high performance & high scalability protocol servers & clients.", "`Trustin Lee`_, JBoss Inc."
    2009, Reflex_, "Reflex is a class library for writing reactive_ Perl programs. It provides base classes for reactive_ objects, and specific subclasses for various tasks.", `Rocco Caputo`_
    2009, Node.js_, "Evented I/O for `V8 JavaScript`_. Node.js is a server-side JavaScript environment that uses an asynchronous event-driven model. This allows Node.js to get excellent performance based on the architectures of many Internet applications.", "`Ryan Dahl`_, `Isaac Schlueter`_"
    2010, Gretty_, "Gretty is simple framework for networking, both building web servers and clients. Built on top of netty, it supports NIO style http server, asynchronous http client. It also supports both websocket server and client. See also: `512000 concurrent websockets with Groovy++ and Gretty`_.", Alex Tkachman
    2010, async-http-client_, "Asynchronous Http Client library for Java. The library uses Java non blocking I/O for supporting asynchronous operations. The default asynchronous provider is build on top of Netty_, the Java NIO Client Server Socket Framework from JBoss, but the library exposes a configurable provider SPI which allows to easily plug in other frameworks.", "`Jeanfrancois Arcand`_, Ning Inc."


Details and outlook
-------------------

- Greenlets_ (used by gevent_) are provided as a C extension module for the regular unmodified interpreter, while Stackless_ is a patch into the regular Python. On the other hand, Stackless_ could improve efficiency by about 10% by using soft switching instead of hard switching (this statement was made by `Christian Tismer`_, the inventor of Stackless_). 
- The upcoming gevent 1.0 will be based on libev_, see `libev and libevent`_.
- On EuroPython2011_ it was announced by some authors of PyPy_, that Tasklets_ and Greenlets_ support for PyPy_ would require some weeks of effort.
- WSGI1_ was designed for synchronous operation only. WSGI2_/Web3_ addresses that. See also:
    - `WSGI for Python 3`_, `PEP 3333 -- Python Web Server Gateway Interface v1.0.1`_
    - `Project area for Collaborating on the WSGI 2 spec`_
    - `Project area for Collaborating on Web3 spec`_
    - AsyncWSGISketch_ from PEAK_


More
====

Architectures
-------------

- `EDA\: Event-driven architecture`_
- | `SEDA\: Staged event-driven architecture`_
  | `SEDA\: An Architecture for Highly Concurrent Server Applications`_
  | "SEDA is an acronym for staged event-driven architecture, and decomposes a complex, event-driven application into a set of stages connected by queues. This design avoids the high overhead associated with thread-based concurrency models, and decouples event and thread scheduling from application logic. By performing admission control on each event queue, the service can be well-conditioned to load, preventing resources from being overcommitted when demand exceeds service capacity. SEDA employs dynamic control to automatically tune runtime parameters (such as the scheduling parameters of each stage), as well as to manage load, for example, by performing adaptive load shedding. Decomposing services into a set of stages also enables modularity and code reuse, as well as the development of debugging tools for complex event-driven applications."
  
  - `Building Scalable Enterprise Applications Using Asynchronous IO and SEDA Model`_
  - JCyclone_
  - `Mule ESB`_


----

.. [1] A cross between peak.events_ and PyCells_, but built on 
       Contextual and still able to work with -- or without -- Twisted_. Inspired by the Cells_ library for Common Lisp. 
       Solves the callback-style problems by introducing automatic callback management.
       Instead of worrying about subscribing or "listening" to events and managing the order of callbacks, you just write rules to compute values.  
       The Trellis "sees" what values your rules access, and thus knows what rules may need to be rerun when something changes -- not unlike the    
       operation of a spreadsheet.
       The 'Trellis' name comes from Dr. David Gelernter's 1991 book, "Mirror Worlds", where he describes a parallel programming architecture he called "`The Trellis`_".



.. # Python components: libraries, modules, frameworks
.. _Python: http://www.python.org/
.. _gevent: http://www.gevent.org/
.. _greenlet: http://codespeak.net/py/0.9.2/greenlet.html
.. _greenlets: http://codespeak.net/py/0.9.2/greenlet.html
.. _eventlet: http://eventlet.net/
.. _libevent: http://monkey.org/~provos/libevent/
.. _libev: http://software.schmorp.de/pkg/libev.html
.. _Stackless: http://www.stackless.com/
.. _Stackless - old page: http://www.stackless.com/index_old.htm
.. _Tasklets: http://www.stackless.com/wiki/Tasklets
.. _Twisted: http://twistedmatrix.com/
.. _Twisted DeferredGenerator: http://twistedmatrix.com/trac/wiki/DeferredGenerator
.. _uthreads: http://code.google.com/p/uthreads/
.. _deferreds: http://twistedmatrix.com/documents/current/core/howto/defer.html
.. _peak.events: http://peak.telecommunity.com/DevCenter/events
.. _peak.events.trellis: http://peak.telecommunity.com/DevCenter/Trellis
.. _The Trellis: http://peak.telecommunity.com/DevCenter/Trellis#the-trellis-name
.. _asyncore: http://docs.python.org/library/asyncore.html
.. _Medusa: http://nightmare.com/medusa/medusa.html
.. _PyPy: http://pypy.org/
.. _Tornado: http://www.tornadoweb.org/
.. _PEAK: http://peak.telecommunity.com/
.. _Kamaelia: http://www.kamaelia.org/
.. _cogen: http://code.google.com/p/cogen/
.. _python-multitask: http://code.google.com/p/python-multitask/
.. _Syncless: http://code.google.com/p/syncless/
.. _Weightless: http://weightless.io/weightless
.. _Weightless background: http://weightless.io/background
.. _monocle: https://github.com/saucelabs/monocle
.. _Concurrence: http://opensource.hyves.org/concurrence/
.. _circuits: https://bitbucket.org/prologic/circuits/
.. _Chiral: http://git.emarhavil.com/chiral.git

.. # Other components: libraries, modules, frameworks
.. _ACE: http://www1.cse.wustl.edu/~schmidt/ACE.html
.. _ACE Reactor\: event demultiplexing and event handler dispatching: http://www1.cse.wustl.edu/~schmidt/ACE-papers.html#reactor
.. _JAWS: http://www.dre.vanderbilt.edu/JAWS/
.. _POE\: Perl Object Environment: http://poe.perl.org/
.. _POE Whitepaper: http://poe.perl.org/poedown/poe-whitepaper-a4.pdf
.. _Reflex: https://github.com/rcaputo/reflex/
.. _Apache HTTP Server: http://httpd.apache.org/
.. _Cells: http://common-lisp.net/project/cells/
.. _PyCells: http://pycells.pdxcb.net/
.. _Coroutines for Ruby: http://liber.sourceforge.net/coroutines.rb
.. _Boost.Coroutine: http://www.crystalclearsoftware.com/soc/coroutine/
.. _JavaScript\: Iterators and generators: https://developer.mozilla.org/en/JavaScript/Guide/Iterators_and_Generators
.. _Icon: http://www.cs.arizona.edu/icon/
.. _Icon generators: http://en.wikipedia.org/wiki/Icon_%28programming_language%29#Generators
.. _Node.js: http://nodejs.org/
.. _V8 JavaScript: http://code.google.com/p/v8/
.. _nginx: http://nginx.org/en/
.. _nginx\: Architecture and scalability: http://nginx.org/en/#architecture_and_scalability

.. # Linux Kernel
.. _Linux: http://www.kernel.org/
.. _Linux File AIO: http://lse.sourceforge.net/io/aio.html
.. _An AIO Implementation and its Behaviour: http://www.linuxsymposium.org/archives/OLS/Reprints-2002/lahaise-reprint.pdf
.. _Linux Asynchronous I/O Design: Evolution & Challenges: http://www.kernel.org/pub/linux/kernel/people/suparna/aio-linux.pdf

.. # Misc
.. _ApacheBench: http://httpd.apache.org/docs/2.0/programs/ab.html

.. # Java
.. _Java NIO\: New I/O APIs: http://download.oracle.com/javase/1.4.2/docs/guide/nio/
.. _Java 6\: Enhancements in Java I/O: http://download.oracle.com/javase/6/docs/technotes/guides/io/enhancements.html
.. _Apache MINA: http://mina.apache.org/
.. _Netty: http://www.jboss.org/netty/
.. _Gretty: https://github.com/gretty/gretty
.. _Grizzly: http://grizzly.java.net/
.. _async-http-client: https://github.com/sonatype/async-http-client
.. _JCyclone: http://jcyclone.sourceforge.net/
.. _Mule ESB: http://www.mulesoft.com/mule-esb-open-source-esb
.. _Atmosphere: http://atmosphere.java.net/
.. _Jetty: http://www.eclipse.org/jetty/
.. _Jetty\: Asynchronous Servlets and Continuations: http://wiki.eclipse.org/Jetty/Feature/Continuations
.. _JSR 315\: Java™ Servlet 3.0 Specification: http://jcp.org/en/jsr/detail?id=315

.. # Specifications
.. _PEP 255 -- Simple Generators: http://www.python.org/dev/peps/pep-0255/
.. _PEP 288 -- Generators Attributes and Exceptions: http://www.python.org/dev/peps/pep-0288/
.. _PEP 325 -- Resource-Release Support for Generators: http://www.python.org/dev/peps/pep-0325/
.. _PEP 342 -- Coroutines via Enhanced Generators: http://www.python.org/dev/peps/pep-0342/
.. _enhanced generators: http://www.python.org/dev/peps/pep-0342/
.. _PEP 380 -- Syntax for Delegating to a Subgenerator: http://www.python.org/dev/peps/pep-0380/
.. _PEP\: XXX - Standard Microthreading Pattern: http://code.google.com/p/uthreads/source/browse/trunk/microthreading-pep.txt
.. _PEP\: XXX - Cofunctions: http://www.cosc.canterbury.ac.nz/greg.ewing/python/yield-from/cd_current/cofunction-pep-rev2.txt

.. # WSGI
.. _PEP 333 -- Python Web Server Gateway Interface v1.0: http://www.python.org/dev/peps/pep-0333/
.. _PEP 3333 -- Python Web Server Gateway Interface v1.0.1: http://www.python.org/dev/peps/pep-3333/
.. _PEP 444 -- Python Web3 Interface: http://www.python.org/dev/peps/pep-0444/
.. _WSGI1: http://www.python.org/dev/peps/pep-0333/
.. _WSGI2: http://www.wsgi.org/wsgi/WSGI_2.0
.. _Web3: http://www.python.org/dev/peps/pep-0444/
.. _Project area for Collaborating on the WSGI 2 spec: https://github.com/GothAlice/wsgi2/
.. _Project area for Collaborating on Web3 spec: https://github.com/mcdonc/web3/
.. _WSGI for Python 3: http://www.wsgi.org/wsgi/Python_3
.. _AsyncWSGISketch: http://peak.telecommunity.com/DevCenter/AsyncWSGISketch

.. # .com
.. _Omegle: http://omegle.com/
.. _Spotify: http://spotify.com/
.. _sitesupport: http://sitesupport.com/
.. _eventlet@secondlife: http://wiki.secondlife.com/wiki/Eventlet
.. _Stackless Python In Eve: http://www.slideshare.net/Arbow/stackless-python-in-eve
.. _FriendFeed: http://friendfeed.com/
.. _Meresco: http://meres.co/

.. # Articles & Presentations
.. _Asynchronous Servers in Python: http://nichol.as/asynchronous-servers-in-python
.. _Benchmark of Python WSGI Servers: http://nichol.as/benchmark-of-python-web-servers
.. _libev and libevent: http://blog.gevent.org/2011/04/28/libev-and-libevent/
.. _Comparing gevent to eventlet: http://blog.gevent.org/2010/02/27/why-gevent/
.. _C10K problem: http://www.kegel.com/c10k.html
.. _Interesting scalable server implementations: http://www.kegel.com/c10k.html#examples
.. _Charming Python\: Inside Pythons implementations: http://www.ibm.com/developerworks/library/l-pyth7/
.. _Charming Python\: Iterators and simple generators: http://www.ibm.com/developerworks/library/l-pycon/
.. _Charming Python\: Implementing "weightless threads" with Python generators: http://www.ibm.com/developerworks/library/l-pythrd/
.. _StandardMicrothreadingPattern - Description of a proposed standard microthreading programming pattern: http://code.google.com/p/uthreads/wiki/StandardMicrothreadingPattern
.. _Asynchronous Socket Programming: http://www.nightmare.com/pythonwin/async_sockets.html
.. _The technology behind Tornado, FriendFeed's web server: http://bret.appspot.com/entry/tornado-web-server
.. _Tornado\: Facebook's Real-Time Web Framework - With Paul Buchheit: http://www.livestream.com/f8opentechnology/video?clipId=pla_2ed966ba-a0de-4ebf-9203-347a1f68eb68
.. _Coroutines In Python: http://nightmare.com/~rushing/copython/
.. _Unsung Heroes of Python\: asynchat/asyncore: http://apipes.blogspot.com/2005/05/unsung-heroes-of-python.html
.. _Introduction to MINA: http://gleamynode.net/file_download/8/ACUS2005.pdf
.. _512000 concurrent websockets with Groovy++ and Gretty: http://java.dzone.com/articles/512000-concurrent-websockets
.. _Building Scalable Enterprise Applications Using Asynchronous IO and SEDA Model: http://www.theserverside.com/news/1363672/Building-a-Scalable-Enterprise-Applications-Using-Asynchronous-IO-and-SEDA-Model
.. _To poll or epoll\: that is the question: http://blogs.oracle.com/alanb/entry/epoll
.. _Asynchronous Support in Servlet 3.0: http://blogs.oracle.com/enterprisetechtips/entry/asynchronous_support_in_servlet_3
.. _cogen and greenlets: http://ionelmc.wordpress.com/2009/01/19/cogen-and-greenlets/
.. _Experimental HTTP server using Stackless Python: http://ptspts.blogspot.com/2009/12/experimental-http-server-using.html
.. _Feature comparison of Python non-blocking I/O libraries: http://ptspts.blogspot.com/2010/05/feature-comparison-of-python-non.html
.. _Introduction to Stackless Python: http://www.oreillynet.com/pub/a/python/2000/10/04/stackless-intro.html
.. _Programming Stackless Python: http://www.oreillynet.com/pub/a/python/2000/10/11/stackless-programming.html
.. _Stackless Python and Korea: http://www.stackless.com/spc-sheets-seoul.ppt
.. _Beyond Python Enhanced Generators: http://ep2011.europython.eu/conference/talks/beyond-python-enhanched-generators
.. _Emulating Stackless and greenlet with each other: http://ep2011.europython.eu/conference/talks/emulating-stackless-and-greenlet-with-each-other

.. # Wikipedia
.. _Non-blocking I/O: http://en.wikipedia.org/wiki/Non-blocking_I/O
.. _Non-blocking: http://en.wikipedia.org/wiki/Non-blocking_I/O
.. _asynchronous I/O: http://en.wikipedia.org/wiki/Non-blocking_I/O
.. _asynchronous IO: http://en.wikipedia.org/wiki/Non-blocking_I/O
.. _coroutine: http://en.wikipedia.org/wiki/Coroutine
.. _coroutines: http://en.wikipedia.org/wiki/Coroutine
.. _coroutine-based: http://en.wikipedia.org/wiki/Coroutine
.. _microthreads: http://en.wikipedia.org/wiki/Microthread
.. _microthreading: http://en.wikipedia.org/wiki/Microthread
.. _weightless threads: http://en.wikipedia.org/wiki/Microthread
.. _lightweight threads: http://en.wikipedia.org/wiki/Microthread
.. _lightweight threading: http://en.wikipedia.org/wiki/Microthread
.. _lightweight tasks: http://en.wikipedia.org/wiki/Microthread
.. _thread: http://en.wikipedia.org/wiki/Thread_%28computer_science%29
.. _threads: http://en.wikipedia.org/wiki/Thread_%28computer_science%29
.. _threading: http://en.wikipedia.org/wiki/Thread_%28computer_science%29
.. _thread-based: http://en.wikipedia.org/wiki/Thread_%28computer_science%29
.. _Multi-threaded: http://en.wikipedia.org/wiki/Thread_%28computer_science%29
.. _multi-threading: http://en.wikipedia.org/wiki/Thread_%28computer_science%29
.. _continuations: http://en.wikipedia.org/wiki/Continuation
.. _fibers: http://en.wikipedia.org/wiki/Fiber_%28computer_science%29
.. _cooperative multitasking: http://en.wikipedia.org/wiki/Computer_multitasking#Cooperative_multitasking.2Ftime-sharing
.. _futures: http://en.wikipedia.org/wiki/Future_%28programming%29
.. _event loop: http://en.wikipedia.org/wiki/Event_loop
.. _event loops: http://en.wikipedia.org/wiki/Event_loop
.. _Reactive programming: http://en.wikipedia.org/wiki/Reactive_programming
.. _reactive systems: http://en.wikipedia.org/wiki/Reactive_programming
.. _reactive: http://en.wikipedia.org/wiki/Reactive_programming
.. _scheduler: http://en.wikipedia.org/wiki/Scheduling_%28computing%29
.. _scheduling: http://en.wikipedia.org/wiki/Scheduling_%28computing%29
.. _generator: http://en.wikipedia.org/wiki/Generator_%28computer_science%29
.. _generators: http://en.wikipedia.org/wiki/Generator_%28computer_science%29
.. _event-driven: http://en.wikipedia.org/wiki/Event-driven_programming
.. _EDA\: Event-driven architecture: http://en.wikipedia.org/wiki/Event-driven_architecture
.. _SEDA\: Staged event-driven architecture: http://en.wikipedia.org/wiki/Staged_event-driven_architecture
.. _monkeypatching: http://en.wikipedia.org/wiki/Monkey_patch
.. _CPython: http://en.wikipedia.org/wiki/CPython
.. _preemptive: http://en.wikipedia.org/wiki/Preemption_%28computing%29


.. # Lowlevel: Internals
.. _gevent.core: http://www.gevent.org/gevent.core.html
.. _gevent event loop: http://www.gevent.org/intro.html#event-loop
.. _tornado.ioloop: http://www.tornadoweb.org/documentation/ioloop.html
.. _Apache Event MPM: http://httpd.apache.org/docs/2.2/mod/event.html

.. # Lowlevel: interfaces
.. _epoll: http://www.kernel.org/doc/man-pages/online/pages/man4/epoll.4.html
.. _Kqueue: http://www.freebsd.org/cgi/man.cgi?query=kqueue&apropos=0&sektion=0&format=html
.. _evdns: http://linux.die.net/man/3/evdns

.. # Publications and Papers
.. _Continuations and Stackless Python: http://www.stackless.com/spcpaper.pdf
.. _SEDA\: An Architecture for Highly Concurrent Server Applications: http://www.eecs.harvard.edu/~mdw/proj/seda/
.. _The Problem with Threads: http://www.eecs.berkeley.edu/Pubs/TechRpts/2006/EECS-2006-1.html

.. # Persons
.. _David Mertz: http://gnosis.cx/dW/
.. _Nicholas Piël: http://nichol.as/
.. _Raymond Hettinger: http://rhettinger.wordpress.com/
.. _Guido van Rossum: http://www.python.org/~guido/
.. _Phillip J. Eby: http://dirtsimple.org/programming/
.. _Samuele Pedroni: http://lucediurna.net/
.. _Christian Tismer: http://tismerysoft.de/
.. _Glyph Lefkowitz: http://twistedmatrix.com/glyph/
.. _Bob Ippolito: http://bob.pythonmac.org/
.. _Denis Bilenko: http://denisbilenko.com/
.. _Dustin J. Mitchell: http://people.cs.uchicago.edu/~dustin/
.. _Bret Taylor: http://bret.appspot.com/
.. _Paul Buchheit: http://en.wikipedia.org/wiki/Paul_Buchheit
.. _Sam Rushing: http://www.nightmare.com/~rushing/
.. _Andrew Kuchling: http://www.amk.ca/
.. _Trustin Lee: http://gleamynode.net/
.. _Jeanfrancois Arcand: http://jfarcand.wordpress.com/
.. _Dan Kegel: http://www.kegel.com/
.. _Rocco Caputo: https://github.com/rcaputo/
.. _Marc De Scheemaecker: http://marc.descheemaecker.eu/
.. _Maries Ionel Cristian: http://ionelmc.wordpress.com/
.. _Christopher Stawarz: http://pseudogreen.org/
.. _Gregory Ewing: http://www.cosc.canterbury.ac.nz/greg.ewing/
.. _Edward A. Lee: http://ptolemy.berkeley.edu/~eal/
.. _Péter Szabó: http://ptspts.blogspot.com/
.. _Erik J. Groeneveld: http://seecr.nl/teamlid/erik-groeneveld
.. _Steven Hazel: http://awesame.org/
.. _Cameron Laird: http://phaseit.net/claird/
.. _James Mills: http://prologic.shortcircuit.net.au/
.. _Ryan Dahl: http://tinyclouds.org/
.. _Isaac Schlueter: http://foohack.com/
.. _Igor Sysoev: http://sysoev.ru/en/
.. _Davide Libenzi: http://www.xmailserver.org/davide.html

.. # Misc
.. _EuroPython2011: http://ep2011.europython.eu/
