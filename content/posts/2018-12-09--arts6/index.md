---
title: ARTS 5
author: Galileo Finch
category: "arts"
cover: laura-college-190105-unsplash.jpg
---

# Algorithm

给定一个字符串，根据字符的频率按递减顺序对其进行排序。

>Input:
>"tree"
>
>Output:
>"eert"
>
>Explanation:
>'e' appears twice while 'r' and 't' both appear once.
>So 'e' must appear before both 'r' and 't'. Therefore "eetr" is also a valid answer.

```java
public String frequencySort(String s) {
    Map<Character, Integer> frequencyMap = new HashMap<>();
    for (char c : s.toCharArray())
        frequencyMap.put(c, frequencyMap.getOrDefault(c, 0) + 1);

    List<Character>[] frequencyBucket = new ArrayList[s.length() + 1];
    for (char c : frequencyMap.keySet()) {
        int f = frequencyMap.get(c);
        if (frequencyBucket[f] == null) {
            frequencyBucket[f] = new ArrayList<>();
        }
        frequencyBucket[f].add(c);
    }
    StringBuilder str = new StringBuilder();
    for (int i = frequencyBucket.length - 1; i >= 0; i--) {
        if (frequencyBucket[i] == null) {
            continue;
        }
        for (char c : frequencyBucket[i]) {
            for (int j = 0; j < i; j++) {
                str.append(c);
            }
        }
    }
    return str.toString();
}
```

# Review

[`How to Read an Academic Article`](https://organizationsandmarkets.com/2010/08/31/how-to-read-an-academic-article/)

本周读了一下这篇文章，并总结了一下自己的阅读文章的[方法](https://galileofinch.com/some-about-learn-method/)。

# Technique/Tips

本周有个对账的需求，考虑到使用Spring和Quartz实现的定时对账是基于服务的。个人网站的业务升级重启比较多，不适合做这类定时需求。所以开始考虑使用shell+crontab定时向对方FTP发送账单。

如下，整理了3个基础function，初始化文件、发送文件、以及数据提取。配置`database.config`以及`workflow.config`,
database.config包含ISMP_DB_USER等数据库相关配置，workflow.config包含ftp相关信息。

```sh
#!/bin/bash

########################################################################
# 程序名：commonFtpUpload.sh
# 作者：galileo finch
# 生成日期：2018-12-05
#修改内容：
########################################################################

# PATH="${PATH}":/usr/local/bin/:/usr/local/etc/:/usr/local/lib/:/usr/local/lftp
# export PATH

function InitEnv 
{

  #脚本名，去.sh后缀
  SCRIPT_NAME="$(basename $0 .sh)"
  #脚本安装路径
	DIR_INSTALL=$(cd $(dirname $0);pwd)
	#脚本运行日期
  DAY=$(date +'%Y%m%d')
  HOUR=$(date +'%H')
  TIME=$(date +'%H%M%S')
  #脚本运行UTC时间
  UTC=$(date +'%s')
  #日志目录路径
  DIR_LOG="${DIR_INSTALL}/log/"
  #脚本运行正常日志
  LOG_NORMAL="${DIR_LOG}/${SCRIPT_NAME}_${DAY}.log"
  #脚本运行错误日志
  LOG_ERROR="${DIR_LOG}/${SCRIPT_NAME}_${DAY}.err"

	source ${DIR_INSTALL}/config/database.config
	source ${DIR_INSTALL}/config/workflow.config

	#建立日志存放目录
  mkdir -p ${DIR_LOG}
	
	exec 2>"${LOG_ERROR}"
}

########################################################################
# 函 数 名 ：UploadFile
# 功能描述 ：上传文件至FTP服务器
# 输入参数 ： $1目标ip $2目标端口 $3目标ftp账号 $4目标ftp密码 $5文件存放地址 $6 文件
########################################################################
function UploadFile
{
	ftp -n -i <<- EOF  
		open ${1} ${2}
		user ${3} ${4}
		pass
		bin
		cd ${5}
		put ${6}
		bye   
	EOF
}

#######################################################################
# 函 数 名  : send4GFTP
# 功能描述  : 上报4G相关账单
# 输入参数  : 
# 返 回 值  : 无
# 调用函数  :
########################################################################
function send4GFTP
{
  InitEnv
  local FILENAME="${DIR_INSTALL}/G4_APPLYLOG_${DAY}.bat"
 	echo "unload to ${FILENAME} select * from db_name" | dbaccess ${ISMP_DB_USER}
	UploadFile ${G4_FTP_HOST} ${G4_FTP_PORT} ${G4_FTP_USER} ${G4_FTP_PASSWORD} ${G4_REMOTE_PATH} ${FILENAME}
}
```

# Share

本周看了下李永乐老师的[基因编辑](https://www.youtube.com/watch?v=o1MdiW5UZh0&t=1s)

CRISPR是存在于细菌中的一种基因组，该类基因组中含有曾经攻击过该细菌的病毒的基因片段。细菌透过这些基因片段来侦测并抵抗相同病毒的攻击，并摧毁其DNA。这类基因组是细菌免疫系统的关键组成部分。透过这些基因组，人类可以准确且有效地编辑生命体内的部分基因，也就是CRISPR/Cas9基因编辑技术。
有趣的是人们的评论，代表的不同的观点，我摘取了部分。

~~~
家长A:“你家宝宝的微积分这次考了多少分?” 
家长B:“嗨，别提了。才99分，全班倒数第三。 ”
家长A:“你要孩子之前没定制数学加强款呐?  ”
家长B:“我们家孩子爸爸嫌贵呀，只买了6个基因包，全癌症免疫和长跑、双眼皮显性遗传都有了，结果数学加强这块，就没买最新款。
家长A:“那你们准备什么时候要老二啊?”
家长B:“明年吧，听说明年要出新款，先天免疫疾病比现款多21种，平均寿命高40年，还有限量版的金城武颜值加强款。”
家长A:“还是晚点儿要孩子好，我家这个才3岁，配置已经明显过时了。  ”
家长B:“可不是，这周末我就找售后去闹一下，怎么着也得给我们家老大数学基因这块给补齐喽，最起码也得让他们给打个折”


大神写了几十亿行的代码，持续维护了几十亿年，无数个版本，运行至今。然而一个小白看了一段代码，说这个地方有bug，要改一下，也没测试，就放线上跑了，自信的说，系统挂了，我负责。。。

总得来说，不变是相对的，变是永恒的。讨论的大多是担心到时候只有高等人种和低等人种。其实根据二八定律，也就是社会上20%的人占有80%的社会财富。奶头乐就是控制这些人最好的手段，综艺、抖音、王者荣耀等等。所以不用担心这么多，人只要有事做就行了。
~~~