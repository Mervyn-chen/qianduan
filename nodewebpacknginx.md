# 构建工具-Gulp知识点

​    Gulp 和 Grunt 同为前端自动化构建工具， 但在工作流程和效率等方面存在很大的差异，下面从工作流程、使用方式、效率和插件的重用性等多个方面进行比较分析。
（1）工作流程
Grunt 的工作流程：重复着读文件、修改文件、写文件的流程。使用 Grunt 的 I/O 过程中会产生一些中间态的临时文件，一些任务生成临时文件，其它任务可能会基于临时文
件再做处理并生成最终的构建后文件。
Gulp 的工作流程：从文件流到文件流。使用 Gulp 的优势就是利用流的方式进行文件的处理，通过管道将多个任务和操作连接起来，因此只有一次 I/O 的过程，流程更清晰，
更纯粹。
（2）使用方式：Grunt 是采用配置的策略处理任务，而Gulp 采用代码优于配置的策略，使得复杂的任务便于管理，更易于维护。
（3）效率：因为 Grunt 操作会创建临时文件，会有频繁的 I/O 操作，而 Gulp 使用的是流操作，不需要往磁盘中写中间文件，一直是在内存中处理，直到输出结果，可以更快地完成构建。因此在效率上 Gulp 远胜 Grunt。

# 构建工具-webpack

​    说白了就是webpack = module bundler(加载器)，对于webpack来说，所有的文件都是模块，只是处理的方式不一样罢了

# 构建工具-Grunt

​    gulp作用
​        Sass、Less编译
​        CSS JS 图片压缩
​        CSS JS合并
​        CSS JS 内联
​        自动刷新