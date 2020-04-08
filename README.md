# Kwaisign
快手API签名算法
快手的API接口都使用签名做了保护，API接口请求使用的是POST方法，签名是POST表单中的`sig`参数，我们看一下API请求的内容：
```
POST https://apissl.gifshow.com/rest/n/user/profile/v2?mod=Xiaomi%282014813%29&lon=113.734844&country_code=CN&kpn=KUAISHOU&oc=GENERIC&egid=DFP635C749E2DDA6433B6D675E0AD1EC85737BB48E06C7EDEB5A7BBE555968B6&hotfix_ver=&sh=1280&appver=6.11.0.11882&socName=%3A%20Qualcomm%20MSM8916&max_memory=96&isp=&browseType=1&kpf=ANDROID_PHONE&did=ANDROID_4afa33d874ca03f4&net=WIFI&app=0&ud=1329122448&c=GENERIC&sys=ANDROID_5.1.1&sw=720&ftt=&language=zh-cn&iuid=&lat=34.770369&did_gt=1577761445710&ver=6.11 HTTP/1.1
Host: apissl.gifshow.com
Connection: keep-alive
Content-Length: 272
Cookie: region_ticket=RT_3934BBDA9108DA947909724E0F49F964A182C6E00B8075F784F83F6F94187;token=f49d24315f564c0798f4de74de251b7c-1329122448
X-REQUESTID: 157845368620498412
User-Agent: kwai-android
Accept-Language: zh-cn
Content-Type: application/x-www-form-urlencoded
Accept-Encoding: gzip, deflate, br

user=1137667035&pv=true&__NS_sig3=2212054879c6d647c3cae9c25604b5f2ea2d1895fe&__NStokensig=702cea9556d98f2de6ff4e1b075f24d3add2e0336d6341aa45bcba3f08d38a0e&token=f49d24315f564c0798f4de74de251b7c-1329122448&client_key=3c2cd3f3&os=android&sig=9bae685031a105be31603c9606ed393e
```
`sig`的计算方法如下：  
 1. 把url中的参数放入map1中；
 2. 把表单中的参数放入map2中；
 3. 把map1和map2中的元素以key=value的形式放入arraylist中；
  ![在这里插入图片描述](http://upload-images.jianshu.io/upload_images/14774992-a643a8184793d873?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
 4. 对arraylist进行排序；
 5. 把arraylist中的元素按顺序拼接成一个字符串str；
 6. 把str转成bytearray；
 7. 调用`CPU.getClock()`，传入str计算签名；  

`CPU.getClock()`是一个native方法，在`libcore.so`中实现，`__NS_sig3`是根据url中的path和sig计算的，在`libkwsgmain.so`中实现，快手接口签名算法大概就这些，对细节感兴趣的朋友可以[联系](http://47.105.95.219)。
