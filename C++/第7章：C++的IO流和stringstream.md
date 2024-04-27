 

- [pdf获取：7281](https://url18.ctfile.com/f/22722418-803656481-b71b2c)

### 文章目录

- [一：C++ IO流](#C_IO_22)
- - [（1）C++标准IO流](#1CIO_32)
  - [（2）C++文件IO流](#2CIO_61)
  - - [A：概述](#A_63)
    - [B：示例](#B_72)
- [二：stringstream](#stringstream_218)
- - [（1）数据类型转换](#1_220)
  - [（2）字符串的拼接](#2_251)

**在C语言中，经常用到的输入和输出函数共有如下三组**

| 输入/输出 | 操作对象 |
| --- | --- |
| `printf/scanf` | 控制台 |
| `fprintf/fscanf` | 文件 |
| `sprintf/sscanf` | 字符数组（缓冲区） |

**C++中对应的操作如下**

| 输入/输出 | 操作对象 |
| --- | --- |
| `ostream/isream` | 控制台 |
| `ofstream/istream` | 文件 |
| `ostringstream/istringstream` | 字符数组（缓冲区） |

# 一：C++ IO流

**C++ I/O流**：流即**流动**的意思，是指物质从一处向另一处流动的过程，是一种对**有序、连续且具有方向性**的数据的抽象描述 。C++ I/O流是指**信息从外部输入设备（比如键盘）向计算机内部（内存）输入和从内存向外部输出设备（比如屏幕）输出**的过程。为了实现这种流动，**C++定义了I/O标准类库，每个类称为一个流**，以完成某些功能

**该I/O标准类库中，IOS为基类，其他类都是直接或间接继承自IOS类。我们经常使用的`cin`和`cout`就是类`istream`和`ostream`的一个对象**

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210424131605718.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116088107)

## （1）C++标准IO流

**C++标准IO流：C++标准IO流提供了4个全局流对象：`cin`、`cout`、`cerr`和`cblog`，分别对应标准输入，标准输出，标准错误和标准日志**

**对于`cin`要特别注意以下两点**

 -    **空格和回车**都可以作为数据之间的分隔符，所以多个数据**可以在一行输入，也可以分行输入**
 -    如果是**字符型和字符串**，则空格无法用`cin`输入，这时可以使用 **`getline(cin,"this is a test")`**
 -    在做一些算法题时，可以这样接收测试数据

```cpp
//单个元素输入
while(cin>>a)
{
            
            	
	//代码片段
}

//多个元素循环输入
while(cin>>a>>b>>c)
{
            
            
	//代码片段
}
//整行接受
while(cin>>str)
{
            
            
	//代码片段
}
```

## （2）C++文件IO流

### A：概述

**C++文件IO流**：C/C++会根据文件内容格式将文件分为**二进制文件**和**文本文件**  
![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F20210424140726195.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116088107)  
**在C++中，对文件进行操作时主要会使用到以下两种文件流，实例化后对象之后，文件操作就变成了调用这些对象的接口的操作了**

- `ifstream ifile`（只做输入）
- `ofstream ofile`（只做输出）

### B：示例

如下是一个服务器的ip地址和端口号字段，它们被封装在一个结构体`SeverInfo`中

```cpp
struct ServerInfo
{
            
            
	char _ip[32];//Ip地址
	int _port;//端口号
};
```

创建一个类`ConfigManager`，它会完成文件的读写操作

 -    类成员`_configfile`指的是文件名

```cpp
class ConfigManager
{
            
            
public:
	ConfigManager(const char* configfile = "myconfig.txt")//构造
		:_configfile(configfile)
	{
            
            }

private:
	string _configfile;//配置文件名字
};
```

**①：对文件进行二进制读写**

写操作需要使用`ofstream`，其构造函数是

 -    第一个参数就是配置文件名
 -    第二个参数指定以什么方式打开

```cpp
explicit ofstream (const char* filename, ios_base::openmode mode = ios_base::out);
```

`ofstream`对象的`write`方法的参数为

 -    第一个参数是起始地址（由于是二进制的方式写，所以我们将其强转为`char*`）

```cpp
ostream& write (const char* s, streamsize n);
```

读操作需要使用`ifstream`和其`read`方法，用法同上

因此，二进制读写操作如下

```cpp
struct ServerInfo
{
            
            
    char _ip[32];//Ip地址
    int _port;//端口号
};

class ConfigManager{
            
            
public:
    ConfigManager(const char *configfile = "myconfig.txt"):_configfile(configfile)
    {
            
            }
    void WriteBin(const ServerInfo &info){
            
            
        ofstream  ofs(_configfile); // 二进制写入目标文件
        ofs.write((const char*) &info, sizeof(ServerInfo)); // 写入
    }
    void ReadBin(const ServerInfo &info){
            
            
        ifstream ifs(_configfile); //二进制获取文件
        ifs.read((char*) &info, sizeof(ServerInfo));
    }
private:
    string _configfile;
};

int main(){
            
            
    ConfigManager conf;

    ServerInfo W_info;
    ServerInfo R_info;

    strcpy(W_info._ip, "192.168.0.1");
    W_info._port = 22;

    conf.WriteBin(W_info); // 写入文件
    conf.ReadBin(R_info); // 从文件中读取
    cout << R_info._ip << endl;
    cout << R_info._port << endl;


}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2Fe51548aa19d04157bc194f35bfc59fa8.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116088107)

**②：对文件进行文本读写**

文本读写和二进制读写很类似，但更加方便，因为`ofstream`和`ifstream`分别重载了`<<`和`>>`

```cpp
struct ServerInfo
{
            
            
    char _ip[32];//Ip地址
    int _port;//端口号
};

class ConfigManager{
            
            
public:
    ConfigManager(const char *configfile = "myconfig.txt"):_configfile(configfile)
    {
            
            }
    void WriteText(const ServerInfo &info){
            
            
        ofstream  ofs(_configfile); // 二进制写入目标文件
        ofs << info._ip << endl;
        ofs << info._port << endl;
    }
    void ReadText(const ServerInfo &info){
            
            
        ifstream ifs(_configfile); //二进制获取文件
        ifs >> info._ip;
        ifs >> info._port;

    }
private:
    string _configfile;
};

int main(){
            
            
    ConfigManager conf;

    ServerInfo W_info;
    ServerInfo R_info;

    strcpy(W_info._ip, "192.168.0.1");
    W_info._port = 22;

    conf.WriteText(W_info); // 写入文件
    conf.ReadText(R_info); // 从文件中读取
    cout << R_info._ip << endl;
    cout << R_info._port << endl;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F2021042414540726.png%3Fx-oss-process%3Dimage%2Fwatermark%2Ctype_ZmFuZ3poZW5naGVpdGk%2Cshadow_10%2Ctext_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM5MTgzMDM0%2Csize_16%2Ccolor_FFFFFF%2Ct_70&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116088107)

# 二：stringstream

- `stringstream`非常实用，主要用于各种数据类型之间的转换和字符串的拼接

## （1）数据类型转换

```cpp
#include <string>
#include <sstream>
#include <iostream>
#include <stdio.h>
 
using namespace std;
 
int main()
{
            
            
    stringstream sstream;
    string strResult;
    int nValue = 1000;
 
    // 将int类型的值放入输入流中
    sstream << nValue;
    // 从sstream中抽取前面插入的int类型的值，赋给string类型
    sstream >> strResult;
 
    cout << "[cout]strResult is: " << strResult << endl;
    printf("[printf]strResult is: %s\n", strResult.c_str());
 
    return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F86a42ab03ab947f1bb08a60ac97d7c96.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116088107)

## （2）字符串的拼接

 -    **注意**：`str("")`和`clear()`的区别：`clear()`不会清空内容，它只是把最初对字符串转换类型等操作清除而已，相当于“初始化”

```cpp
#include <string>
#include <sstream>
#include <iostream>
 
using namespace std;
 
int main()
{
            
            
    stringstream sstream;
 
    // 将多个字符串放入 sstream 中
    sstream << "first" << " " << "string,";
    sstream << " second string";
    cout << "strResult is: " << sstream.str() << endl;
 
    // 清空 sstream
    sstream.str("");
    sstream << "third string";
    cout << "After clear, strResult is: " << sstream.str() << endl;
 
    return 0;
}
```

![在这里插入图片描述](https://ziquyun.com/main/csdn/img?url=https%3A%2F%2Fimg-blog.csdnimg.cn%2F7ac05e443e8644a492854850dc3ff66d.png&rfUrl=https%3A%2F%2Fzhangxing-tech.blog.csdn.net%2Farticle%2Fdetails%2F116088107)