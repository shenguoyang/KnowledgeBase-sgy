<font style="color:rgb(34, 34, 34) !important;">今天同事阿杰兄发现内部一台mysql测试服务器乱码，以前也记录过关于字符集的，今天再补充下</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">修改mysql的字符集和默认存储引擎  
</font>[http://blog.csdn.net/wyzxg/article/details/7581415](http://blog.csdn.net/wyzxg/article/details/7581415)

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(0, 102, 0);">查看库现有的字符集：  
</font><font style="color:rgb(34, 34, 34) !important;">mysql> show variables like '%char%';  
</font><font style="color:rgb(34, 34, 34) !important;">+--------------------------+------------------------------+  
</font><font style="color:rgb(34, 34, 34) !important;">| Variable_name            | Value                        |  
</font><font style="color:rgb(34, 34, 34) !important;">+--------------------------+------------------------------+  
</font><font style="color:rgb(34, 34, 34) !important;">| character_set_client     | latin1                       |  
</font><font style="color:rgb(34, 34, 34) !important;">| character_set_connection | latin1                       |  
</font><font style="color:rgb(34, 34, 34) !important;">| character_set_database   | latin1                       |  
</font><font style="color:rgb(34, 34, 34) !important;">| character_set_filesystem | binary                       |  
</font><font style="color:rgb(34, 34, 34) !important;">| character_set_results    | latin1                       |  
</font><font style="color:rgb(34, 34, 34) !important;">| character_set_server     | latin1                       |  
</font><font style="color:rgb(34, 34, 34) !important;">| character_set_system     | utf8                         |  
</font><font style="color:rgb(34, 34, 34) !important;">| character_sets_dir       | /mysql/share/mysql/charsets/ |</font>

<font style="color:rgb(34, 34, 34) !important;">  
</font><font style="color:rgb(0, 102, 0);">mysql和字符集有关的变量  
</font><font style="color:rgb(34, 34, 34) !important;">character_set_client：客户端请求数据的字符集  
</font><font style="color:rgb(34, 34, 34) !important;">character_set_connection：从客户端接收到数据，然后传输的字符集  
</font><font style="color:rgb(34, 34, 34) !important;">character_set_database：默认数据库的字符集，无论默认数据库如何改变，都是这个字符集；如果没有默认数据库，那就使用 character_set_server指定的字符集，</font>

<font style="color:rgb(34, 34, 34) !important;">character_set_results：结果集的字符集</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">这个变量建议由系统自己管理，不要人为定义。  
</font><font style="color:rgb(34, 34, 34) !important;">character_set_filesystem， 默认binary是不做任何转换的  
</font><font style="color:rgb(34, 34, 34) !important;">character_set_server：数据库服务器的默认字符集  
</font><font style="color:rgb(34, 34, 34) !important;">character_set_system：这个值总是utf8，不需要设置，是为存储系统元数据的字符集</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(0, 102, 0);">一个完整的用户请求的字符集转换流程是  
</font><font style="color:rgb(34, 34, 34) !important;">   1) mysql Server收到请求时将请求数据从character_set_client转换为character_set_connection  
</font><font style="color:rgb(34, 34, 34) !important;">2) 进行内部操作前将请求数据从character_set_connection转换为内部操作字符集,步骤如下  
</font><font style="color:rgb(34, 34, 34) !important;">A. 使用每个数据字段的CHARACTER SET设定值；  
</font><font style="color:rgb(34, 34, 34) !important;">B. 若上述值不存在，则使用对应数据表的字符集设定值  
</font><font style="color:rgb(34, 34, 34) !important;">C. 若上述值不存在，则使用对应数据库的字符集设定值；  
</font><font style="color:rgb(34, 34, 34) !important;">D. 若上述值不存在，则使用character_set_server设定值。  
</font><font style="color:rgb(34, 34, 34) !important;">3) 最后将操作结果从内部操作字符集转换为character_set_results</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">在 MySQL 中，有一些字符集变量是全局的，而另一些是针对每个客户端连接的。下面是这些变量的解释：</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">全局变量：</font>

<font style="color:rgb(34, 34, 34) !important;">1. character_set_server：全局服务器字符集设置，表示 MySQL 服务器所使用的字符集。它影响到所有连接到该服务器的客户端连接。</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">2. character_set_database：全局数据库字符集设置，表示创建新数据库时使用的默认字符集。当创建新数据库时，如果没有显式指定字符集，将使用该变量的值。</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">客户端连接变量：</font>

<font style="color:rgb(34, 34, 34) !important;">1. character_set_client：客户端字符集设置，表示客户端应用程序所使用的字符集。它指定客户端应用程序发送给服务器的字符集。</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">2. character_set_connection：连接字符集设置，表示客户端连接使用的字符集。它指定服务器与客户端之间进行数据传输和通信时所使用的字符集。</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">3. character_set_results：结果字符集设置，表示从服务器返回的结果集所使用的字符集。它影响到查询结果集的字符集编码。</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">这些变量的作用范围如下：</font>

<font style="color:rgb(34, 34, 34) !important;">- 全局变量是服务器级别的设置，对所有连接到该服务器的客户端生效。</font>

<font style="color:rgb(34, 34, 34) !important;">- 客户端连接变量是针对每个客户端连接进行设置的，每个客户端连接都可以有不同的设置。</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">请注意，全局变量的值对于新建的客户端连接来说是默认值。当客户端连接与服务器建立时，服务器会将全局变量的值复制到客户端连接变量中。然后，客户端可以根据需要对连接变量进行修改。</font>

<font style="color:rgb(34, 34, 34) !important;"></font>

<font style="color:rgb(34, 34, 34) !important;">因此，在 MySQL 中，可以根据需求设置全局变量和客户端连接变量，以适应不同的字符集需求和处理要求。每个变量的作用和范围都有所不同，根据具体情况进行设置。</font>

