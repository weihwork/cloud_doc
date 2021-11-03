# ownership
The types covered previously are all a known size, can be stored on the stack and popped off the stack when their scope is over, and can be quickly and trivially copied to make a new, independent instance if another part of code needs to use the same value in a different scope. But we want to look at data that is stored on the heap and explore how Rust knows when to clean up that data.


1. Each value in Rust has a variable that’s called its owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

We’ve already seen string literals, where a string value is hardcoded into our program. 
string literals is immutable.

In the case of a string literal, we know the contents at compile time, so the text is hardcoded directly into the final executable.

With the String type, in order to support a mutable, growable piece of text, we need to allocate an amount of memory on the heap, unknown at compile time, to hold the contents. This means:

    The memory must be requested from the memory allocator at runtime.
    We need a way of returning this memory to the allocator when we’re done with our String.

堆上内存如何释放问题

Rust takes a different path: the memory is automatically returned once the variable that owns it goes out of scope. 

    When a variable goes out of scope, Rust calls a special function for us. This function is called drop, and it’s where the author of String can put the code to return the memory. Rust calls drop automatically at the closing curly bracket.
the behavior of code can be unexpected in more complicated situations when we want to have multiple variables use the data we’ve allocated on the heap. 

    let x = 5;
    let y = x;
integers are simple values with a known, fixed size, and these two 5 values are pushed onto the stack.

赋值操作时，栈上数据做复制操作，堆上数据转移所有权

A String is made up of three parts: a pointer to the memory that holds the contents of the string, a length, and a capacity. This group of data is stored on the stack. 
the memory on the heap holds the contents.

When we assign s1 to s2, the String data is copied, meaning we copy the pointer, the length, and the capacity that are on the stack. We do not copy the data on the heap that the pointer refers to.

when a variable goes out of scope, Rust automatically calls the drop function and cleans up the heap memory for that variable. 

After let s2 = s1, Rust considers s1 to no longer be valid. Therefore, Rust doesn’t need to free anything when s1 goes out of scope. 

If we do want to deeply copy the heap data of the String, not just the stack data, we can use a common method called clone. 

When you see a call to clone, you know that some arbitrary code is being executed and that code may be expensive. It’s a visual indicator that something different is going on.

    ？？？If a type implements the Copy trait, an older variable is still usable after assignment. Rust won’t let us annotate a type with the Copy trait if the type, or any of its parts, has implemented the Drop trait. ？？？


Passing a variable to a function will move or copy, just as assignment does. 

Returning values can move or copy

assign values can move or copy

When a variable that includes data on the heap goes out of scope, the value will be cleaned up by drop unless the data has been moved to be owned by another variable.


In Rust, the compiler guarantees that references will never be dangling references: if you have a reference to some data, the compiler will ensure that the data will not go out of scope before the reference to the data does.

At any given time, you can have either one mutable reference or any number of immutable references.

A string slice is a reference to part of a Strin

Another data type that does not have ownership is the slice. Slices let you reference a contiguous sequence of elements in a collection rather than the whole collection.

    let s = String::from("hello world");
    let hello = &s[0..5];
This is similar to taking a reference to the whole String but with the extra [0..5] bit. Rather than a reference to the entire String, it’s a reference to a portion of the String.

The type that signifies “string slice” is written as &str




