# 二维数值对频数分布图

二维数值频数分布问题在很多地方比较常见，如何通过matlab画出二维数值的分布情况。一般有两种方法，1、提高维度，二、通过边缘直方图表示来间接表示。

## 三维表示

X坐标表示数值的第一个维度值，Y坐标表示数值的第二个维度值，Z坐标表示对应的频数。为此需要统计二维数值对的频数，这在数据库中很好处理，只需使用group by 然后在count对应列就可以实现。在matlab中需要怎么实现？下面将详细介绍计算二维数值对的频数。

```matlab
val = 1';
subs = [1 1; 2 2; 3 2; 1 1; 2 2; 4 1] 
% subs 为对应二维数值对，当val为1时统计个数
A = accumarray(subs,val);

```
得到数据集A为对应坐标的统计个数，使用bar3函数画出对应三维图形即可：
```matlab
bar3(A);
```



![三维表示图](https://imgkr.cn-bj.ufileos.com/92ac934e-179e-4dc1-a7bf-ce4cdf69f8ba.png)


## 边缘曲线表示
增加图形对某一条件进行锁定，统计其余变量的直方图表示。这里需要使用到两种图形，点阵图，直方曲线图。

![边缘曲线表示图](https://imgkr.cn-bj.ufileos.com/a964074c-c2de-4a0c-bd18-c9b7188b7ad6.png)

实现的关键代码如下所示：
```matlab
hp1 = uipanel('position',[0 0 1 1],'BackgroundColor','white');
scatterhist(x,y,'Group',species,'Kernel','on','Parent',hp1);
```
实际运行代码在公众号资料栏中获取。



