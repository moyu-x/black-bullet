KDD99 是一个用来从正常连接中监测非正常连接的数据集。产出于 1999 年 Thrid International Knowlegde Discovery and Data Mining Tools Competition，其目的是建立一个稳定的的入侵检测系统。

https://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html

# 对于 TCP 的描述

表示该连接是否按照协议要求开始或完成。例如 SF 表示连接正常建立并终止；S0 表示只接到了 SYN 请求数据包，而没有后面的 SYN/ACK。其中 SF 表示正常，其他 10 种都是 error。

http://www.takakura.com/Kyoto_data/BenchmarkData-Description-v5.pdf

![[assets/b530578c252be878074e19024f60d48f_MD5.jpeg]]

对于同样状态的描述，也可以在`zeek`的文档中找到，如下：

![[assets/3d81fa28761223cfea70810608216e26_MD5.jpeg]]

https://docs.zeek.org/en/master/scripts/base/protocols/conn/main.zeek.html