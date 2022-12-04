#### Difference between ArrayList<String>() and mutableListOf<String>() in Kotlin



[source](https://stackoverflow.com/questions/43114367/difference-between-arrayliststring-and-mutablelistofstring-in-kotlin)

The only difference between the two is communicating your intent.

When you write val a = mutableListOf(), you're saying "I want a mutable list, and I don't particularly care about the implementation". When you write, instead, val a = ArrayList(), you're saying "I specifically want an ArrayList".

In practice, in the current implementation of Kotlin compiling to the JVM, calling mutableListOf will produce an ArrayList, and there's no difference in behaviour: once the list is built, everything will behave the same.

Now, let's say that a future version of Kotlin changes mutableListOf to return a different type of list.

Likelier than not, the Kotlin team would only make that change if they figure the new implementation works better for most use cases. mutableListOf would then have you using that new list implementation transparently, and you'd get that better behaviour for free. Go with mutableListOf if that sounds like your case.

On the other hand, maybe you spent a bunch of time thinking about your problem, and figured that ArrayList really is the best fit for your problem, and you don't want to risk getting moved to something sub-optimal. Then you probably want to either use ArrayList directly, or use the arrayListOf factory function (an ArrayList-specific analogue to mutableListOf).

------
Under the covers, both mutableListOf() and arrayListOf() create an instance of ArrayList. ArrayList is a class that happens to implement the MutableList interface.

The only difference is that arrayListOf() returns the ArrayList as an actual ArrayList. mutableListOf() returns a MutableList, so the actual ArrayList is "disguised" as just the parts that are described by the MutableList interface.

The difference, in practice, is that ArrayList has a few methods that are not part of the MutableList interface (trimToSize and ensureCapacity).

The difference, philosophically, is that the MutableList only cares about the behaviour of the object being returned. It just returns "something that acts like a MutableList". The ArrayList cares about the "structure" of the object. It allows you to directly manipulate the memory allocated by the object (trimToSize).

The rule of thumb is that you should prefer the interface version of things (mutableListOf()) unless you actually have a reason to care about the exact details of the underlying structure.

Or, in other words, if you don't know which one you want, choose mutableListOf first.
