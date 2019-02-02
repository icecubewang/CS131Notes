# Language Systems

There is more to the language than just its formal syntax though. Languages/notations are meant to be *written* and *used*. In practice, this often means there is a particular **environment** 🌎 🌍 🌏  associated with a language, and the scope of this environment can vary greatly. 

Although most programmers use IDEs (integrated development environments), historically programs were written in a much more involved process.

## Unintegrated Systems 👨👨👶 

1. Write the program as a *source file* in a text editor (emacs). The program cannot be run by the text editor. We must pass the source file to the next step.
2. Compiler translates the source code into assembly.
3. Assembler translates the assembly into machine code, changing variables to memory addresses.
4. We now have an *object file* that can be understood by the machine but is no longer readable to humans.
5. Before running the object file though, we have to use the *linker* to resolve all variable and function references so that the machine knows exactly where everything resides.
6. After linking, we have an *executable* that can finally be run by the hardware.
   1. We need an additional, although minimal, component called the *loader* to bring the program instructions into main memory and - finally - translate all the named references into memory addresses.

Summarized:

```
editor => [source file] => compiler => [assembly file] => assembler => [object file] => linker => [executable] => [loader] => process (running program)
```

### Optimization ⚡️ 

Optimization is usually done at compile time, with many different tricks being used. However, as we will see later, optimization can be performed at other times too.

## Variations 🌈 

### Hide Intermediates

Many systems combine the compile-assembly-link steps into one command/click.

- ex: `gcc` takes in `main.c` and outputs `a.out`, an executable, showing none of the assembly or linking steps

### IDEs 👨‍👨‍👦

Take the above strategy even further, providing many of the following:

- syntax highlighting
- auto completion
- version control
- fetching remote code (i.e. stored in database)
- deploying remotely
- dependency analysis and smart rebuilding (you changed one part of your system and don't wanna recompile everything)

### Interpreters

What does the following code do:

```
x = 14
y = 49
while x != y do
	if x < y then y = y - x else x = x - y
print x
```

...

...

...

Well, if you figured it out, good on you! 👍 It doesn't really matter though, the point of this was to show that you can look at the program and execute it in your head, mentally running through the steps. This process is called **interpreting** and some languages/language systems run on interpretation rather than compilation. This has the advantage of running the program immediately, helpful for quick troubleshooting and initial development, with the drawback of - classically - being slower.

In reality the difference between compilers and interpreters is not as simple as "compilers translate into machine code and interpreters execute immediately." But that's for another time 🤸‍♂️

### Virtual Machines 📺🤖

Another problem with compilation is that it's machine-dependent: every time you want to run a program on a new computer, you have to recompile the whole thing. What if there was a way to fix this?

Well! Luckily, we have virtual machines.

Virtual machines are a type of software "simulator" that run a specific kind of executable made for them. Language systems designed for virtual machines undergo the same classical steps in language compilation, just for a different end target. 

- Virtual machines are really a kind of **interpreter** that run their own low-level language called **intermediate code**

Virtual machines give us the pros of being able to run on any platform that can run our interpreter and also of having a "watchdog" that can monitor the executing program (other than the OS ofc). 👀 

The most famous example of this is the **Java virtual machine** that runs on nearly every browser out there. But there really exists an entire spectrum of possibilities:

1. Pure interpreter - the "intermediate code" is the same as the high level code we write. Worst for performance.
2. Tokenize - Strip out whitespace and comments, identify keywords, and then run
3. Bytecodes - Compile into low-level intermediate code (Java VM calls them "bytecodes") and execute in a virtual machine/interpreter
4. Pure compilation - Translate high level code into physical machine code ("physical machine" here being contrasted with virtual machine)

### Dynamic Linking 

Linking does not always need to be packaged directly into the source code however. What if the code we run only needs one function from that massive library it included? Or if every other program we run wants that same library in the same exact version? It would be wasteful to package the entire library into every program when we could share or delay the load of such libraries until really needed.

This is the basis for **dynamic linking**. Although a pretty common concept, different OS have different ways of doing this. Let's look at a few.

#### Windows

1. Load time dynamic linking - A program gets a link to every function in the libraries it needs, which may already be in memory, just before running
2. Run-time dynamic linking - A program makes explicit calls to library files at runntime to load individual functions from libraries (less common)

#### Unix

1. Shared libraries - Linked to programs just before running 
2. Dynamically loaded libraries - Program makes explicit calls at runtime 

### Profiling 📸 

The program is compiled *twice*, once to observe runtime behavior and collect data on how may times each piece of the program was run and how much time is spent in certain parts, and then twice to make the efficient version based on the profile gathered.

### Just-in-Time Compilation ⏰ 

Some compiling takes place after the program starts running. Sometimes this means compiling each function only when called or sometimes it means compiling only pieces called frequently. Additionally, the Java VM may truly compile high-traffic parts of its intermediate code. 

## Binding 🧵

Take the following C program that calls `fred` 100 times:

```c
int i;
void main() {
    for (i=1; i<=100; i++) {
        fred(i);
    }
}
```

Most of the tokens in this program are **tokens**🏅: `int`, `i`,`void`, `main`, `for`, `fred`. They have no inherent meaning of their own like `1` or `100` do. Each are *named*, and have properties that need to be determined. Which brings us to...

🧵 *Binding* - The act of associating properties with names.

- Note that this does not just mean values with names, but can mean things like type, memory address, and possible values.

Binding can happen at many different times, depending on what is being bound.

1. **Language-definition time** - In C, `void` and `for` are reserved keywords: they are defined when the language is defined and any system that conforms to the C standard binds these values.
2. **Language-implementation time** - In the example above, the range of values that the `int` type can take would be implementation dependent and thus bound by the compiler depending on the machine it's on. Other things that may be decided are the maximum length of a name, the max size of an array, and the max number of stack calls.
   1. In Java, `int` is bound at language-definition time and is thus 32 bits no matter what machine is running.
3. **Compile time** - All statically-typed languages bind the types of variables at compile time (i.e. `i` is an `int`). In addition, the scoping of variables and all references are resolved in this step (i.e. if we have `i` and `i` in two different scope the compiler will translate to `i` and `i'` basically).
4. **Link time** - Here the references to function and libraries are bound.
5. **Load time** - Memory addresses are bound to variables.
6. **Runtime** - Most bindings take place at runtime. Bindings that happen at runtime are sometimes called *late bindings*, while anything else is *early*. Early bindings generally lead to faster, more secure code, while late bindings allow more flexibility.
   1. Sometimes languages specify that some properties must be bound at specific time. ex: Java specifies all variables must be assigned before referenced and that this must be checked at compile time.

## Debugging Tools 🐜 

Most language systems also provide debuggers that provide the commonly known abilities: core dumps, tracebacks, breakpoints, step-ins, etc.

An interesting point is that as compilers optimize code, they can obfuscate the original source, making debugging a bit difficult.

## Runtime Support 🏥 

- Exception handling (what happens if divide by 0?) - Some languages like Java provide `try` statements that provide details on exceptions
- Garbage collection/memory management
- Operating system interface (getting mouse and keyboard interaction)
- Concurrency

## Final Review

Remember ☝️: edit -> compile -> assemble -> link -> load -> run

Programming language !== programming language system