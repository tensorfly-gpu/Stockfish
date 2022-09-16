## 总览

[![Build Status](https://github.com/official-stockfish/Stockfish/actions/workflows/stockfish.yml/badge.svg)](https://github.com/official-stockfish/Stockfish/actions)

[Stockfish](https://stockfishchess.org) 是一个从Glaurung 2.1衍生的免费、强大的UCI国际象棋引擎。Stockfish不是一个完整的国际象棋程序，需要一个与UCI兼容的图形用户界面（GUI）（例如，XBoard with PolyGlot, Scid, Cute Chess, eboard, Arena, Sigma Chess, Shredder, Chess Partner 或 Fritz） 才能轻松使用。阅读所选GUI的文档，了解如何在其中使用Stockfish引擎。

Stockfish引擎具有两个国际象棋评估函数。基于高效可更新神经网络（NNUE）的评估是默认的，也是迄今为止最强的。基于手工术语的经典评估仍然可用。最强的网络集成在二进制文件中，并在构建过程中自动下载。NNUE评估受益于大多数CPU（sse2、avx2、neon或类似处理器）上可用的向量内部函数。

## 文件

Stockfish的发行版由以下文件组成：

  * [README.md](https://github.com/official-stockfish/Stockfish/blob/master/README.md),您当前正在读取的文件。

  * [Copying.txt](https://github.com/official-stockfish/Stockfish/blob/master/Copying.txt),一个包含GNU通用公共许可证版本3的文本文件。

  * [AUTHORS](https://github.com/official-stockfish/Stockfish/blob/master/AUTHORS),包含项目作者列表的文本文件

  * [src](https://github.com/official-stockfish/Stockfish/tree/master/src),包含完整源代码的子目录，包括可用于在类Unix系统上编译Stockfish的Makefile。

  * 扩展名为.nnue的文件，用于存储神经网络以进行nnue评估。二进制发行版将嵌入此文件。

## UCI协议和可用选项

通用国际象棋界面（UCI）是用于与国际象棋引擎通信的标准协议，是典型图形用户界面（GUI）或国际象棋工具的推荐通信方式。Stockfish实现了[UCI协议](https://www.shredderchess.com/download/div/uci.zip)中描述的大部分选项。

开发人员可以通过在终端中键入`./stockfish uci` 来查看Stockfish中可用的UCI选项的默认值，但大多数用户通常会看到这些值，并通过国际象棋GUI进行更改。以下是Stockfish中可用的UCI选项列表：

  * #### Threads
    用于搜索位置的CPU线程数。要获得最佳性能，请将其设置为可用CPU核数。

  * #### Hash
    哈希表的大小（MB）。建议在设置线程后设置哈希。

  * #### Clear Hash
    清除哈希表。

  * #### Ponder
    让Stockfish在对手思考时思考下一步行动。

  * #### MultiPV
    搜索时输出N条最佳线（主要变量、PV）。保持1以获得最佳性能。

  * #### Use NNUE
    在NNUE和经典评估函数之间切换。如果设置为"true"，则网络参数必须可从文件加载(参见EvalFile)，如果它们没有嵌入到二进制文件中。

  * #### EvalFile
    NNUE评估参数的文件名。根据GUI，文件名可能必须包括包含文件的文件夹/目录的完整路径。还搜索其他位置，如包含二进制文件的目录和工作目录。

  * #### UCI_AnalyseMode
    由你的GUI处理的选项。

  * #### UCI_Chess960
    由GUI处理的选项。如果这是真的，Stockfish将玩960号棋子。

  * #### UCI_ShowWDL
    如果启用，将显示近似的WDL统计信息作为引擎输出的一部分。这些WDL数字模拟了给定评估的预期游戏结果，以及在fishtest LTC条件下(每场游戏60+0.6秒)引擎自玩的游戏厚度。

  * #### UCI_LimitStrength
    启用较弱的发挥，旨在Elo评级设置为UCI_Elo。此选项将覆盖技能级别。

  * #### UCI_Elo
    如果启用了UCI_LimitStrength，目标是给定Elo的引擎强度。该Elo评级已在60s+0.6s的时间控制下校准，并锚定在CCRL 40/4上。

  * #### Skill Level
    降低技能等级以使Stockfish打得更弱(参见UCI_LimitStrength)。在内部，MultiPV是启用的，根据技能等级的不同，有一定的概率会打出一个较弱的招式。

  * #### SyzygyPath
    存储Syzygy表基文件的文件夹/目录的路径。多个目录在Windows上用“;”分隔，在基于unix的操作系统上用“:”分隔。不要在“;”或“:”周围使用空格。

    例如：`C:\tablebases\wdl345;C:\tablebases\wdl6;D:\tablebases\dtz345;D:\tablebases\dtz6`

    建议将。rtbw文件存储在SSD硬盘上。在普通的硬盘上存储.rtbz文件不会有任何损失。建议检查下载的tablebase文件的所有md5校验和(md5sum -c checksum.md5)，因为损坏会导致引擎崩溃。

  * #### SyzygyProbeDepth
    探测位置的最小剩余搜索深度。将此选项设置为较高的值，以便在由于表基探测而遇到太大的减速(以nps计算)时不那么积极地进行探测。

  * #### Syzygy50MoveRule
    禁用使Syzygy表基探测探测到的50个移动规则绘制计数为赢或输。这对ICCF通信游戏很有用。

  * #### SyzygyProbeLimit
    限制Syzygy表基的探测位置，最多只剩下这么多棋子(包括国王和兵)。

  * #### Move Overhead
    假设由于网络和GUI开销，时间延迟为x毫秒。在这些情况下，这有助于避免准时损失。

  * #### Slow Mover
    较低的数值会让Stockfish花在游戏中的时间更短，而较高的数值则会让它思考的时间更长。

  * #### nodestime
    告诉引擎使用搜索的节点而不是标准时间来解释运行时间。用于引擎测试。

  * #### Debug Log File
    将所有与引擎之间的通信写入文本文件。

对于开发人员来说，以下非标准命令可能会感兴趣，主要用于调试： 

  * #### bench *ttSize threads limit fenFile limitType evalType*
    使用各种选项执行标准基准测试。使用所有默认值获得版本(标准节点数)的签名。`bench` is currently`bench 16 1 13 default depth mixed`。

  * #### compiler
    给出用于构建二进制文件的编译器和环境的信息。

  * #### d
    显示当前位置，用ascii art和fen。

  * #### eval
    返回当前位置的估值。

  * #### export_net [filename]
    将当前加载的网络导出到文件。如果当前加载的网络是嵌入式网络，且未指定文件名，则网络将被保存到与嵌入式网络名称匹配的文件中，如evaluate.h中定义的那样。如果当前加载的网络不是嵌入式网络(通过UCI设置设置的某个网络)，则需要filename参数，并将网络保存到该文件中。

  * #### flip
    翻转侧面以移动。

## 关于经典评价与NNUE评价的注释

这两种方法都为一个位置赋值，在alpha-beta (PVS)搜索中用于寻找最佳移动。经典的评估计算这个值作为各种象棋概念的函数，由专家手工制作，使用fishtest进行测试和调整。NNUE评估使用基于基本输入的神经网络计算该值(例如，仅棋子位置)。该网络经过优化和训练，以适当的搜索深度评估数百万个位置。

NNUE评估首先在shogi引入，随后移植到Stockfish。它可以在cpu上高效地进行评估，并利用了在一个典型的棋局之后只需要更新部分神经网络的事实。nodchip存储库提供了培训和开发NNUE网络所需工具的第一个版本。今天，nnue-pytorch存储库中提供了更高级的培训工具，而数据生成工具则在专门的分支中提供。

在支持现代向量指令(avx2和类似指令)的cpu上，NNUE评估会产生更强的播放强度，即使引擎计算的每秒节点数略低(典型情况下约为80%的nps)。

注:

NNUE计算依赖于Stockfish二进制文件和网络参数文件(参见EvalFile UCI选项)。不是每个参数文件都与给定的Stockfish二进制文件兼容，但是EvalFile UCI选项的默认值是保证与该二进制文件兼容的网络的名称。

要使用NNUE评估，需要提供附加的神经网络参数数据文件。通常，该文件已经嵌入到二进制文件中，或者可以下载。默认(推荐的)网络的文件名可以作为EvalFile UCI选项的默认值，格式为nn-[SHA256前12位]。Nnue(例如，nn-c157e0a5755b.nnue)。此文件可从https://tests.stockfishchess.org/api/nn/filename 下载。根据需要替换[filename]。

## 从Syzygy表基中可以期待什么?

如果引擎正在搜索一个不在表基中的位置(例如有棋子8的位置)，它将在搜索过程中访问表基。如果引擎报告一个非常大的分数(通常是153.xx)，这意味着它找到了进入表基位置的制胜线。

如果给了引擎一个在表库中的搜索位置，它将在搜索开始时使用表库来预选所有好的招式，即所有能够保持胜利或保持平局的招式，同时考虑50步规则。然后它将只对这些移动执行搜索。引擎不会立即移动，除非只有一个好的移动。引擎可能不会报告同伴分数，即使已知位置已经赢得。

因此，很明显，这种行为与人们使用Nalimov表基时所习惯的不一样。造成这种差异有技术原因，主要技术原因是Nalimov表基使用DTM度量(距离-配对)，而Syzygy表基使用DTZ度量的变体(距离-零，零意味着重置50步计数器的任何移动)。这一特殊指标也是为什么Syzygy表基比Nalimov表基更紧凑的原因之一，同时它仍然能够存储最佳玩法所需的所有信息，并且能够考虑到50步规则。

## 自己从源代码编译Stockfish

Stockfish支持32或64位cpu、某些硬件指令、大端计算机(如Power PC)和其他平台。

在类unix系统上，直接从src文件夹中包含的Makefile的源代码编译Stockfish应该很容易。一般情况下，建议运行make help以查看带有相应描述的make目标列表。

```
    cd src
    make help
    make net
    make build ARCH=x86-64-modern
```

当不使用Makefile进行编译时(例如，使用Microsoft MSVC)，您需要在编译器命令行中手动设置/取消设置一些开关;参见file types.h获得快速参考。

当报告一个问题或一个bug时，请告诉我们您使用哪个Stockfish版本和哪个编译器创建您的可执行文件。可以通过在控制台中输入以下命令找到此信息:

```
    ./stockfish compiler
```

## 了解代码库并参与项目

Stockfish在过去十年中的进步是社区的巨大努力。有一些方法可以帮助它的发展。

### 捐赠硬件

改进Stockfish需要大量的测试。您可以通过安装Fishtest Worker来捐赠硬件资源，并查看Fishtest上的当前测试。

### 提高代码

如果你想帮助改进代码，这里有一些有价值的资源:

* [In this wiki,](https://www.chessprogramming.org) many techniques used in
Stockfish中使用的许多技术都通过大量的背景信息进行了解释。

* [The section on Stockfish](https://www.chessprogramming.org/Stockfish)
描述了Stockfish使用的许多特性和技术。然而，它是通用的，而不是专注于Stockfish的精确实现。然而，这是一个有用的资源。

* The latest source can always be found on [GitHub](https://github.com/official-stockfish/Stockfish).
最新的源代码可以在GitHub上找到。最近关于Stockfish的讨论主要发生在FishCooking组和Stockfish Discord频道上。发动机测试是在Fishtest上完成的。如果您想帮助改进Stockfish，请先阅读本指南，其中解释了Stockfish开发的基础知识。


## 使用条款

Stockfish是免费的，并且在GNU通用公共许可证版本3 (GPL v3)下发布。从本质上说，这意味着您几乎完全可以自由地对程序做任何您想做的事情，包括在您的朋友中分发它，从您的网站上下载它，出售它(单独或作为更大的软件包的一部分)，或将它作为您自己的软件项目的起点。

唯一真正的限制是，无论何时以某种方式发布Stockfish，都必须始终包含许可证和完整的源代码(或指向可以找到源代码的位置的指针)，以生成要发布的确切二进制文件。如果您对源代码做了任何更改，这些更改也必须在GPL v3下可用。

有关详细信息，请阅读GPL v3的副本，该副本可以在名为copy .txt的文件中找到。
