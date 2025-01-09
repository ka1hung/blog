+++
title= "How to hide the Console window when running a Golang compiled file"
date= "2024-03-20T10:37:55+08:00"
tags=["windows","golang"] 
+++

## 🎯 Ninja Ways to Hide Console Window in Go

Ever wanted to make your Go applications look more professional by hiding that pesky console window? You're in the right place! Let's explore some ninja techniques to achieve this. 🥷

## 🚀 Method 1: The Quick Strike (-ldflags)

The simplest way to hide the console window is using build flags. Think of it as a stealth mode for your app!

```bash
go build -ldflags -H=windowsgui main.go
```
Pros:

Super simple to implement
No code changes needed
Works for most basic applications
Cons:

Limited flexibility
Can't toggle console visibility at runtime
## 🎭 Method 2: The Shape-Shifter (syscall)
Want more control? Let's use Windows API calls to dynamically hide/show the console!

``` golang
package main

import (
    "fmt"
    "syscall"
)

func main() {
    hideConsole()
    // Your awesome code here!
}

func hideConsole() {
    console := syscall.MustLoadDLL("kernel32").MustFindProc("GetConsoleWindow")
    if console != nil {
        showWindow := syscall.MustLoadDLL("user32.dll").MustFindProc("ShowWindow")
        hwnd, _, _ := console.Call()
        if hwnd != 0 {
            showWindow.Call(hwnd, 0) // 0 = SW_HIDE
        }
    }
}

// Want to show it again? Just use this!
func showConsole() {
    console := syscall.MustLoadDLL("kernel32").MustFindProc("GetConsoleWindow")
    if console != nil {
        showWindow := syscall.MustLoadDLL("user32.dll").MustFindProc("ShowWindow")
        hwnd, _, _ := console.Call()
        if hwnd != 0 {
            showWindow.Call(hwnd, 5) // 5 = SW_SHOW
        }
    }
}
```
## 🎨 Method 3: The Artist's Way (manifest.xml + syso)
This is the professional approach! Create a manifest file for your application:

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <assemblyIdentity version="1.0.0.0" processorArchitecture="*" name="YourCoolApp" type="win32"/>
    <dependency>
        <dependentAssembly>
            <assemblyIdentity type="win32" name="Microsoft.Windows.Common-Controls" version="6.0.0.0" processorArchitecture="*" publicKeyToken="6595b64144ccf1df" language="*"/>
        </dependentAssembly>
    </dependency>
</assembly>
```
Then follow these magical steps:

``` bash
# Install the wizard's tool
go install github.com/akavel/rsrc@latest

# Create the magic potion (syso file)
rsrc -manifest manifest.xml -o app.syso

# Build your masterpiece
go build
```
## 🧙‍♂️ Pro Tips and Tricks
### Logging Magic
When your console is hidden, don't forget to implement proper logging:
``` golang
package main

import (
    "log"
    "os"
)

func main() {
    // Create your spell book (log file)
    logFile, _ := os.OpenFile("app.log", os.O_RDWR|os.O_CREATE|os.O_APPEND, 0666)
    defer logFile.Close()
    
    // Direct your magical energies (logs) to the spell book
    log.SetOutput(logFile)
    
    // Cast your spells (write logs)
    log.Println("✨ Application started!")
}
```
### Debug Mode Toggle
Add this cool feature to show/hide console based on command line flags:
``` golang
package main

import "flag"

func main() {
    debug := flag.Bool("debug", false, "show console window")
    flag.Parse()
    
    if !*debug {
        hideConsole()
    }
    // Rest of your awesome code
}
```
## 🎮 Best Practices
Always provide error logging mechanisms
Consider adding a debug mode
Test thoroughly on different Windows versions
Keep a development build with console visible
Handle panics gracefully
## 🌟 When to Use What?
Quick Project: Go with Method 1 (-ldflags)
Need Runtime Control: Choose Method 2 (syscall)
Professional App: Use Method 3 (manifest)
## 🎉 Conclusion
Now you're equipped with all the ninja techniques to hide that console window like a pro! Remember, with great power comes great responsibility - make sure to implement proper logging and error handling in your invisible applications!

## 📚 Further Reading
[Windows API Documentation](https://learn.microsoft.com/en-us/windows/win32/api/)  
[Go Windows Examples](https://github.com/golang/go/wiki/WindowsDLLs)  
[rsrc Tool Documentation](https://github.com/akavel/rsrc)  
