# Debugging Shaders

We may have bugs in our shader programs.

During the running, the main C++ program will compile our shader programs and report error.

Don't panic when you see your program stops and reports erros about shaders.

For example:

<figure><img src="../../../../.gitbook/assets/b5e837fe-01eb-4f77-9ecc-3d9ff3ebaac0.png" alt=""><figcaption></figcaption></figure>

And this:

<figure><img src="../../../../.gitbook/assets/86a5a9c0-6e5f-468a-8fa8-8e826e4b0f51.png" alt=""><figcaption></figcaption></figure>

The shader compiler will tell you the location (the line number and offset) of the error with a description.

Try to use the description as a reference to fix your bug.
