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

1. INITIALIZING VARIABLES: C# has two methods for ensuring that variables are initialized before use
    - Variables that are fields in a class or struct, if not initialized explicitly, are by default zeroed out when they are created
    - Variables that are local to a method must be explicitly initialized in your code prior to any statements in which their values are used

2. VARIABLE SCOPE:
    - local variables with the same name can’t be declared twice in the same scope.

3. Constants:
    - They must be initialized when they are declared. After a value has been assigned, it can never be overwritten.
    - The value of a constant must be computable at compile time. Therefore, you can’t initialize a constant with a value taken from a variable. If you need to do this, you must use a read-only field (this is explained in Chapter 3).
    - Constants are always implicitly static. However, notice that you don’t have to (and, in fact, are not permitted to) include the static modifier in the constant declaration.

4. Decimal Type 128-bit, high-precision decimal notation To specify that your number is a decimal type rather than a double, a float, or an integer, you can append the M (or m) character to the value, as shown here:
    ```csharp
    decimal d = 12.30M;
    ```

5. Character Type: 