# Kwaisign
快手API签名算法
快手的API接口都使用签名做了保护，API接口请求使用的是POST方法，签名是POST表单中的`sig`参数，我们看一下API请求的内容：
```
POST http://api.gifshow.com/rest/n/moment/tag/userTagList?app=0&kpf=ANDROID_PHONE&ver=6.2&c=GENERIC&mod=Xiaomi%28Mi%20Note%203%29&appver=6.2.1.8491&ftt=K-T-T&isp=CUCC&kpn=KUAISHOU&lon=113.712144&language=zh-cn&sys=ANDROID_8.1.0&max_memory=256&ud=0&country_code=cn&oc=GENERIC&hotfix_ver=&did_gt=1553327019079&iuid=&net=WIFI&did=ANDROID_ae30380af28a769e&lat=34.59372 HTTP/1.1
User-Agent: kwai-android
Connection: keep-alive
Accept-Language: zh-cn
X-REQUESTID: 634454806
Content-Type: application/x-www-form-urlencoded
Content-Length: 83
Host: api.gifshow.com
Accept-Encoding: gzip

userId=51363790&sig=4fa5b967070fe445ccec17fa6a53c820&client_key=3c2cd3f3&os=android
```
`sig`的计算方法如下：

 1. 把url中的参数放入map1中；
 2. 把表单中的参数放入map2中；
 3. 把map1和map2中的元素以key=value的形式放入arraylist中；
  ![在这里插入图片描述](https://img-blog.csdnimg.cn/20190409122339589.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9bG9nLmNzZG4ubmV0L3dhbmdteGU=,size_16,color_FFFFFF,t_70)
 4. 对arraylist进行排序；
 5. 把arraylist中的元素按顺序拼接成一个字符串str；
 6. 把str转成bytearray；
 7. 调用`CPU.getClock()`，传入str计算签名；

`CPU.getClock()`是一个native方法，在`libcore.so`中实现，快手接口签名算法大概就这些，对细节感兴趣的朋友可以[联系我](http://47.105.95.219/)。
