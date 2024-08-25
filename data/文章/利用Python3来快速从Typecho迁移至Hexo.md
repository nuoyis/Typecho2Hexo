---
title: 利用Python3来快速从Typecho迁移至Hexo
date: 2024-02-04 13:42:34
categories: 
tags: []
---
最近在GitHub上找到一个写Typecho转Hexo的大佬，可惜我是最新版的Python，运行直接报错，修改一个如同大洪水一样又报错很多。
于是在他的基础上，用python3的包来替换原来的一些写的方法。
代码如下:
```Python
# -*- coding: utf-8 -*-
# 原作者:https://github.com/zhourongyu/Typecho2Hexo
# 新数据库借鉴作者:https://www.jianshu.com/p/4e72faebd27f
import os
import re
import pymysql
import arrow
from flask import Flask
import urllib
import codecs

host = ''
port = 3306
db = ''
user = ''
password = ''

def main():
    conn = pymysql.connect(host=host, port=port, db=db, user=user, password=password)
    cursor = conn.cursor(pymysql.cursors.DictCursor)
    # 创建分类和标签
    cursor.execute("select type, slug, name from typecho_metas")
    for cate in cursor.fetchall():
        path = 'data/分类/%s' % urllib.parse.unquote(cate['slug'])
        if not os.path.exists(path):
            os.makedirs(path)
        f = codecs.open('%s/index.md' % path, 'w', "utf-8")
        f.write("title: %s\n" % urllib.parse.unquote(cate['slug']))
        f.write("date: %s\n" % arrow.now().format('YYYY-MM-DD HH:mm:ss'))
        # 区分分类和标签
        if cate['type'] == 'category':
            f.write('type: "categories"\n')
        elif cate['type'] == 'tags':
            f.write('type: "tags"\n')
        # 禁止评论
        f.write("comments: false\n")
        f.write("---\n")
        f.close()

    # 创建文章
    cursor.execute("select cid, title, slug, text, created from typecho_contents where type='post'")
    for e in cursor.fetchall():
        title = re.sub('[\/:*?"<>|]','-',e['title'].encode('raw_unicode_escape').decode("unicode-escape"))
        content = str(e['text'].replace('', ''))
        tags = []
        category = ""
        # 找出文章的tag及category
        cursor.execute(
            "select type, name, slug from `typecho_relationships` ts, typecho_metas tm where tm.mid = ts.mid and ts.cid = %s",
            e['cid'])
        for m in cursor.fetchall():
            if m['type'] == 'tag':
                tags.append(m['name'])
            if m['type'] == 'category':
                category = urllib.parse.unquote(m['slug'])
        path = 'data/文章/'
        if not os.path.exists(path):
            os.makedirs(path)
        f = codecs.open('%s%s.md' % (path, title), 'w', "utf-8")
        f.write("---\n")
        f.write("title: %s\n" % title)
        f.write("date: %s\n" % arrow.get(e['created']).format('YYYY-MM-DD HH:mm:ss'))
        f.write("categories: %s\n" % category)
        f.write("tags: [%s]\n" % ','.join(tags))
        f.write("---\n")
        f.write(content)
        f.close()
    conn.close()


if __name__ == "__main__":
    main()
```
复制粘贴,填写你的数据库信息即可