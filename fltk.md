### FLTK 1.3.4-2 + VS2017 community

#### Go to: http://www.fltk.org/index.php get fltk-1.3.4-2-source.tar.gz 

#### Compiling and building fltk-1.3.4 from source

```
C:\Users\fltk-1.3.4-2-source\fltk-1.3.4-2\ide\VisualC2010
```

Open (double-click) fltk.sln.

Visual Studio 2017 Community will open, and ask you if you want to update the files.

Right-click the demo solution file. Click on Set as StartUp Project.

Click on Build and Build Solution.

Output window shows Build: 79 succeeded…

Make sure to set Debug mode.

Press F5 on your keyboard. 

to Release mode

Press F5 on your keyboard.

#### Copy

- \fltk-1.3.4-2-source and copy the Fl folder into your include folder (in my case): C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.10.25017\include

- \fltk-1.3.4-2-source\lib) and copy all 14 .lib files (Note that there are pairs of 2, one of them named with a “d” for debug in the end). And paste them into your Visual Studio 2017 lib folder (in my case: C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.10.25017\lib\x64andx86). 

-  Go to: the fluid folder in your fltk source folder \fltk-1.3.4-1-source\fltk-1.3.4-1\fluid and copy ``fluid.exe`` and ``fluidd.exe`` into your Visual Studio 2017 bin folder  C:\Program Files (x86)\Microsoft Visual Studio\2017\Community\VC\Tools\MSVC\14.10.25017\bin).

#### VS
open Visual Studio 2017 Community (or any other of course) and under the Visual C++ – Windows Desktop tab create a new Windows Desktop Wizard project. 向导。->empty

put file in project !!

add in header and source 

#### Set

- linker —> input ->the first
debug -> XXXd.lib
release XXX.lib

```
fltkd.lib
wsock32.lib
comctl32.lib
fltkjpegd.lib
fltkimagesd.lib
```

- linker->system->windows 

?? if Chinese wrong add uif8(no)

??if 11
Double click the error message in your errors tab, the x.H file will open in Visual Studio 2017 Community as a new tab, if not go to your External Dependencies folder in your Solution Explorer within Visual Studio and look for the file manually and open it. As you may notice already, the errors will be underlined in curly red.

Now go to line 28, between # include “Enumerations.H” on line 27 and # ifdef WIN32 and include # define WIN32 like this:

```
# include “Enumerations.H”
# define WIN32
# ifdef WIN32
```

#### Bjarne Stroustrup’s header and .cpp files.

At FLTK file
