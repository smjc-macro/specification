# Specificaiton of general source code

本文规定了 SAS 程序文件的一般规范。

## 程序头部

程序头部是一段顶级注释块（_top-level comment block_），包含以下信息：

- [] 待补充

## 前置准备

包含一些初始化操作，例如：

- 重置系统自动宏变量 SYSCC
- 清空 WORK 逻辑库中的数据集
- 清空日志、输出结果、ODS 结果
- 导入宏程序

```sas
%let syscc=0;
proc datasets lib=work kill memtype=data nolist;
quit;
dm ' log; clear; output; clear; odsresult; clear; ';
```

## 程序主体

程序主体包含生成分析结果的程序代码，这些代码会被递交至监管机构。

对于生成表格的程序文件，程序主体的最终输出应是一个数据集，它可以直接被传送至 ODS 处理，进一步输出为外部文件。

对于生成图形的程序文件，程序主体的最终输出应是一个 HTML ，它直接显示在结果查看器中。

> [!IMPORTANT]
> 程序主体应使用注释 `/* SUBMIT START */` 和 `/* SUBMIT END */` 包围，便于自动化提取应当被递交至监管机构的代码片段。

> [!CAUTION]
> 程序主体应当是一个可独立运行的代码片段，换句话说，程序主体只能使用 `setup.sas` 定义的逻辑库，且不能使用在其外部定义的宏变量。

## 程序尾部

程序尾部包括：

1. 日志输出

   ```sas
   %SM_LOG;
   ```

2. RTF 输出

   ```sas
   proc report;
       ...
   run;
   ```

> [!IMPORTANT]
> 如果你的程序主体和 RTF 输出部分在同一个宏程序中定义，请在宏程序内部使用注释 `/* SUBMIT START */` 和 `/* SUBMIT END */` 定义需要被递交（或不递交）至监管机构的代码片段。

3. 错误检测

   ```sas
   %ERROR;
   ```
