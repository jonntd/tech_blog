Title: Maya custom menu 
Date: 2010-12-03 10:20
Modified: 2010-12-05 19:30
Category: programming 
Tags: 
Slug: Maya custom menu 
Authors: 
Summary: How to add custom menu or menuitem into maya ui.

--- 
layout: post 
title: Maya custom menu 
categories: programming 
--- 

# how to add custom menu or menuitem into maya 
在maya中，怎么新加自定义的菜单? 

There are some tutorials already on the web outside, learn from them, here i just reorganize some cases i have tried. 

## do it using mel

![maya_custom_menu](链接地址) 
难点包括：
1. 是要找打uv editor这个window的名字, 才能在setParent里面用。如在图片所示，这里我先尝试打开和关闭uv editor window来看它的名字。

局限包括:
1. 当uv editor没有打开的时候，执行上面的add menu mel是失败的, 所以要先想办法或者手动来打开那个uv editor window; 
2. 关了这个uv editor，再重新打开刚才假如的menu又没了，需要重新加入；

## do it for plugin 


