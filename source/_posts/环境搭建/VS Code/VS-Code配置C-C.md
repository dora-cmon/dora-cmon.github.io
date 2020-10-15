---
title: VS Code配置C/C++
categories:
  - 环境搭建 - VS Code
tags:
  - VS Code
  - C/C++
  - 配置
abbrlink: e3726273
cover: /images/cover/VSCode_Cplusplus.jpg
date: 2020-02-19 01:27:27
---


# 安装

- 下载并安装MinGW-w64

  推荐使用离线方式安装，因网络原因，在线安装大概率失败。

  - 在线安装包

    进入[MinGW-w64](https://sourceforge.net/projects/mingw-w64/files/)页面下载并安装`MinGW-w64`。

    ![1582037372703](/images/VS-Code配置C-C/1582037372703.png)

  - 离线压缩包

    若在线安装因为网络原因失败，可下载[压缩包](https://sourceforge.net/projects/mingw-w64/files/)。往下稍微翻一下，选最新版本中的`x86_64-posix-seh`。下载完成后解压到合适的位置，如`C:\Program Files\mingw64`，然后手动添加环境变量：

    ![1582043841169](/images/VS-Code配置C-C/1582043841169.png)
  
- 下载插件：C/C++ 插件

  ![1582035915993](/images/VS-Code配置C-C/1582035915993.png)

# 创建配置文件

在项目工程目录下创建`.vscode`文件，分别创建三个文件：

- `c_cpp_properties.json`，将文件中的目录替换成你的MinGW安装目录：

  ```json
  {
      "configurations": [
          {
              "name": "Win32",
              "includePath": [
                  "${workspaceRoot}",
                  "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++",
                  "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32",
                  "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward",
                  "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include",
                  "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/tr1",
                  "C:/Program Files/mingw64/x86_64-w64-mingw32/include"
              ],
              "defines": [
                  "_DEBUG",
                  "UNICODE",
                  "__GNUC__=6",
                  "__cdecl=__attribute__((__cdecl__))"
              ],
              "intelliSenseMode": "msvc-x64",
              "browse": {
                  "path": [
                      "${workspaceRoot}",
                      "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++",
                      "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/x86_64-w64-mingw32",
                      "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/backward",
                      "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include",
                      "C:/Program Files/mingw64/lib/gcc/x86_64-w64-mingw32/8.1.0/include/c++/tr1",
                      "C:/Program Files/mingw64/x86_64-w64-mingw32/include"
                  ],
                  "limitSymbolsToIncludedHeaders": true,
                  "databaseFilename": ""
              },
              "compilerPath": "C:/Program Files (x86)/mingw64/bin/gcc.exe",
              "cStandard": "c11",
              "cppStandard": "c++17"
          }
      ],
      "version": 4
  }
  ```

- `launch.json`，将`miDebuggerPath`替换成你的MinGW安装目录：

  ```json
  {
      "version": "0.2.0",
      "configurations": [
          {
              "name": "C++ Launch (GDB)",                
              "type": "cppdbg",                         
              "request": "launch",                        
              "targetArchitecture": "x86",                
              "program": "${workspaceRoot}\\${fileBasename}.exe",                 
              "miDebuggerPath":"C:/Program Files/mingw64/bin/gdb.exe", 
              "args": [],     
              "stopAtEntry": false,                  
              "cwd": "${workspaceRoot}",                  
              "externalConsole": true,                  
              "preLaunchTask": "g++"　　                  
              }
      ]
   }
  ```

- `tasks.json`：

  ```json
  {
      "version": "2.0.0",
      "command": "g++",
      "args": ["-g","-std=c++11","${file}","-o","${workspaceRoot}\\${fileBasename}.exe"],
      "problemMatcher": {
          "owner": "cpp",
          "fileLocation": ["relative", "${workspaceRoot}"],
          "pattern": {
              "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning|error):\\s+(.*)$",
              "file": 1,
              "line": 2,
              "column": 3,
              "severity": 4,
              "message": 5
          }
      }
  }
  ```

# 使用

按`F5`或左侧工具栏第四个Debug按钮中的绿色运行按钮即可运行，若有断点则为debug。

# （可选）安装 Code Runner插件

Code Runner 可以免除以上三个配置文件的创建，能很方便地运行代码，但是**无法调试**。如果无需调试，可以选择安装此插件：

![1582035983293](/images/VS-Code配置C-C/1582035983293.png)

按`ctrl+,`进入设置界面，点击右上角切换到`Config`文件，添加以下内容：

![1582035717006](/images/VS-Code配置C-C/1582035717006.png)

```json
/* Code Runner Config*/
"code-runner.clearPreviousOutput": true,    // 运行时清除输出
"code-runner.saveAllFilesBeforeRun": true,  // 运行前保存所有文件
"code-runner.runInTerminal": true,          // 在terminal中运行，从而可以输入，且不会乱码
```

完成后，点击右上角三角按钮或右键`Run Code`即可运行代码。

# 更多

更多VS Code相关配置见[环境搭建 - VS Code](/categories/环境搭建-VS-Code/)

# 参考资料

[Visual Studio Code 如何编写运行 C、C++ 程序？ - 知乎](https://www.zhihu.com/question/30315894)

[VScode 调试 c++_C/C++_yDarkin-CSDN博客](https://blog.csdn.net/qq_38413498/article/details/88747546)