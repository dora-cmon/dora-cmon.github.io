---
title: VS Code配置Latex
categories:
  - 环境搭建 - VS Code
tags:
  - VS Code
  - Latex
  - 配置
abbrlink: a7554b7c
cover: /images/cover/latex.jpg
date: 2020-03-05 11:07:19
---


# 安装

## 安装 texlive
打开[清华镜像网站](https://mirrors.tuna.tsinghua.edu.cn/CTAN/systems/texlive/Images/)，下载`texlive.iso`, 加载到虚拟光驱并以管理员方式运行`install-tl-windows.bat`，除安装路径自定义外，其余选项皆默认。（安装约30min）

## 安装插件：LaTeX Workshop

![latex_workshop](/images/VS-Code配置Latex/2020-02-21-22-52-17.png)

# 配置

按`Ctrl+,`进入配置界面并按右上角切换到`settings.json`文件，添加以下内容：

## 配置编译引擎程序

```json
"latex-workshop.latex.tools": [
    // 编译工具和命令
    {
        "name": "latexmk",
        "command": "latexmk",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "%DOCFILE%"
        ]
    },
    {
        "name": "xelatex",
        "command": "xelatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "%DOCFILE%"
        ]
    },
    {
        "name": "pdflatex",
        "command": "pdflatex",
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
            ]
        }
],
```

**将 tools 中的 `%DOC%` 替换成`%DOCFILE%`可以支持中文路径下的文件。**

## 配置编译链

```json
"latex-workshop.latex.recipes": [
    {
        "name": "XeLaTeX",
        "tools": [
            "xelatex"
        ]
    },
    {
        "name": "PDFLaTeX",
        "tools": [
            "pdflatex"
        ]
    },
    {
        "name": "latexmk",
        "tools": [
            "latexmk"
        ]
    },
    {
        "name": "BibTeX",
        "tools": [
            "bibtex"
        ]
    },
    {
        "name": "pdf -> bib -> pdf*2",
        "tools": [
            "pdflatex",
            "bibtex",
            "pdflatex",
            "pdflatex"
        ]
    },
    {
        "name": "xe -> bib -> xe*2",
        "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
        ]
    }
], 
```

## 配置临时文件清理方式

```json
"latex-workshop.latex.autoClean.run": "onBuilt",
```

设置需清理文件的后缀(**注意不要清理`.gz`文件，正反向搜索依赖该文件**)

```json
"latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk",
    ],
```

## 配置自动编译

```json
"latex-workshop.latex.autoBuild.run": "never",
```

编译完成后，自动打开PDF并跳转至光标所在行：

```json
"latex-workshop.synctex.afterBuild.enabled": true,
```

## 配置PDF浏览方式

```json
// external-外部浏览，tab-内部分栏，browser-浏览器
"latex-workshop.view.pdf.viewer": "external",
"latex-workshop.view.pdf.ref.viewer":"external",
// 当选择外部PDF浏览器时，安装SumatraPDF并设置以下两项
"latex-workshop.view.pdf.external.viewer.command": "C:/Program Files/SumatraPDF/SumatraPDF.exe",    //SumatraPDF.exe的实际路径
"latex-workshop.view.pdf.external.viewer.args": [
    "%PDF%",  
],
```

设置正向、反向跳转：

```json
"latex-workshop.view.pdf.external.synctex.command": "C:/Program Files/SumatraPDF/SumatraPDF.exe",  //SumatraPDF.exe的实际路径
"latex-workshop.view.pdf.external.synctex.args": [
    "-forward-search",
    "%TEX%",
    "%LINE%",
    "%PDF%",
    "-inverse-search",
    "\"C:/Program Files/Microsoft VS Code/bin/code.cmd\" -g \"%f\":\"%l\"", //修改为你的具体文件路径
],
```

# 配置文件内容汇总

```json
/* Latex Config */
"latex-workshop.latex.tools": [ 
    // 编译工具和命令
    {
        "name": "latexmk",         
        "command": "latexmk",         
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "%DOCFILE%"         
        ]     
    },     
    {         
        "name": "xelatex",         
        "command": "xelatex",         
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "-pdf",
            "%DOCFILE%"         
        ]     
    },     
    {         
        "name": "pdflatex",         
        "command": "pdflatex",         
        "args": [
            "-synctex=1",
            "-interaction=nonstopmode",
            "-file-line-error",
            "%DOCFILE%"
        ]
    },
    {
        "name": "bibtex",
        "command": "bibtex",
        "args": [
            "%DOCFILE%"
        ]
    }
],

"latex-workshop.latex.recipes": [
    {
        "name": "XeLaTeX",
        "tools": [
            "xelatex"
        ]
    },
    {
        "name": "PDFLaTeX",
        "tools": [
            "pdflatex"
        ]
    },
    {
        "name": "latexmk",
        "tools": [
            "latexmk"
        ]
    },
    {
        "name": "BibTeX",
        "tools": [
            "bibtex"
        ]
    },
    {
        "name": "pdf -> bib -> pdf*2",
        "tools": [
            "pdflatex",
            "bibtex",
            "pdflatex",
            "pdflatex"
        ]
    },
    {
        "name": "xe -> bib -> xe*2",
        "tools": [
            "xelatex",
            "bibtex",
            "xelatex",
            "xelatex"
        ]
    }
], 

"latex-workshop.latex.autoClean.run": "onBuilt",
"latex-workshop.latex.clean.fileTypes": [
    "*.aux",
    "*.bbl",
    "*.blg",
    "*.idx",
    "*.ind",
    "*.lof",
    "*.lot",
    "*.out",
    "*.toc",
    "*.acn",
    "*.acr",
    "*.alg",
    "*.glg",
    "*.glo",
    "*.gls",
    "*.ist",
    "*.fls",
    "*.log",
    "*.fdb_latexmk",
],

"latex-workshop.latex.autoBuild.run": "never",
"latex-workshop.synctex.afterBuild.enabled": true,

"latex-workshop.view.pdf.viewer": "external",
"latex-workshop.view.pdf.ref.viewer":"external",
"latex-workshop.view.pdf.external.viewer.command": "C:/Program Files/SumatraPDF/SumatraPDF.exe",    //SumatraPDF.exe的实际路径
"latex-workshop.view.pdf.external.viewer.args": [
    "%PDF%",  
],
"latex-workshop.view.pdf.external.synctex.command": "C:/Program Files/SumatraPDF/SumatraPDF.exe",  //SumatraPDF.exe的实际路径
"latex-workshop.view.pdf.external.synctex.args": [
    "-forward-search",
    "%TEX%",
    "%LINE%",
    "%PDF%",
    "-inverse-search",
    "\"C:/Program Files/Microsoft VS Code/bin/code.cmd\" -g \"%f\":\"%l\"", //修改为你的具体文件路径
],
```

# 更多

更多VS Code相关配置见[环境搭建 - VS Code](/categories/环境搭建-VS-Code/)

# 参考资料

[Win10 安装 TeXLive 2018 - 简书](https://www.jianshu.com/p/8f8da0517a31)

[使用VSCode编写LaTeX - 知乎](https://zhuanlan.zhihu.com/p/38178015)

[Synctex inverse search doesn&#39;t work half the time (and How I got forward search to work for SumatraPDF) · Issue #637 · James-Yu/LaTeX-Workshop · GitHub](https://github.com/James-Yu/LaTeX-Workshop/issues/637)
