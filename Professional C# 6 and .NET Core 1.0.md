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