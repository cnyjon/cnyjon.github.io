---
layout: post
title:  "使用requests发送POST请求"
date:   2016-01-20 23:05:20

---

# 使用requests发送POST请求

在Python脚本中直接POST表单到服务器

	#!/usr/bin/env python
	# -*- coding: utf-8 -*-
	import requests

	payload = {‘key1’: 'value1', 'key2': 'value2'}

	headers = {'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64; rv:38.0) Gecko/20100101 Firefox/38.0', 'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8', 'Accept-Encoding': 'gzip, deflate', 'Referer': 'http://www.example.com/open.php', 'Cookie': 'OSTSESSID=3gvg0daagonmsds3lr4dac8r32', 'Content-Type': 'application/x-www-form-urlencoded' }

	r = requests.post("http://www.example.com/open.php", data=payload, headers=headers)
