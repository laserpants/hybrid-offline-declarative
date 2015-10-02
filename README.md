# Hybrid, offline and declarative

## Hybrid mobile web apps

Hybrid mobile web applications behave more like native apps, but are created using our everyday web technologies (HTML, CSS, and JavaScript). These applications run inside a native [WebView](http://developer.android.com/reference/android/webkit/WebView.html) container on the target device. From normal users' perspective, this means little or no difference in terms of overall experience. You find these applications in app stores online and install them to your device just like any other app. For application developers, these technologies offer some important advantages over vendor-proprietary SDKs. In particular, web technologies are

* already familiar to most developers, or otherwise easy to learn;
* inherently cross-platform; and
* founded on open standards.

UX designers can leverage their existing web design skills and rely on CSS toolkits (e.g. [Twitter Bootstrap](http://getbootstrap.com/)) to build sophisticated and highly interactive user interfaces with modern, responsive features. For businesses, the advantage is that their development team can target more than one platform, which means a single code base to maintain.

The overall trend today is that more and more applications run either in the browser, or in a browser-like environment. Language technologies are quickly adapting to keep up with this new reality. Compilers and other build tools are capable of producing code that runs in legacy browsers and performs well even on low-end devices. In fact, modern JavaScript runtimes have performance characteristics comparable to native code execution.

Although there may be capabilities unique to a specific device which are unavailable using the hybrid app approach, plugins and native APIs can be utilized to target most platform-specific requirements.

Some questions to consider before making the decision to develop a hybrid app:

* What are the target devices?
* What does the development team's skill set look like?
* What capabilities of the device does the application need to utilize?

## Offline capabilities

Despite large-scale adaptation of smartphone technologies, bandwidth and network connectivity issues remain a barrier for wider acceptance of the browser as a service platform in the developing world. Slow and disruptive connections effectively render most web-based applications useless in these environments. Offline capabilities (an application's ability to continue working while disconnected) can therefore be crucial to a project's success in regions undergoing this transition from SMS and USSD based services to mobile web.

#### Ground computing as a catalyst to drive innovation

For a system to be able to operate effortlessly offline, the conventional client-server model is clearly inadequate. Since this aspect of system behavior is largely neglected in other parts of the world, modern cloud architecture is doing little more (if anything) to solve the problem. Instead we need to look beyond the cloud, to an architectural style sometimes referred to as *ground computing* -- applications that run on the device (i.e., on the ground) with robust data replication built in as an integral part of the software stack. 

The [Groundfork synchronization framework](https://github.com/johanneshilden/groundfork-js) was designed specifically for these environments. Important requirements were that:

* User experience is unaffected by network connectivity. (It shouldn't matter if the device is offline.)
* Syncing is effortless on the part of the user.
* The solution introduces no coupling between replication/synchronization and business logic. (App developer doesn't have to think about syncing.)
* No assumption is made w.r.t. replication topology.

Groundfork is open-source software and primarily intended to be used in browser-based applications, i.e., those using the hybrid app approach described earlier.

The solution implements a software design pattern known as the [Command pattern](https://en.wikipedia.org/wiki/Command_pattern). Access to application resources is relayed through a programming interface which enables the system to maintain a [transaction log](https://en.wikipedia.org/wiki/Transaction_log) of changes while the device is offline. These changes are subsequently propagated and synchronized with other devices with the help of a web service, when connectivity is available. This service can run in the cloud or on a host system available to all network nodes. 

## Declarative programming techniques

Declarative, in this context, means writing programs more in terms of what things are (description), and not so much in terms of what the computer should do in order to arrive at some result (prescription). In most languages, a program is made up of sequences of statements that perform direct operations on program state. Variables are merely placeholders for the storage facilities provided by the underlying machine layer (computer memory and registers) and functions behave nothing like how we expect them to behave in math. This makes it difficult to study these programming constructs in a more mathematically rigorous context. E.g., what does it mean for a program to be formally correct?

In contrast, [functional programming](https://en.wikipedia.org/wiki/Functional_programming) (a form of declarative programming) favors construction over mutation, which is to say that all data is immutable. Since this is closer to how we are used to think about expressions in math and logic, it enables us to apply similar techniques of reasoning to program code. Just like when manipulating formulae in algebra and arithmetic, we can replace pieces of code with others without worrying about implementational details like control flow, evaluation order or program state. (This is generally not possible in an imperative language setting, since the value of a given code block depends on the context in which it appears.) 

Perhaps contrary to most people's intuition, the notation `x = 42` in a typical programming language doesn't really assert an equality between a variable `x` and the value `42`. That is, the left and right side of the equals sign are not substitutable for each other both ways. Instead, this notation is an instruction for the computer to assign the value 42 to the storage container `x` when the statement is executed. (In some languages, the syntax `x := 42` is used instead to avoid this abuse of notation.) This type of assignment is sometimes referred to as a *destructive* assignment and is not possible in pure functional programming. 

An important consequence here is that functions are free from *side-effects* -- repeated evaluation of the same function procedure always yields the same result (return value). The fact that mainstream programming languages do not possess this property is one of the primary sources of bugs in software. The problem gets a lot worse when [concurrent and parallel](https://en.wikipedia.org/wiki/Concurrency_(computer_science)) programming is involved. This is also the reason why declarative languages are preferred in these problem domains. As an example, [Erlang](http://www.erlang.org/) is a functional programming language that has successfully been used to solve problems involving large-scale concurrency in the telecom industry.

Proponents of declarative programming argue that it generally leads to program code which is

* more readable, 
* compact,
* less error-prone, and 
* easier to maintain.

To illustrate this, below are two versions of the insertion sort algorithm. One imperative, and the other declarative.

#### Imperative (implementation in C)

```c
void insertion_sort (int *n, size_t s)
{
    int *i, *j, *top, t;
    i = n;
    top = n + s - 1;
    while (1) {
        while (*(i + 1) > *i && i < top)
            ++i;
        if (i == top)
            break;
        j = i++;
        t = *i;
        while (t < *j && j >= n) {
            *(j + 1) = *j;
            *(j--) = t;
        }
    }
}
```

#### Declarative (implementation in Haskell)

```haskell
insort [] = []
insort (x:xs) = insert x (insort xs)
```

The point here is not to show that one language is better than the other -- these two programs have very different behavior, although they have the same external meaning. In particular, performance considerations have been completely left out of the discussion. Poor [cache locality](https://en.wikipedia.org/wiki/Locality_of_reference) is often mentioned by critics as an argument against this programming paradigm, especially when applying some of the ideas mentioned here to other languages (those based on the conventional, stateful model of computation). Still, there is lots to be gained by adding a more declarative set of tools to the repertoire of most programmers. Many modern JavaScript libraries (e.g. Facebook's [React](https://facebook.github.io/react/)) follow this philosophy and rely less on state and object-oriented programming practices. In React, a guiding principle is that components should be stateless and behave as ([referentially transparent](https://wiki.haskell.org/Referential_transparency)) functions of their input (properties). Even though JavaScript is not a functional programming language, recent additions to the ECMAScript standard make this declarative approach a more natural choice.

In summary, declarative and functional programming can help us to:

* create clear and simple abstractions;
* make better use of ideas and knowledge from other branches of science and engineering;
* apply techniques of equational reasoning to program code; 
* write better concurrent programs; 
* introduce less bugs; and
* talk about correctness properties of programs in more formal ways.




