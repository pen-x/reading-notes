## Chapter 1: .NET Application Architectures

1. **Common Language Runtime (CLR)**: Before the existence of .NET with the CLR, every programming language had its own runtime. With C++, the C++ Runtime is linked with every C++ program. Visual Basic 6 had its own runtime with VBRun. The runtime of Java is the Java Virtual Machine—which can be compared to the CLR. The CLR is a runtime that is used by every .NET programming language. At the time the CLR appeared on the scene, Microsoft offered JScript.NET, Visual Basic .NET, and Managed C++ in addition to C#.

2. **Intermediate Language (IL) code**: A compiler for a .NET programming language generates IL code. The IL code looks like object-oriented machine code and can be checked by using the tool ildasm.exe to open DLL or EXE files that contain .NET code. 

3. **Just-in-time (JIT) compiler**: Generates native code out of the IL code when the program starts to run.

4. **Assembly**: Libraries and executables of .NET programs are known by the term assembly. An assembly is the logical unit that contains compiled IL code targeted at the .NET Framework.

5. **NuGet package**: A NuGet package is a zip file that contains the assembly (or multiple assemblies) as well as configuration information and PowerShell scripts. NuGet packages are easily accessible on the NuGet server at http://www.nuget.org.

6. **Compilation Steps**: Before an application can be executed by the CLR, any source code that you develop (in C# or some other language) needs to be compiled. Compilation occurs in two steps in .NET:
    1. Compilation of source code to Microsoft Intermediate Language (IL). The IL code is available within a .NET assembly.
    2. Compilation of IL to platform-specific native code by the CLR. During runtime, a Just-In-Time (JIT) compiler compiles IL code and creates the platform-specific native code.

7. **Compiling with .NET 4.6**: 
    - csc HelloWorld.cs: you can compile program by simply running the C# command-line compiler (csc.exe) against the source file, compiling an executable this way produces an assembly that contains Intermediate Language (IL) code.
    - ildasm.exe:  the assembly can be read using the Intermediate Language Disassembler (IL DASM) tool

8. **Compiling with .NET Core CLI**:
    - dotnet new: create project with
        - a Program.cs file that includes the code for the Hello, World program
        - a NuGet.config file that defines the NuGet server where NuGet packages should be loaded
        - project.json, the new project configuration file
    - dotnet restore: downloads all dependencies needed for the application, as defined in the project.json file
    - dotnet build: compile the application
    - dotnet run: run the application
    - dotnet pack: creates a NuGet package that you can put on a NuGet server
    - dotnet publish: publish the application

9. The **using static** directive designates a type whose static members you can access without specifying a type name.
    ```csharp
    using static System.Console;

    class Program
    {
        static void Main()
        {
            WriteLine("Hello, World!");
        }
    }
    ```

## Chapter 2: Core C#

1. **Initializing Variables**: C# has two methods for ensuring that variables are initialized before use
    - Variables that are fields in a class or struct, if not initialized explicitly, are by default zeroed out when they are created
    - Variables that are local to a method must be explicitly initialized in your code prior to any statements in which their values are used

2. **Type Inference (var)**: Type inference makes use of the var keyword, it must be initialized, and after the variable has been declared and the type inferred, the variable’s type cannot be changed. 

3. **Variable Scopes**: The scope of a variable is the region of code from which the variable can be accessed. In general, the scope is determined by the following rules:
    - A field (also known as a member variable) of a class is in scope for as long as its containing class is in scope.
    - A local variable is in scope until a closing brace indicates the end of the block statement or method in which it was declared.
    - A local variable that is declared in a for, while, or similar statement is in scope in the body of that loop.
Notice: 
    - Local variables with the same name can’t be declared twice in the same scope.
        ```csharp
        static int Main()
        {
            int j = 20;
            for (int i = 0; i < 10; i++)
            {
                int j = 30; // Can't do this, j is still in scope
                WriteLine(j + i);
            }
            return 0;
        }
        ```
    - C# makes a fundamental distinction between variables that are declared at the type level (fields) and variables that are declared within methods (local variables), so they can use the same name.

4. **Constants**:
    - They must be initialized when they are declared. After a value has been assigned, it can never be overwritten.
    - The value of a constant must be computable at compile time. Therefore, you can’t initialize a constant with a value taken from a variable. If you need to do this, you must use a read-only field.
    - Constants are always implicitly static. 

5. **Decimal Type**: The decimal type is a 128-bit, high-precision decimal notation. To specify that your number is a decimal type you can append the M (or m) character to the value:
    ```csharp
    decimal d = 12.30M;
    ```

6. **Character Type**: 
    - Literals enclosed in single quotation marks (for example, 'A').
    - Four-digit hex Unicode values (for example, '\u0041').
    - Integer values with a cast (for example, (char)65).
    - Hexadecimal values (for example,'\x0041').
    - Escape sequence (for example, '\t').

7. **String Type**: 
    - Strings are immutable. Making changes to a string creates an entirely new string object, leaving the other string unchanged.
    - You can prefix a string literal with the at character (@) and all the characters after it are treated at face value, they aren’t interpreted as escape sequences:
        ```csharp
        string filepath = @"C:\ProCSharp\First.cs";
        ```
    - (C# 6) Prefixing a string with $ enables you to put curly braces into the string that contains a variable or even a code expression. The result of the variable or code expression is put into the string at the position of the curly braces:
        ```csharp
        public static void Main()
        {
            string s1 ="a string";
            WriteLine($"s1 is {s1}");
        }
        ```

8. **Foreach Loop**: In foreach loop you can’t change the value of the item in the collection.
    ```csharp
    foreach (int temp in arrayOfInts)
    {
        temp++;     //Cannot assign to 'item' because it is a 'foreach iteration variable'
        WriteLine(temp);
    }
    ```

9. **Preprocessor Directives**: These commands are never actually translated to any commands in your executable code, but they affect aspects of the **compilation process** (from source code to IL code). 
    - #define and #undefine: Tells the compiler that a symbol with the given name (in this case DEBUG) exists/non-exists.
    - #if, #elif, #else and #endif: These directives inform the compiler whether to compile a block of code.
    - #warning: Displays whatever text appears after the #warning to the user, after which compilation continues.
    - #error: Displays the subsequent text to the user as if it is a compilation error message and then immediately abandons the compilation, so no IL code is generated.
    - #region and #endregion: Indicate that a certain block of code is to be treated as a single block with a given name.
    - #line: Alter the filename and line number information that is output by the compiler in warnings and error messages, use the syntax #line default to restore the line to the default line numbering.
        ```csharp
        #line 164"Core.cs"   // We happen to know this is line 164 in the file Core.cs, before the intermediate package mangles it.
        // later on
        #line default      // restores default line numbering
        ```
    - #pragma: Suppress or restore specific compiler warnings.
        ```csharp
        #pragma warning disable 169
        public class MyClass
        {
            int neverUsedField;
        }
        #pragma warning restore 169
        ```

## Chapter 3: Objects and Types

1. **Access Modifiers for Properties**: C# allows the set and get accessors of a property to have differing access modifiers, but one of the accessors must follow the access level of the property. 
    ```csharp
    public string Name { get; private set; }
    ```

2. **Inline Code**: Some developers may be concerned that the previous sections have presented a number of situations in which standard C# coding practices have led to very small functions—for example, accessing a field via a property instead of directly. Will this hurt performance because of the overhead of the extra function call? The answer is no. The JIT compiler is designed to generate highly optimized code and will ruthlessly inline code as appropriate (in other words, it replaces function calls with inline code). A method or property whose implementation simply calls another method or returns a field will almost certainly be inlined.

3. **Expression-Bodied Methods**: C# 6 gives a simplified syntax to method definitions: expression-bodied methods. You don’t need to write curly brackets and the return keyword with the new syntax. The operator => (the lambda operator) is used to distinguish the declaration of the left side of this operator to the implementation that is on the right side.
    ```csharp
    public bool IsSquare(Rectangle rect) => rect.Height == rect.Width;
    ```

4. **Method Arguments**: 
    - Named Arguments: The compiler gets rid of the name and creates an invocation of the method just like the variable name would not be there.
        ```csharp
        public void MoveAndResize(int x, int y, int width, int height)

        r.MoveAndResize(x: 30, y: 40, width: 20, height: 40);
        ```
    - Optional Arguments: You must supply a default value for optional parameters, which must be the last ones defined.
        ```csharp
        public void TestMethod(int n, int opt1 = 11, int opt2 = 22, int opt3 = 33)
        {
            WriteLine(n + opt1 + opt2 + opt3);
        }
        ```
    - Variable Number of Arugments: Allow passing a variable number of arguments。
        ```csharp
        public void AnyNumberOfArguments(params int[] data) {}
        
        AnyNumberOfArguments(1);
        AnyNumberOfArguments(1, 3, 5, 7, 11, 13);
        ```

5. **Static Constructors**:
    - The static constructor can never take any parameters and can access only static members.
    - It’s never called explicitly by any other C# code, but always by the .NET runtime when the class is loaded.
    - The .NET runtime makes no guarantees about when a static constructor will be executed, what is guaranteed is that the static constructor will run at most once, and that it will be invoked before your code makes any reference to the class.

6. **Classes**:
    - Static class can contain only static members and cannot be instantiated.
    - An implementation of the Singleton pattern is shown in the following code snippet.
        ```csharp
        public class Singleton
        {
            private static Singleton s_instance;

            private int _state;
            private Singleton(int state)
            {
                _state = state;
            }

            public static Singleton Instance
            {
                get { return s_instance ?? (s_instance = new MySingleton(42); }
            }
        }
        ```

7. **Readonly Members**: Can be assigned only on initializion or inside constructors
    - Readonly Fields
    - Readonly Properties: Simply omitting the set accessor from the property definition.
        - Auto-implemented Readonly Properties
            ```csharp
            public string Id { get; } = Guid.NewGuid().ToString();
            ```
        - Expression-Bodied Properties (C# 6)
            ```csharp
            public string Id => Guid.NewGuid().ToString();
            ```

8. **Anonymous Types**: An anonymous type is simply a nameless class that inherits from object. 
    ```csharp
    var captain = new
    {
        FirstName ="James",
        MiddleName ="T",
        LastName ="Kirk"
    };
    ```

9. **Structs**: 
    - Structs are value types, not reference types. This means they are stored either in the stack or inline
    - Structs do not support inheritance.
    - Allocating memory for structs is very fast because this takes place inline or on the stack. The same is true when they go out of scope. Structs are cleaned up quickly and don’t need to wait on garbage collection. 
    - Whenever you pass a struct as a parameter or assign a struct to another struct (as in A = B, where A and B are structs), the full contents of the struct are copied, whereas for a class only the reference is copied. (**however, you can avoid this performance loss by passing it as a ref parameter**)

10. **Nulable Types**: 
    - Passing a variable of int to int? always succeeds and is accepted from the compiler.
    - int? cannot be directly assigned to int. This can fail, and thus a cast is required.
    ```csharp
    int x1 = 1;
    int? x2 = x1;
    int x3 = (int)x2;
    ```

11. **Enumerations**:
    - By default, the type behind the enum type is an int, The values of the named constants are incremental values starting with 0, but they can be changed to other values:
        ```csharp
        public enum Color
        {
            Red = 1,
            Green = 2,
            Blue = 3
        }
        Color c2 = (Color)2;
        int number = (int)c2;
        ```
    - You can use an enum type to assign multiple options to a variable and not just one of the enum constants. To do this, the values assigned to the constants must be different bits, and the **Flags** attribute needs to be set with the enum.
        ```csharp
        [Flags]
        public enum DaysOfWeek
        {
            Monday = 0x1,
            Tuesday = 0x2,
            Wednesday = 0x4,
            Thursday = 0x8,
            Friday = 0x10,
            Saturday = 0x20,
            Sunday = 0x40
        }
        DaysOfWeek mondayAndWednesday = DaysOfWeek.Monday | DaysOfWeek.Wednesday;
        WriteLine(mondayAndWednesday);
        ```

12. **Extension Methods**: Extension methods are static methods that can look like part of a class without actually being in the source code for the class. The **this** keyword is needed to match an extension method for a type.
    ```csharp
    public static int GetWordCount(this string s) => s.Split().Length;
    ```

13. **System.Object**: 
    - ToString(): A fairly basic, quick-and-easy string representation.
    - GetHashCode(): If objects are placed in a data structure known as a map (also known as a hash table or dictionary), it is used by classes that manipulate these structures to determine where to place an object in the structure.
    - Equals(Object), Equals(Object, Object), ReferenceEquals(Object, Object): Measure equality.
    - Finalize(): Called when a reference object is garbage collected to clean up resources. 
    - GetType(): Returns an instance of a class derived from System.Type.
    - MemberwiseClone(): Makes a copy of the object and returns a reference (or in the case of a value type, a boxed reference) to the copy.

## Chapter 4: Inheritance

1. **Structs and Classes**
    - Structs are always derived from System.ValueType. They can also be derived from any number of interfaces.
    - Classes are always derived from either System.Object or a class that you choose. They can also be derived from any number of interfaces.

2. **Virtual Methods**:
    - By declaring a base class method as virtual, you allow the method to be overridden in any derived classes.
    - It is also permitted to declare a property as virtual. 
    - In C#, functions are not virtual by default but in Java, by contrast, all functions are virtual. 
    - It requires you to declare when a derived class’s function overrides another function, using the **override** keyword.

3. **Abstract Classes/Methods**:
    - An abstract class cannot be instantiated, but can be assigned with its derived class instance.
    - An abstract method does not have an implementation and must be overridden in any nonabstract derived class.
    - An abstract method is automatically virtual.
    - If any class contains any abstract methods, that class is also abstract and must be declared as such.
    ```csharp
    public abstract class Shape
    {
        public abstract void Resize(int width, int height);   // abstract method
    }

    public class Ellipse : Shape
    {
        public override void Resize(int width, int height)
        {
            Size.Width = width;
            Size.Height = height;
        }
    }

    Shape s1 = new Ellipse();
    ```

4. **Sealed Classes/Methods**:
    - Adding the sealed modifier to a class doesn’t allow you to create a subclass of it.
    - Sealing a method means it’s not possible to override this method.
    - In order to use the sealed keyword on a method or property, it must have first been **overridden** from a base class. If you do not want a method or property in a base class overridden, then don’t mark it as virtual.

5. **Access Modefiers**:
    - **Public**, **protected**, and **private** are **logical access modifiers**. 
    - **Internal** is a **physical access modifier** whose boundary is an assembly.
    - Cannot define types as protected, private, or protected internal because these visibility levels would be meaningless for a type contained in a namespace. Hence, these visibilities can be applied only to members.
    - If you have a nested type, the inner type is always able to see all members of the outer type.

    Modifier | Applies to | Description
    -------- | ---------- | -----------
    public | Any types or members | The item is visible to any other code.
    protected | Any member of a type, and any nested type | The item is visible only to any derived type.
    internal | Any types or members | The item is visible only within its containing assembly.
    private | Any member of a type, and any nested type | The item is visible only inside the type to which it belongs.
    protected internal | Any member of a type, and any nested type | The item is visible to any code within its containing assembly and to any code inside a derived type.

6. **Is and As Operators**:
    - The as operator works similar to the cast operator within the class hierarchy—it returns a reference to the object. However, it never throws an InvalidCastException. Instead, this operator returns null in case the object is not of the type asked for.
    - Instead of using the as operator, you can use the is operator. The is operator returns true or false, depending on whether the condition is fulfilled and the object is of the specified type. After verifying whether the condition is true, a cast can be done because now this cast always succeeds

## Chapter 5: Managed and Unmanaged Resources

1. **Managed/Unmanaged Resources**: Using managed and unmanaged resources means objects that are stored on the **managed** or the **native** heap. Although the garbage collector frees up managed objects that are stored in the managed heap, it isn’t responsible for the objects in the native heap, such as **file handles**, **network connections**, and **database connections**. You have to free them on your own.

2. **Memory Management**: Windows uses a system known as **virtual addressing**, in which the mapping from the memory address seen by your program to the actual location in hardware memory is entirely managed by Windows. As a result, each process of a 32-bit application sees 4GB of available memory, regardless of how much hardware memory you actually have in your computer.
    - **Value Data Types**: 
        - Somewhere inside a processor’s virtual memory is an area known as the **stack**. The stack stores value data types that are not members of objects.
        - A **stack pointer** (a variable maintained by the operating system) identifies the next free location on the stack, it goes from high memory addresses to low addresses. 
        - The compiler converts human-readable variable names into **memory addresses** that the processor understands.
    - **Reference Data Types**:
        - Another area of memory from the processor’s available memory is the **managed heap**, it stores reference data types and works under the control of the **garbage collector**.
        - For "var customer = new Customer()", it allocates memory on the heap to store a Customer object, then it sets the value of the variable customer(on stack) to the address of the memory it has allocated to the new Customer object.
        - By assigning the value of one reference variable to another of the same type, you have two variables that reference the same object in memory. 
        - When a reference variable goes out of scope, it is removed from the stack, but the data for a referenced object is still sitting on the heap. The data remains until either the program terminates or the garbage collector removes it, which happens only when it is no longer referenced by any variables.
    ![](/resources/stack-heap.png)

3. **Garbage Collection**:
    - When the garbage collector runs, it removes all those objects from the heap that are no longer referenced. The GC finds all referenced objects from a root table of references and continues to the tree of referenced objects.
    - As soon as the garbage collector has freed all the objects it can, it compacts the heap by moving all the remaining objects to form one continuous block of memory. Of course, when the objects are moved about, all the references to those objects need to be updated with the correct new addresses, but the garbage collector handles that, too.
    - When objects are created, they are placed within the managed heap. The first section of the heap is called the generation 0 section, or **gen 0**. Your objects remain there until the first collection of objects occurs through the garbage collection process. The objects that remain alive after this cleansing are compacted and then moved to the next section or generational part of the heap — the generation 1, or **gen 1**, section.

4. **Weak Reference**: A weak reference allows the object to be created and used, but if the garbage collector happens to run, it collects the object and frees up the memory. After retrieving the strong reference successfully, you can use it in a normal way, and now it can’t be garbage collected because you have a strong reference again.
    ```csharp
    var weakReference = new WeakReference(new DataObject());

    if (weakReference.IsAlive)
    {
        DataObject strongReference = weakReference.Target as DataObject;
        if (stringReference != null)
        {
            // Use the strong reference.
        }
    }
    else 
    {
        // Reference not available.
    }
    ```

5. **Working with Unmanaged Resources**: You can use two mechanisms to automate the freeing of unmanaged resources.
    - Declare a **destructor** (or **finalizer**) as a member of your class, **this is only called when GC runs**. Objects that do not have a destructor are removed from memory in one pass of the garbage collector, but objects that have destructors require two passes to be destroyed: The first pass calls the destructor without removing the object, and the second pass actually deletes the object. 
        ```csharp
        class MyClass
        {
            ~MyClass()
            {
                // Finalizer implementation, only called when GC runs.
            }
        }
        ```
    - Implement the **System.IDisposable** interface in your class, **this can be called anytime**. The **using** keyword guarantee that Dispose is automatically called against an object that implements IDisposable when its reference goes out of scope.
        ```csharp
        class MyClass: IDisposable
        {
            public void Dispose()
            {
                // implementation
            }
        }

        ResourceGobbler theInstance = null;
        try
        {
            theInstance = new ResourceGobbler();
            // do your processing
        }
        finally
        {
            theInstance?.Dispose();
        }

        // same as
        using (var theInstance = new ResourceGobbler())
        {
            // do your processing
        }
        ```
    -  Dispose sample: If you are creating a finalizer, you should also implement the IDisposable interface.
        ```csharp
        using System;
      
        public class ResourceHolder: IDisposable
        {
            private bool _isDisposed = false;
            
            public void Dispose()   // Clean up all managed and unmanaged resources.
            {
                Dispose(true);
                GC.SuppressFinalize(this);
            }
            
            protected virtual void Dispose(bool disposing)
            {
                if (!_isDisposed)
                {
                    if (disposing)
                    {
                        // Cleanup managed objects by calling their
                        // Dispose() methods.
                    }
                    // Cleanup unmanaged objects
                }
                _isDisposed = true;
            }
            
            ~ResourceHolder()   // Should not attempt to access other managed objects because you can no longer be certain of their state.
            {
                Dispose (false);
            }
            
            public void SomeMethod()
            {
                // Ensure object not already disposed before execution of any method
                if(_isDisposed)
                {
                    throw new ObjectDisposedException("ResourceHolder");
                }
                
                // method implementation
            }
        }
        ```

6. **Unsafe Code**: C# allows the use of **pointers** only in blocks of code that you have specifically marked for this purpose. The keyword to do this is **unsafe**. Methods, classes, structs and block of code can be marked as unsafe, but you cannot mark a local variable unsafe.
    - & means take the address of, and converts a value data type to a pointer — for example, int to *int. This operator is known as the address operator.
    - \* means get the content of this address, and converts a pointer to a value data type — for example, *float to float. This operator is known as the indirection operator (or the de-reference operator).
    - It is not possible to declare a pointer to a class or an array; this is because doing so could cause problems for the garbage collector.
    - You can cast a pointer to any of the integer types. However, because an address occupies 4 bytes on 32-bit systems, casting a pointer to anything other than a uint, long, or ulong is almost certain to lead to overflow errors. If you are creating a 64-bit application, you need to cast the pointer to ulong.
    - If you want to maintain a pointer but not specify to what type of data it points, you can declare it as a pointer to a void - void*.
    - The general rule is that adding a number X to a pointer to type T with value P gives the result P + X*(sizeof(T)). 
        ```csharp
        double* pD1 = (double*)1243324;
        double* pD2 = (double*)1243300;
        long L = pD1-pD2;               // gives the result 3 (=24/sizeof(double))
        ```
    - C# defines an arrow operator -> that enables you to access members of structs through pointers.
    - It is not possible to create **pointers to classes** because during garbage collection, the garbage collector might move object to a new location. The solution is to use the **fixed** keyword, which tells the garbage collector that there may be pointers referencing members of certain objects, so those objects must not be moved.
        ```csharp
        var myObject = new MyClass();
        fixed (long* pObject = &(myObject.X))
        {
            // do something
        }
        ```

7. **Platform Invoke**: To reuse an unmanaged library that doesn’t contain COM objects—it contains only exported functions—you can use Platform Invoke (P/Invoke). With P/Invoke, the CLR loads the DLL that includes the function that should be called and marshals the parameters.

## Chapter 6: Generics

1. **Generics Overview**: With generics, you can create classes and methods that are independent of contained types. 
    - **Performance**: Using value types with non-generic collection classes results in boxing and unboxing when the value type is converted to a reference type, and vice versa.
    - **Type Safety**: Another feature of generics is type safety, the generic type T defines what types are allowed. 
    - **Code Bloat**: When a generic class definition goes into the assembly, instantiating generic classes with specific types doesn’t duplicate these classes in the IL code. However, when the generic classes are compiled by the JIT compiler to native code, a new class for every specific value type is created. Reference types share all the same implementation of the same native class, but since every value type can have different memory requirements, a new class for every value type is instantiated.

2. **Generics Features**: 
    - **Default values**: It is not possible to assign **null** to generic types. That’s because a generic type can also be instantiated as a value type, and null is allowed only with reference types. With the **default** keyword, null is assigned to reference types and 0 is assigned to value types.
    - **Constrains**: The **where** clause defines the requirement to implement the interface IDocument.
    
        Constraint | Description
        ---------- | -----------
        where T: **struct** | With a struct constraint, type T must be a value type.
        where T: **class** | The class constraint indicates that type T must be a reference type.
        where T: **IFoo** | Specifies that type T is required to implement interface IFoo.
        where T: **new()** | A constructor constraint; specifies that type T must have a default constructor.
        where T1: **T2** | With constraints it is also possible to specify that type T1 derives from a generic type T2.
    - **Inheritance**: The requirement is that the generic types of the interface must be **repeated**, or the type of the base class must be **specified**. You can also create a **partial specialization**.
        ```csharp
        public class Base<T> {}
        public class Derived<T>: Base<string> {}

        public abstract class Calc<T> {}
        public class IntCalc: Calc<int> {}

        public class Query<TRequest, TResult> {}
        public StringQuery<TRequest>: Query<TRequest, string> {}
        ```
3. **Covariance and Contra-variance**:  Covariance and contra-variance are used for the conversion of types with arguments and return types.
    - Parameter types are covariant. 
    - Return types of methods are contra-variant.
    - A generic interface is **covariant (Interface\<Derived\> => Interface\<Base\>)** if the generic type is annotated with the **out** keyword. This also means that type T is allowed only with return types. 
        ```csharp
        IEnuremable<Rectangle> rectangles = RectangleGenerator();
        IEnuremable<Shape> shapes = rectangles;
        ```
    - A generic interface is **contra-variant (Interface\<Base\> => Interface\<Derived\>)** if the generic type is annotated with the **in** keyword. This way, the interface is only allowed to use generic type T as input to its methods.
        ```csharp
        Action<Shape> shapeDisplay = s => Console.WriteLine($"Width: {s.Width}, Height: {s.Height}");
        Action<Rectangle> rectangleDisplay = shapeDisplay;
        ```
    - If a read-write indexer is used with the IIndex interface, the generic type T is passed to the method and retrieved from the method. This is not possible with covariance; the generic type must be defined as **invariant**. Defining the type as invariant is done without out and in annotations.

4. **Generic Structs**: Structs can be generic as well, one example is **Nullable\<T\>** where the generic type T needs to be a struct.
    - Non-nullable types can be converted to nullable types, **an implicit conversion** is possible where casting is not required.
    - A conversion from a nullable type to a non-nullable type can fail. If the nullable type has a null value and the null value is assigned to a non-nullable type, then an exception of type InvalidOperationException is thrown, so **an explicit conversion** is required.
    ```csharp
    int y1 = 4;
    int? x1 = y1;
    int x2 = (int)x1;
    ```

5. **Generic Methods**: With a generic method, the generic type is defined with the method declaration. 
    - Generic methods can be defined within non-generic classes.
    - As with generic classes, you can restrict generic types with the where clause. You can use the same clause with generic methods that you use with generic classes.

## Chapter 7: Arrays and Tuples

1. **Arrays**:
    - **Simple Arrays**:
        - An array is a reference type, so memory on the heap must be allocated.
        - An array cannot be resized after its size is specified without copying all the elements, you must set the number of elements during initializing.
        - C# provides a shorter form for initialization.
        ```csharp
        int[] array = new int[4];
        int[] array = new int[4] {4, 7, 11, 12};
        int[] array = {4, 7, 11, 12};
        ```
    - **Multidimensional Arrays**:
        ```csharp
        int[,,] threedim = new int[3, 3, 3];
        ```
    - **Jagged Arrays**: With a jagged array every row can have a different size. To initialize the jagged array, only the size that defines the number of rows in the first pair of brackets is set. The second brackets that define the number of elements inside the row are kept empty because every row has a different number of elements.
        ```csharp
        int[][] jagged = new int[3][];
        jagged[0] = new int[2] {1, 2};
        jagged[1] = new int[6] { 3, 4, 5, 6, 7, 8 };
        jagged[2] = new int[3] { 9, 10, 11 };
        ``` 

2. **Array Class**: Declaring an array with brackets is a C# notation using the Array class, using the C# syntax behind the scenes creates a new class that derives from the abstract base class Array.
    - The Array class is abstract, so you cannot create an array by using a constructor, you can use the static **CreateInstance** method instead. You can set values with the **SetValue** method, and read values with the **GetValue** method, you can also cast the created array to one using C# notation.
        ```csharp
        Array array = Array.CreateInstance(typeof(int), 1);
        array.SetValue(33, 0);
        Console.WriteLine(array.GetValue(0));

        int[] array2 = (int[])array;
        ```
    - The array implements the interface **ICloneable**. The **Clone** method that is defined with this interface creates a shallow copy of the array. For value types all values are copied, for reference types only the references are copied, not the elements.
    - **Array.Copy** also creates a shallow copy, however with this method you must pass an existing array with the same type and enough elements.
    - The Array class uses the Quicksort algorithm to sort the elements in the array. The **Sort** method requires the interface **IComparable** to be implemented by the elements in the array. Simple types such as System.String and System.Int32 implement IComparable.
    - **Array.Sort** can also accept a second parameter which implements **IComparer** interface. This interface is independent of the class to compare and needs two arguments that should be compared.

3. **Arrays as Parameters**:
    - With arrays, **covariance** is supported. This means that an array can be declared as a base type and elements of derived types can be assigned to the elements. But notice that array covariance is only possible with reference types, not with value types.
    - **ArraySegment\<T\>** represents a segment of an array, it contains information about the segment (the offset and count). If elements of the array segment are changed, the changes can be seen in the original array.

4. **Enumerators**:
    - The array or collection implements the **IEnumerable** interface with the **GetEnumerator** method. The GetEnumerator method returns an enumerator implementing the **IEnumerator** interface.

        ![](resources\enumerable.png)
    - IEnumerator defines the property **Current** to return the element where the cursor is positioned, and the method **MoveNext** to move to the next element of the collection. MoveNext returns true if there’s an element, and false if no more elements are available.
    - The **foreach** statement invokes the GetEnumerator method of a collection to iterate through its elements.
        ```csharp
        IEnumerator<Person> enumerator = persons.GetEnumerator();
        while (enumerator.MoveNext())
        {
            Person p = enumerator.Current;
            WriteLine(p);
        }
        ```
    - C# provides **yield** statement for creating enumerators easily. A method or property that contains yield statements is also known as an iterator block. An iterator block must be declared to return an IEnumerator or IEnumerable interface, or the generic versions of these interfaces. This block may contain multiple yield return or yield break statements; a return statement is not allowed.
        ```csharp
        public IEnumerator<string> GetEnumerator()
        {
            yield return"Hello";
            yield return"World";
        }
        ```
    - With the yield statement you can also do more complex things, such as return an enumerator from yield return.

5. **Tuples**: Whereas arrays combine objects of the same type, tuples can combine objects of different types. The .NET Framework defines eight generic Tuple classes and one static Tuple class that act as a factory of tuples. The different generic Tuple classes support a different number of elements—for example, Tuple<T1> contains one element, Tuple<T1, T2> contains two elements, and so on.  
    ```csharp
    var tuple = Tuple.Create<string, string, string, int, int, int, double,
        Tuple<int, int>>("Stephanie","Alina","Nagel", 2009, 6, 2, 1.37,
            Tuple.Create<int, int>(52, 3490));
    ```

6. **Structural Comparison**: 
    - Both arrays and tuples implement the interfaces IStructuralEquatable and IStructuralComparable. These interfaces compare not only references but also the content. This interface is implemented explicitly, so it is necessary to cast the arrays and tuples to this interface on use. IStructuralEquatable is used to compare whether two tuples or arrays have the same content; IStructuralComparable is used to sort tuples or arrays.