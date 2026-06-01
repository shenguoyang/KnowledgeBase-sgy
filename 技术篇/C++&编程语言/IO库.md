---
tags:
  - seed
created: 2026-05-30
updated: 2026-05-30
topic: tech
---

# C++标准IO库
# 1.输入输出流 
头文件<iostream>

cout ：标准输出流 输出到屏幕，能文件重定向   wcin 宽字节版本对象

Cerr ：标准错误输出，不能文件重定向

clog ：标准错误输出，可重定向

cin   ： 标准输入

2.流对象常用处理函数

put（）输出字符          get（）输入字符

write（）输出字符串    getline（）输入字符串

3.流控制字符

头文件 <iomanip>

boolalpha: bool以true or false打印

setbase（n） 整数以n进制输出  n为8，10，16

setfill(n) 设置填充字符

setprecision（）设置有效位数

```cpp
void testIO()
{
    cout<<"请输入一个字符串"
    char str[20] = "";
    cin.getline(str,20);
    cout.write(str,20);
    
    cout<<"请输入一个字符"<<endl;
    int value = 0;
    cin.get(value);
    cout.put(value);
}

void testIOType()
{
    bool num = 1;
    cout<< boolalpha<<num<<endl;
    cout<< setbase(8)<<32<<endl;
    cout<<setprecision(4)<<endl;
    cout<<fixed<<setprecision(4)<<endl;设置小数精度
}
```

# 2 字符流(字节流)
1）头文件 <sstream>

2）sstream

istringstream ostringstream stringstream(常用这个)

3）stringsteam常用函数

获取字符流对象中字符串 string str（）

重置字符流对象中字符串 string str（const string& str）；

>>字符流读操作

<<字符流写操作

```cpp
void testStringStream()
{
    stringStream strStream(string("ILOVEYOU"));
    cout<<strStream.str()<<endl;
    strStream.str(string(IMISSYOU));
    cout<<strStream.str()<<endl;
    
    stringStream ipInfo("ip地址：192.168.1.1");
    char info[20] = "";
    ipInfo>>info;
    cout<<info<<endl;
    int ipNum[4];
    for	(int i=0;i<4;i++)
    {
        char temp;
        ipInfo>>ipNum[i];
        if(i!=3)
        {
            ipInfo>>temp;
        }
    }
    for(int i=0;i<4;i++)
    {
        cout<<ipNum[i]<<"\t";
    }
        
}
```

# 3 文件流
1）头文件 <fstream>

C语言标准IO库

fopen fread fwrite fseek fclose


## 基础IO（c标准IO接口库）
fopen,fread,fwrite,fseek,fclose 1.

scanf  printf

## 接口实现方式：
（1）FILE* fopen（char* filename，char* mode）； 1.


**filename：文件名  
****mode：文件打开方式——只读、只写、读写、追加写；**

+ r ：只读——若文件不存在，则打开失败；若存在，直接打开；
+ r+ ：读写——若文件不存在，打开失败，若存在，直接打开；
+ w ：只写——若文件不存在，则创建新文件，若存在，则清空文件原有内容打开文件；
+ w+ ：读写——若文件不存在，则创建新文件，若存在，则清空文件原有内容打开文件；
+ a ：追加写——每次写入文件数据时总是写入文件末尾；若文件不存在，则创建新文件;
+ a+ ：追加读写——每次写入文件数据时总是写入文件末尾；若文件不存在，则创建新文;
+ b ：fopen打开文件默认是文本文件，如果使用b，则表示进行二进制操作;**

（2）size_t fwrite(char* data,size_t block_size,size_t block_num,FILE* fp); 1.


**data ： 要向文件中写入的数据；  
****block_size : 块大小；  
****block_num : 块个数；  
****fp : fopen返回的文件操作句柄（文件流指针）；  
****返回值：成功返回实际操作个数，失败返回0；**

block_size为strlen（字符串）的话，block_size为1，就能很好的确定字符串大小；

![](https://cdn.nlark.com/yuque/0/2022/png/26460094/1651628810015-b7c11e8f-7992-4312-ba74-3340142c2dab.png)

（3）size_fread(char* buf,size_t block_size,size_t block_num,FILE* fp); 1.


**返回值：实际操作的块个数（完整的块个数）**  
注意：返回0时，可能是失败，可能是读到了文件末尾；  
例如：文件大小为10，块大小为100，块个数为1，则实际操作的块个数为0，返回值为0，表示读到了文件末尾；

（4）int fseek(FILE* fp,int offset,int whence) 1.


fp:文件流指针；  
offset：相对于指定位置（whence位置）的偏移量；  
whence：SEEK_SET 起始位置；SEEK_CUR 当前位置；SEEK_END末尾位置；  
返回值：成功返回0，失败返回-1

