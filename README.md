网络流量未知协议识别
===
研究意义
---
网络协议的准确分类与识别是提高 **差异性服务质量保障、入侵检测、流量监控及计费管理** 等方面的基础。随着网络技术的不断革新，新型网络应用协议不断涌现，它们的协议规范属于公司专利或者技术秘密，没有公开可得到的协议规范文档，这给网络协议分类与识别带来了新的挑战。

背景知识
---
  - 协议：运行在网络中的设备和软件都遵守着一定的规则, 这些规则被称为协议。 **网络上传输的数据要按照一定的协议格式进行打包封装成数据包后才能在网络中正确地传输。** 由于网络中存在多种协议, 故需要分析网络中传输的数据包使用何种协议,即对数据包进行协议分析。  
  - 私有协议/未知协议：指的是协议规格不公开的协议，从民用上看包括QQ协议、Skype协议等社交网络协议，以及商业用途的通信协议和工业系统的工控协议等等。从军事上看，包括所有军事用途的对外不公开的协议。  
  - 网络协议由三个部分组成：语义、语法和时序。语义是解释控制信息每部分的意义，它规定了需要发出何种控制信息以及完成的动作与做出什么样的响应；语法是用户数据和控制信息的结构与格式以及数据出现的顺序；时序是对事件发生顺序的详细说明。  
  - 协议分析技术：协议分析技术是一个大的概念，这里面包括两个分支，一个是协议逆向工程，一个是流量分类与识别，即DPI，这两个技术是相辅相成的。  
  - 协议逆向工程： **协议逆向工程是指在不依赖协议描述的情况下，通过对协议实体的网络输入输出、系统行为和指令执行流程进行监控和分析，提取协议语法、语义和同步信息的自动化过程，是工程化的协议逆向分析方法。** 随着网络规模的扩大和应用种类的增多，对协议逆向的准时性和时效性要求越来越高，自动化协议逆向分析技术成为人们追求的目标。  
  - 网络流量协议的分类与识别（DPI）：DPI技术，Deep Packet Inspect，中译为深度包检测，其概念建立在精细分析数据包，力求从头到尾分析数据包，通过分析协议数据包内部，由此分析协议的相关属性。可以说，协议逆向工程中的最简单的部分实际上就是DPI技术。  
  
 协议逆向工程的概念
 ---
  - 三个阶段：以协议规范描述模型为目标，协议逆向分析系统应当包括 **输入预处理，协议格式提取，协议状态机推断** 三个阶段。
  - 输入预处理：协议逆向的原始输入为连续的网络数据流。要实现协议格式提取以及协议状态机推断。会话划分和报文定界是网络流量分析领域的重要研究方向。目前研究已较为成熟。
  - 协议格式提取：基于网络流量的协议格式逆向分析技术，也叫作基于报文的协议逆向分析技术，或者报文序列分析技术。即以截获的网络数据流作为分析对象，依据协议字段的取值变化频率和和特征推断得到协议格式。其依据是：数据流中的当个报文是协议格式的一个实例，相同格式的报文样本往往具有相似性，可以将具有相似性的报文汇集到一起，推断他们所遵循的报文格式。
  - 协议逆向工程的几个特点  
    1. 逆向过程无先验性，即不掌握协议规格
    2. 技术方法或者技术算法要求接近或者逼近自动化程度
    3. 分析对象有两种，一种是程序或者软件的指令序列，一种是网络轨迹的网络数据包或数据流
    4. 分析的目的包含字段格式，字段含义和协议状态机

协议逆向工程与未知协议类型识别的关系
---
  - 网络流量的数据包中除了有已知的协议外，还含有大量未知的协议。对未知协议进行聚类，然后虽然类别标签是未知的，但是能将属于同一未知类别的协议聚集到一起，从而可以对未知的这一类别做协议逆向分析，逆向分析得到的结果能更精确对该类协议报文进行检测。
  - 未知流量包含若干不同未知协议；同一未知协议包含不同的帧格式；同一帧格式包含若干不同语义。那么流量的未知协议识别就属于第一个工作。在对未知流量进行协议分类的时候，要保证精准率（即保证某一类非完备样本是同一未知协议）和召回率。追求精准率的话可以使用聚类分析方法，召回率的话就得对聚类后得结果进行协议逆向工程，从而更进一步细化分类结果
  - 如果要采用逆向分析技术追求召回率，那么就要分析三大逆向目标：字段格式、语义信息和状态机推演。关键地方在于如果能分析出字段格式等信息，那么这些特征又可以被未知协议流量检测利用，可以使得未知协议流量监测的先验信息增加，不确定性降低，进一步增强分类精度
  - 目前协议逆向工程根据分析对象的不同，可以分为基于报文序列分析和基于指令序列分析两大类。 **协议逆向工程与未知协议类型识别是反复迭代的一个过程，相辅相成、相互转化。** 

基于网络流量的协议分类技术
---
  - 技术背景：在成熟的方案体系中，一般有基于端口号，载荷，以及规则匹配的方法。后来发展出来了基于机器学习的流量分类技术。这些技术都有一个共同的特点就是技术由粗到细，传统分析协议数据包，不需要分析太多的数据包内容，只需要分析一小段即可确当数据包的属性内容，所以分析的做法较为粗糙。但是后来世界通信网的爆炸式发展，不仅因为因特网发展，还有各类WLAN、卫星、短波、工业互联网等等，对协议分析提出了更高的要求。
  - 技术目标：对不同或者未知协议进行同一应用类型的协议分类。其中很重要的一环就是如果要进行在线分析的话，网络流量的数据量是非常大的，因此要首先要对协议已知的流量进行剔除，然后再对未知协议的流量进行识别和检测或者进行聚类分析。
  - 方法与现状：
    - 基于标准端口匹配的协议识别方法：网络中多种应用层协议可以同时运行在同一台计算机的同一个 IP 地址上。由于 IP 地址与网络应用程序的关系是一对多的关系，所以主机需要通过端口号来区分不同的网络服务。传统上一直是基于端口映射机制对应用层协议进行识别。随着越来越多的网络协议不使用固定的端口进行通信，基于报文端口的协议识别受到很大限制，准确性受到很大挑战。但由于基于公知端口进行协议识别操作简单，识别速度快，现在的带宽越来越大，在识别 DNS、SMTP、POP3 等目前端口比较固定的传统协议时依然有一定的价值。
    - 基于DPI深度报文解析的协议识别方法  
      1）普通报文检测仅分析IP包4层以下的内容，包括源地址、目的地址、源端口、目的端口以及协议类型，也就是wireshark里面的五元组信息，而DPI 除了对前面的层次分析外，还增加了应用层分析，识别各种应用及其内容。  
      2）基于负载的协议识别，对应用层协议交互过程中产生报文的内容进行分析，找出不同于其它协议的模式特征，根据各协议特有的模式特征确定流量所属协议类型。基于负载的协议识别主要有采用固定字符串和正则表达式来表示协议特征两种方式。这也是现在最常用的方式，大部门应用可以利用这种方式识别。  
      3）实际例子，[基于DPI的新浪微博应用识别](https://blog.csdn.net/chaosju/article/details/39717039)  

针对未知流量协议应用层字节流的聚类实验方案
---
  - 数据集准备
    - 目前先收集主流的应用层协议数据集，即：dns、http、oicq、smtp、ssh。对聚类算法有用的信息主要是数据包里面的应用层协议数据单元，因此对其余的内容则可以作一个剔除处理。
    - 使用scapy包对pcap文件进行处理：第一步先用wireshark抓取主流的应用层协议数据包，包括dns、http、OICQ、smtp、ssh这五类典型的数据包；第二步再用scapy定位到与应用层协议直接相关的payload部分，从而屏蔽上层协议数据对最后聚类结果的影响，然后再在每个数据包的payload部分上做特征提取并进行聚类。目前可以使用scapy提取传输层下面的负载，这个负载包含了关于应用层协议里的所有内容。
  - 特征工程
    - 在进行提取的时候，要注意一个问题就是协议格式都是面向二进制设计的，也就是不同的位标记不同的属性，并规定哪些位是数据部分，因此特征工程的对象应该是二进制字节流，只有这样才不会丢失协议的格式信息，十六进制表示只是为了方便，但是如果对十六进制进行特征工程的话可能会丢失协议的格式信息。
    - 因此先将十六进制字节流转化为二进制字节流，然后将二进制字节流转化为灰度图像，并进行可视化分析，由于灰度图像存在局部纹理特征，因此可以用图像纹理特征挖掘算法得到图像的纹理特征向量，这个特征向量应该会在一定程度上表征了协议的格式信息。因此对于每一个样本都可以按照这个思路去生成对应的特征向量。
  - 算法建模
    - 采用density-peak算法进行聚类分析，这一聚类算法能很好得拟合多种分布类型的数据，并且不用设置初始的类别数目，因此能够进行自发性的聚类分析。

参考论文整理
---
[此博客介绍了主流的协议分类方法](https://blog.csdn.net/pnnngchg/article/details/79826949)






  
  



    



  



  







