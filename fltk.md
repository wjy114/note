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

At FF

#### can't open pdb file

dll->pdb符号文件

http://c.biancheng.net/view/474.html
1) 选择菜单栏中的“调试  --> 选项”，
2) 弹出“选项”对话框后，选择“调试 --> 常规”，在右侧选项栏中勾选“启用源服务器支持”（包含的 3 个子选项不用勾选），此时会弹出一个安全警报框，选择“是”
3) 还是在“选项”对话框中，选择“调试 --> 符号”，在右侧选项栏中勾选“Microsoft符号服务器”，此时会弹出一个提示对话框，点击“确定”即可。同时，对于缓存符号的目录，选择C:\Users
4) 确定之后，重新运行你的程序，首次运行时，由于编译器会加载所有动态链接库的pdb文件，可能会等到几秒钟。程序运行后，之前输出窗口中的“无法查找或打开pdb文件”的提示不见了

