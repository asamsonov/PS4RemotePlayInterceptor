# PS4 Remote Play Interceptor

![nuget](https://img.shields.io/nuget/v/PS4RemotePlayInterceptor.svg)

A small .NET library to intercept controls on PS4 Remote Play for Windows, powered by [EasyHook](https://easyhook.github.io/). The library can be used to automate any PS4 game. See the [prototype demo](https://youtu.be/QjTZsPR-BcI).

Also check out [PS4 Macro](https://github.com/komefai/PS4Macro) repository for a ready-to-use software built on this library.

## Install

#### Using NuGet (Recommended)
```
Install-Package PS4RemotePlayInterceptor
```

#### From Source
Add reference to PS4RemotePlayInterceptor.dll.

## Example Usage

This console application will hold the X button while moving the left analog stick upwards until interrupted by a keypress.

```csharp
using PS4RemotePlayInterceptor;

class Program
{
    static void Main(string[] args)
    {
        // Setup callback to interceptor
        Interceptor.Callback = new InterceptionDelegate(OnReceiveData);
        // Start watchdog to automatically inject when possible
        Interceptor.Watchdog.Start();

        // Or inject manually and handle exceptions yourself
        // Interceptor.Inject();

        Console.ReadKey();
    }

    private static void OnReceiveData(ref DualShockState state)
    {
        /* -- Modify the controller state here -- */

        // Force press X
        state.Cross = true;

        // Force left analog upwards
        state.LY = 0;

        // Force left analog downwards
        // state.LY = 255;

        // Force left analog to center
        // state.LX = 128;
        // state.LY = 128;
    }
}
```

## To-Do List

- Intercept ouput reports
- Emulating DualShock controller

## Troubleshoot

- > {"STATUS_INTERNAL_ERROR: Unknown error in injected C++ completion routine. (Code: 15)"}

SOLUTION: Restart PS4 Remote Play.

- > DualshockState Could not be found

SOLUTION: Rename to DualShockState for version >= 0.2.0

- > Injection IPC failed (on some machines)

SOLUTION: Inject with Compatibility mode instead

```csharp
// Setup callback to interceptor
Interceptor.Callback = new InterceptionDelegate(OnReceiveData);

// Inject
Interceptor.InjectionMode = InjectionMode.Compatibility;
Interceptor.Inject();
```

## Credits

- https://easyhook.github.io/
- https://github.com/Jays2Kings/DS4Windows
- http://www.psdevwiki.com/ps4/DS4-USB
