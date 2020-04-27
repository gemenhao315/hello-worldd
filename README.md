# hello-worldd
just another repository

#-*-conding:utf-8 -*-
import requests
import re
#防止爬虫被拒绝403 Forbindden报错
#爬虫伪装成谷歌浏览器用户Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36
#爬虫伪装谷歌浏览器抓取user-agent标签后订内容
session = requests.Session()
headers = {
   "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/65.0.3325.181 Safari/537.36"
}
#下载网页
req = session.get("https://www.jingcaiyuedu8.com/novel/JhzVj2/list.html", headers=headers)
#修改编码方式
req.encoding="utf-8"
#目标小说的网页源码
html=req.text
#获取小说名字
title=re.findall(r"<title>(.*?)</title>",html)[0]
#新建一个文件，保存小说内容
fb= open("%s.txt" % title,'w',encoding="utf-8")
#获取每一章节列表re.S（匹配所有字符） .*？非贪婪匹配 .不可见字符
#[0]剥离出列表
dl=re.findall(r'<dl class="panel-body panel-chapterlist">.*?</dl>',html,re.S)[0]
#print(dl)
#捕获章节内容如：href="/novel/JhzVj2/3.html">第3章 古镜的来历<
chapter_info_list=re.findall(r'href="(.*?)">(.*?)<',dl)
#循环每个章节，分别去下载
for chapter_info in chapter_info_list:
   print(chapter_info_list)

   #chapter_title=chapter_info[1]
   #chapter_url=chapter_info[0]
   chapter_url,chapter_title=chapter_info
   #合并章节超级链接
   chapter_url="https://www.jingcaiyuedu8.com%s" % chapter_url
   #print(chapter_url,chapter_title)
   #下载章节内容
   req=session.get(chapter_url,headers=headers)
   req.encoding="utf-8"
   #提取所有源代码章节内容赋值给chapter_html
   chapter_html=req.text
   #提取章节内容
   title_zhang=re.findall(r'最新章节！(.*?)</div>',chapter_html,re.S)[0]
   #清洗提取内容
   title_zhang=title_zhang.replace(" ","")
   title_zhang = title_zhang.replace("</p>", "")
   title_zhang = title_zhang.replace("<p>", "")
   #print(title_zhang)
   #持久化
 #  fb.write(chapter_title)
 #  fb.write("\n")
  # fb.write(title_zhang)
  # fb.write("\n")
#删除多余内容
#chapter_info_list= chapter_info_list.replace("<dd class="col-md-4">","")

#print(chapter_info_list)
