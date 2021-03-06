## 功能
1. 支持断点下载；
2. 支持多个文件并行下载；
3. 支持自定义并行下载的文件数，不像chrome那样限制只能并行下载很少的文件（我的chrome限制6个并行文件下载）。


## 使用方法
1. 配置`files_to_download.txt`，每一行的格式是`url 文件名`，url和文件名由空格隔开，文件名可以包含空格；
2. 如果有需要自定义下载文件存放的目录、配置文件名、最大并发数的，请自行修改`dowload.py`里响应的变量值；
3. 直接运行`download.py`，`python dowload.py`。


## 代码编写
原始代码请参考博客：https://blog.csdn.net/qq_35203425/article/details/80987880 。

我在这篇博客的基础上优化了以下几点：
1. 增加进程池，方便多文件并行下载；
2. 增加从配置文件读取url和文件名，而不是硬编码在代码里；
3. 完善下载进度、下载速度的计算和显示；
4. 增加重复下载的检测逻辑，如果已经下载好了，就应该退出不再下载；
5. 调整代码结构、注释和部分变量名，使得整体逻辑比较清晰。


## 实现原理
我理解的原理是这样子的：
1. 通过http请求响应header中的`Content-Length`字段可以知道整个文件的大小；
2. 通过http请求header中的`Range`字段可以设置下载文件的起点，比如`Range: bytes=233-`表示从位置233开始下载（也就是第234个字节，注意是从0开始计数的）；
3. 通过`os.path.getsize`可以获取到本地文件的大小；
4. 综上，获取本地文件大小，然后指定从断点开始下载文件，然后还可以获取整个文件的大小来计算当前下载进度。
5. 下载速度的计算也很容易，就是下载的字节数除以时间区间长度。


## 依赖
依赖一个python的第三方库：`requests`，其他都是pyhton的标准库。
