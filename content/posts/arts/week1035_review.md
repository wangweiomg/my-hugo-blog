---
title: "Week1035_review"
date: 2023-03-31T15:45:46Z
draft: false 
toc: false
images:
categorys:
  - 每周学习
series:
tags:
  - arts 
---

# ARTS - Review 补2019.3.6

## [How To Learn Any New Programming Language Quickly](https://medium.com/better-programming/how-to-learn-any-new-programming-language-quickly-94996895669b)

如何快速学习一门新的编程语言？

* essential adj. 必要，必不可少
* fundamentals  n. 基调， 基本原理
* assumes 假设
* concepts 概念
* disheartening  adj. 令人犯愁的
* preconceived adj. 先入为主的
* Anatomy n 剖析 
* strip out 剔除
* aspect n. 方面，侧面，局面
* equivalent 

> ## An essential checklist of fundamentals
>
> 一个必不可少的原理清单

> This article assumes you already know at least one programming language; however, the concepts here will help you get started with programming.
>
> 本篇文章假设你已经至少懂得一门编程语言；这些概念将帮你开始编程。
>
> When I was in school, a teacher told me something I share with new developers: **The hardest programming language you’ll ever learn will be your second.**
>
> 当我还在学校时候，一个老师对我分享一个新手的开发者，最困难的编程语言是你将学的第二门语言。
>
> Don’t let this be disheartening — it means that when you first learn how to program, you have all of these preconceived ideas about programming. You end up making more syntax connections and assumptions than you should. Because of this, you have to “unlearn” these assumptions when you learn your second language. Try to keep this in mind if you’re attempting to learn your second language or even your third.
>
> 不要因此心烦 --- 它的意思是当你第一次学怎么编程，你将会对编程产生先入为主的观念。你最终会造成更多的假设和关联，而不是应该是什么样的。因此，你需要在开始学第二门语言前，忘记这些臆测。保持这种意识去学你的第二门语言甚至第三门。

#### Anatomy of Programming

编程剖析

> There are a lot of programming languages, [easily over 5,000](https://codelani.com/posts/how-many-programming-languages-are-there-in-the-world.html), but the [TIOBE index](https://www.tiobe.com/tiobe-index/) lists the top 250. All of the top 20 languages that aren’t unique cases will have similar standard libraries.
>
> 有很多编程语言，轻易超过5000， 但是TIOBE 单子收录前250.排名前20的语言除了独特部分都有类似的公共库。
>
> I think the best way to think about programming is to strip out all of the extra “stuff” so you’re left with only the necessities.
>
> 我认为对编程最重要的是去掉废话，留下必要的。

#### The Atoms

原子性

> Every aspect of every language can be reduced down to **true** and **false**. Why? Because electricity works this way — you either have a charge, or you don’t. Memory stores values in the form of **0**s and **1**s, and either this *bit* is charged, or it’s not.
>
> 每个编程语言局部都可以看做是 true 和 false.为什么呢？因为电就是这么工作的 -- 要么在通电， 要么不。内存存储的是由0或1组成的表，这些位要么通电，要么不是。
>
> Eight bits is equivalent to one *byte*, which is enough to list any character on the [ASCII table](http://www.asciitable.com/). The bits are flipped in such an order that it provides the **DEC**imal representation of the character. The computer understands how to translate this representation into a letter.
>
> 8位等于一字节，足够在ASCII表上表示所有字符。比特翻转就能得到这样顺序的10进制代表的字符。计算机知道如何把这种表示转为字母。

| Position |  8   | 7    | 6    | 5    | 4    | 3    | 2    | 1    |
| -------- | :--: | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| Value    | 128  | 64   | 32   | 16   | 8    | 4    | 2    | 1    |
| Bit      |  0   | 1    | 0    | 0    | 1    | 0    | 0    | 0    |

表示

| H    | e    | l    | l    | 0    | \0   |
| ---- | ---- | ---- | ---- | ---- | ---- |



> 64+8 = 72, ASCII 第72个就是 H

> ​	Having an understanding of this one concept, “the why,” will make the rest of this a lot easier on you.
>
> 理解了这个”为什么“的概念，将会让你更容易理解以下的内容。

### The Tools

工具

> The tools are all the same, and while they can be learned in any order, this is the order I usually take.
>
> 工具都一样，按任意顺序学习，以下是我常用的顺序。

#### Variables

> This seems simple enough, but seriously, how do you create a variable?
>
> 变量
>
> 这似乎很简单了，但是严肃说，你怎么创建一个变量？

#### Operator

> What are the operators, and how are they used? You can assume you have basic math operators, but what about logical operators? Is an “AND” operator spelled out as “and” or “AND,” or does it use symbols such as “&&?”
>
> 什么是操作符，他们如何用？你可以假定你有基本的数学操作符，但是关于逻辑操作呢？一个 AND操作符拼写为’and‘ 还是 ’AND‘ , 或者用 && ?

#### **Conditionals**

> Surprisingly, my most read articles for both Swift and Python have to do with decision making. The next thing you need to know is how you can make decisions in your program. Does the language you are trying to learn use the traditional “if/else if/else” or something more Pythonic such as “if/elif/else?” Does your language have a “switch” or “guard” statement?
>
> 条件
>
> 令人惊奇的是，我阅读的大多数Swift和Python文章都需要判断。你需要知道的另一个事就是在你程序中做决定。语言需要尝试用 if/else if/else 或更多的Python式的 if/elif/else 这样的形式吗？语言有switch 或guard 语句吗。



> ## **Loops**
>
> How can you loop through repetitive tasks? Does the language contain for-loops, while loops, do-while loops, or for-each statements?
>
> 循环
>
> 如何用重复工作循环？语言包含 for循环，while循环 do-while 循环或 for-each循环吗？
>
> 
>
> ## **Functions**
>
> Is it possible to create functions? If so, how do you do it? How do you include parameters in these functions? Knowing how to properly use functions will save you time and make your life so much easier.
>
> 方法
>
> 是否可以创建方法？如果是，如果做？方法中如何包含参数？知道如何使用方法将会节约时间使你的生活更便利。
>
> ## **Classes and structs**
>
> Does this language understand the concept of classes or structs? It sounds like a dumb question, but some languages don’t have either, or they have only one. If it does, how do you create a class or struct? Does the class require a constructor or init method?
>
> 类和结构
>
> 编程语言理解类和结构的概念吗？看似一个糊涂的问题，但是有点语言都没有，或者只有一个。如果是，如果创建class 或者struct? class 是否需要构造器或者初始化方法？
>
> ## **Error handling**
>
> Errors are inevitable. When they occur, does this language have a robust error handling solution and how do you use it? Is it “try/catch,” “try/except,” or something else? Are there other clauses such as “else” or “finally” that allow other options for errors?
>
> 错误处理
>
> 错误是必然的。当他们发生，语言是否有一种简装的解决方法，怎么使用？是 try/catch , try/except 或者其他？是否有其他的语句 如 else /finally 允许其他条件的错误？
>
> ## **Testing**
>
> How do you test your code? Is there a built-in library for testing or do you have to download a separate tool?
>
> All of these tools should be in most modern programming languages. Even the older languages such as COBOL have most of these, but they may be called something different, like paragraphs or copybooks.
>
> 测试
>
> 如何测试你的代码？这些在库里就建立了还是需要下载一个独立的工具？
>
> 所有的这些工具都应该包含在最流行的编程语言里。尽管老羽凡如COBOL 也有大多数，但是他们可能叫的不一样，想段落或复写簿。



> # Getting Good
>
> Once you understand these tools, the next thing you need to do is use them and write an application. You can know of a language by reading the docs, but you don’t know the language until you’ve written a few applications with it.
>
> 当你一旦理解了这些工具，下一步就是使用他们写一个应用。你可以通过阅读文档知道一门语言，但是当你使用它写很多应用后才会了解它。
>
> By writing an application, you’re forced to think like an *X* programmer. I can say that I know C++ because I took a class in C and read the docs on C++, but I really don’t know it until I’ve written an application in C++ using features specific to the language.
>
> 通过写一个应用，你必须强制像一个 X语言一样思考。我可以说我知道C++， 因为我上过C语言课读过C++文档，但是当我使用C++特性写过一个应用后才了解它。
>
> A good starter project is Blackjack. Blackjack requires variables, operators, conditionals, loops (based on the number of players), functions, classes/structs, and error handling. You can include test cases for potential failures, such as running out of cards.
>
> 一个好的启动项目就是扑克牌。扑克牌需要变量，操作，条件，循环(基于玩家数目)，方法，类/结构，和错误处理。你可以加入测试用例为可能的错误，例如超过牌数量。
>
> Other good starter projects might include Chutes and Ladders, Yahtzee, or a Slot Machine.
>
> 其他好的项目可能是 Chutes and Ladders, Yahtzee, or a Slot Machine.
>
> For something more advanced, try to recreate a game like Monopoly. Worry more about the mechanics and keep it text-based.
>
> 更先进的 ，尝试重新创造一个像Monopoly的游戏。多考虑机械，基于文本。
>
> It’s key to remember that if you short yourself on the difficulty of the task (such as skipping the double down or split features of blackjack), you’re only limiting your comprehension of the language.
>
> 记住，关键点是如果你降低任务难度（如跳过双击或分割扑克的特性）， 只会限制你对语言的理解。





> # What Else?
>
> I know that the list of things above isn’t everything that a language has to offer. The truth is you can write just about anything with the tools listed above, but the additional functionality included in standard libraries just make it easier. Most standard libraries include the same functions, and so you can rely on similar names between languages.
>
> 我知道以上列出来的并不是语言提供的所有东西。真实的是你可以上面提供的工具来写任何东西，但是标准库的附加功能只会让你更方便。很多标准库包含相同方法，所以你可以在多个语言中使用相似的名字。
>
> The more you work with a language, the more you can discover about the standard library, but be sure to learn the tools beforehand.
>
> 你和一个语言工作越多，越多会发现标准库，但是要先学工具。
>
> As you work with a language, try to figure out what its strengths and weaknesses are. These will help you understand which language to use for a particular problem.
>
> 学一门语言，要去找出哪里是优势，哪里是弱势。这将会帮你理解哪门语言适合解决哪类问题。
>
> Need to do some data science quickly? Look at a few Python packages or R. Need to write a fast service? Look at C or Go. How about a web server? Look at Java or Python.
>
> 需要快速做一些数据科学的事？看看一些Python包或者R. 需要些一个快速的服务？看看C或者Go.关于web服务器？看看Java或Python.
>
> I didn’t just know this by looking at the languages. I learned this by *using* these languages.
>
> 我看这些语言并不了解，我用这些语言之后才了解的。
>
> Since this is possibly my shortest article to date, I’m going to leave you with a challenge to yourself to learn a new language. Good luck!
>
> 当我这门短文结束时候，我将会给你留一个有挑战的任务，去学一门新语言，好运！

