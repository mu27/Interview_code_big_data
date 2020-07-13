
# 目录

[一、题目描述](#一、题目描述)
[二、思路](#二、思路)
[三、环境 python3.6](#三、环境 python3.6)
[四、代码执行](#四、代码执行)


-----------------------

## 一、题目描述
#### 题目1、计算P90

已知一张表，记录了每一篇日记的点赞量，使用你熟悉的编程语言，计算日记的点赞量的 P90。
P90的定义:设P90=n，则90%的日记的点赞量，都小于等于n。 注:原始数据未排序，日记量达10亿

表结构如下:

    note_id, like_count
    1, 4
    2, 6
    ..., ...
举例:

1、10条日记的点赞量分别为:1，2，2，3，3，4，5，5，6，50，则 P90 = 6

2、9条日记的点击量分别为:1，2，2，3，3，4，5，5，6，则P90=5.5

#### 题目2、最⻓文本
一个文件，其中包含100亿条文本，每行一条，每条文本的⻓度不超过1024，可能存在重复 文本。
使用你熟悉的编程语言，找到最⻓的1万条文本，条件如下:

1、文本之间不重复。

2、如果某个文本被保留在结果中，那么所有与该文本⻓度相同的文本都应该被保留在结果中。

-----------------------
## 二、思路
#### 题目1、计算P90
Description: 外部排序实现计算日记点赞表的P90值

    1.读取大文件表，维护一个数据总量nTotal
    2.分为n个小文件（第一次内部排序<采用优化后的快排>；注意：内存量 大于等于 小文件的数据量大小）
    3.采用外部排序排序每个小文件
    4.合并所有小文件并输出到一个新的文件（依次读取每个文件从第一行到末尾，比较获取极（大/小）值，存入新文件）
    5.最终获得一个排序好的大文件
    6.通过排序后的大文件获取p90的位置 通过文件指针偏移读取具体的p90数据
    注：小文件倒序排列，点击数多的排在前面
    
#### 题目2、最⻓文本
###### 解法1
Description: pandas可以处理<=5TB的文件，直接使用pandas来处理超大文件

    1.读入文件为DataFrame：reader
    2.去重
    3.新增文本长度列和索引列
    4.按文本长度列分组（1024个分组）并返回每个分组的元素个数，产生新的DataFrame：group
    5.通过group表返回top_n文本的最小长度
    6.在reader大表中按条件查找行，产生DataFrame:result
    7.通过result表返回符合条件的top_n文本
    
###### 解法2
Description:哈希映射

    1.读取大文件表，生成hash key&value 存入1024个小文件（采用桶排序/计数排序，注意value不是字符串内容而是记录所在大文件中的行数）
        1.1 key为hash值（采用md5<散列化>）
        1.2 value为所在大文件中的行数
    2.根据顺序依次从大到小读出topN
    3.获取topN在文件中的行数并读取大文件表获取内容
    4.循环输出topN
    md5码：128位，16个字节
    
-----------------------


## 三、环境 python3.6

    pip install tqdm

    pip install pandas
    
-----------------------

## 四、代码执行
#### 题目1、计算P90
入口函数：`Q1/get_p90.py`
执行：

    cd Q1
    python get_p90.py


#### 题目2、最⻓文本
##### 解法1
入口函数：`Q2/get_top_1w_pandas.py`
执行：

    cd Q2
    python  get_top_1w_pandas.py


##### 解法2
入口函数：`Q2/get_top_1w_hash.py`
执行：

    cd Q2
    python  get_top_1w_hash.py
    
        
-----------------------------------------------------------------------------------------
