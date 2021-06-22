
https://www.cnblogs.com/giggle/p/9583940.html
### hash 
是为构建计算的

是工程级别的，即每次修改任何一个文件，所有文件名的hash至都将改变


### chunkhash

 是为chunk 计算的
contenthash是ExtractTextPlugin中生成的特殊hash，他是根据提取内容而不是整个区块内容计算的。


### contenthash
contenthash是针对文件内容级别的，只有你自己模块的内容变了，那么hash值才改