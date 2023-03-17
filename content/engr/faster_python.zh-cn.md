---
title: "更快的 Python：Python 3.11 发布和未来路线图"
date: 2022-12-03T20:48:55-05:00
categories: ['工程','技术' ]
tags: ['技术', 'python', 'Cpython','编程', 'python3.11']
draft: false
---

> 本站的所有文章都默认为英文，中文版本由Google Translate 翻译。  
> 由于时间限制，并非所有文章都有中文版本。

Python 是一种流行的编程语言，尤其是在脚本、Web 开发、数据分析和 ML/AI 领域。

根据 TIOBE 编程语言索引，Python 已经成为 2022 年以来最流行的编程语言，超过了 C：
![programming language ranking](/se/programming_language_ranking.png "programming language ranking")


Python 因其平坦的学习曲线（与 C/C++ 等其他语言相比）、人类可读的代码和繁荣的生态系统而受到欢迎。 然而，开发人员总是抱怨 python 不比 JS 或 Lua 等其他解释语言快，甚至慢。

## Python 3.11
Python 3.11 今天（2022 年 12 月 3 日）发布，带来巨大的性能提升：

>Python 3.11 比 Python 3.10 快 10-60%。 平均而言，我们测得标准基准套件的速度提高了 1.25 倍

简而言之，Python 3.11 引入了 CPython 编译器和解释器的改进，因此速度更快。

如果您想了解更多细节，Guido van Rossum 和 Lex Fridman 进行了精彩的采访，讨论了 Python 3.11 为何如此之快：

{{< youtube TLhRuZ9cJWc >}}

## 高性能 Python 路线图
对于 python 社区来说，还有更多好消息。 Python 3.11 是一项重大改进，它不是全部，而是一个重要的里程碑。

有一个向高性能 python 演进的路线图：

>第一阶段——Python 3.10 3.10 的关键改进将是一个自适应的、专业的解释器。 解释器将在执行期间适应类型和值，利用程序中的类型稳定性，而不需要运行时代码生成。

>阶段 2 – Python 3.11 此阶段将对运行时和关键对象进行许多改进。 第二阶段将以大量“调整”为特征，而不是任何“标题”改进。 计划的改进包括： 改进小于一个机器字的整数的性能。 改进了二元运算符的性能。 通过更好地处理帧，更快的调用和返回。 更好的对象内存布局并减少内存管理开销。 零开销异常处理。 对解释器的进一步增强 其他小的增强。

>阶段 3 – Python 3.12（需要运行时代码生成）用于小区域的简单“JIT”编译器。 使用相对简单、快速的编译器编译专用代码的小区域。

>阶段 4 – Python 3.13（需要运行时代码生成）扩展编译区域。 增强编译器以生成高级机器代码。

## 结论
作为 Python 用户，我为 Python 社区发布 Python 3.11 感到高兴。 随着 Python3.12 和 Python3.13 引入 JIT 编译器，Python 将变得更快更好。