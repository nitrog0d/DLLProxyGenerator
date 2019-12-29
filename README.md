# DLL Proxy Generator
This project creates a new DLL which sits between a program and the original DLL. This way you can intercept all DLL calls (this is also a way to auto inject code).   
Program -> Your proxy DLL -> Original DLL  
Based on ProxiFy, by Kristoffer Blasiak (https://www.codeproject.com/Articles/1179147/ProxiFy-Automatic-Proxy-DLL-Generation). 
## Build

Open DLLProxyGenerator.sln with Visual Studio and build it.  
You can also use the [latest release](https://github.com/nitrog0d/DLLProxyGenerator/releases/latest).

## Usage

### Generate the proxy DLL source
DLLProxyGenerator.exe "path/to/your/dll"  
You can also drag a DLL file to the .exe

### Build the proxy DLL
Create a new Visual Studio C++ DLL project. Copy the generated proxy files into your project.
Remove every other file like stdafx.h, pch.h, framework.h, dllmain.cpp, whatever.
Change the following settings:

* General -> Project Defaults -> Character Set = Use Multi-Byte Character Set
* C/C++ -> Precompiled Headers -> Precompiled Header = Not Using Precompiled Headers
* Linker -> Input > Module Definition File = dllname.def (the .def file you copied to the project folder)

If your DLL is 64 bits (from [ProxiFy](https://www.codeproject.com/Articles/1179147/ProxiFy-Automatic-Proxy-DLL-Generation)):
* A .asm file was created alongside the .cpp and .def files if the DLL is 64 bits. Before you add this to the project, you should right click the project -> Build Dependencies -> Build Customizations and then check ".masm". This will allow the .asm file to work correctly.
* Now add the .asm file to the project as well. I'm not sure if the correct settings are set automatically, so to double check, right click it and go to properties. And under "General -> Item Type", make sure it's set to: Microsoft Macro Assembler.

Now go to the dllmain.cpp file and find DllMain, you'll have to set up the original DLL path (you should use Process Explorer to find out the DLL path, use [this guide](https://kb.froglogic.com/misc/getting-list-of-loaded-dlls/)) and your code there.

### Use the new DLL
Copy your new proxy inside the program directory.
If everything is working, your program will launch up and work fine (best sign ever).