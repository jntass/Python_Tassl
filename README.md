# Python_Tassl
支持国密协议的python （Python that supports CNTLS protocols）

Python-2.7.17集成tassl支持国密方法
1.	下载支持CNTLS的Python-2.7.17_tassl.tar.xz增加了支持CNTLS的部分。
2.	export LDFLAGS="-L/root/lib_r/tassl/lib/"  其中/root/lib_r/tassl/lib 为安装tassl的库目录。
3.	./configure --prefix=/root/lib_r/Python-2.7.17 ; make; make install
4.	export LD_LIBRARY_PATH=/root/lib_r/tassl/lib    设置python执行时需要的tassl库目录。
5.	/root/lib_r/Python-2.7.17/bin/python ./sm_http_client.py 其中sm_http_client.py为python测试客户端，发起https请求。

sm_http_client.py脚本如下:

import socket

import ssl 

import urllib

import httplib


host = '192.168.6.245'


port = 443 


ctx = ssl.SSLContext(ssl.PROTOCOL_CNTLS)    #选择国密客户端方法

ctx.set_ciphers('ECC-SM4-SM3')    #选择国密套件


conn = httplib.HTTPSConnection(host, port, context=ctx)


test_data = {'test':'aaaa'}

test_data_urlencode = urllib.urlencode(test_data)

headerdata = {"Host":"192.168.6.244"}


conn.request('GET', '/', test_data_urlencode, headerdata)

response = conn.getresponse()

res= response.read()

print response.status, response.reason

print res 
