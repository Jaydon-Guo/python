## 模拟数字水印的嵌入和提取过程
    import numpy as np
    import cv2
    #读取原始载体图像
    lena=cv2.imread("lena.bmp",0)
    #读取水印图像
    watermark=cv2.imread("watermark.bmp",0)
    #将水印图像内的值255处理为1，以方便嵌入
    #后续章节会介绍使用threshold处理
    w=watermark[:,:]>0
    watermark[w]=1
    #读取原始载体图像的shape值
    r,c=lena.shape
    #============嵌入过程===============
    #生成元素值都是254的数组
    t254=np.ones((r,c),dtype=np.uint8)*254
    #获取lena图像的高七位
    leanH7=cv2.bitwise_and(lena,t254)
    #将watermark嵌入lenaH7内
    e=cv2.bitwise_or(lenaH7,watermark)
    #===========提取过程================
    #生成元素都是1的数组
    t1=np.ones((r,c),dtype=np.uint8)
    #从载体图像内提取水印图像
    wm=cv2.bitwise_and(e,t1)
    print(wm)
    #将水印图像内的值1处理为255，以便显示
    #后续章节会介绍使用threshold实现
    w=wm[:,:]>0
    wm[w]=255
    #==========显示================
    cv2.imshow("lena",lena)
    cv2.imshow("watermark",watermark*255)
    cv2.imshow("e",e)
    cv2.imshow("wm",wm)
    cv2.waitKey()
    cv2.destroyAllWindows()
    
## 脸部打码及解码
    import cv2
    import numpy as np
    #读取原始载体图像
    lena=cv2.imread("lena.bmp",0)
    #读取原始载体图像的shape值
    r,c=lena.shape
    mask=np.zeros((r,c).dtype=np.uint8)
    mask[220:400,250:350]=1
    #读取一个key，打码、解码所使用的密钥
    key=np.random.randint(0,256,size=[r,c],dtype=np.uint8)
    #=============获取打码脸===============
    #使用密钥key对原始图像lena加密
    leanXorKey=cv2.bitwise_xor(lena,key)
    #获取加密图像的脸部信息encryptFace
    encryptFace=cv2.bitwise_and(lenaXorKey,mask*255)
    #将图像lena内的脸部值设置为0，得到noFace1
    noFace1=cv2.bitwise_and(lena,(1-mask)*255)
    #得到打码的lena图像
    maskFace=encryptFace+noFace1
    #============将打码脸解码=============
    #将脸部打码的lena与密钥key进行异或运算，得到脸部的原始信息
    extractOriginal=cv2.bitwise_xor(maskFace,key)
    #将解码的脸部信息extractOriginal提取出来，得到extractFace
    extractFace=cv2.bitwise_and(extractOriginal,mask*255)
    #从脸部打码的lena内提取没有脸部信息的lena图像，得到noFace2
    noFace2=cv2.bitwise_and(maskFace,(1-mask)*255)
    #得到解码的lena图像
    extractLena=noFace2+extractFace
    #===========显示图像================
    cv2.imshow("lena",lena)
    cv2.imshow("mask",mask*255)
    cv2.imshow("1-mask",(1-mask)*255)
    cv2.imshow("key",key)
    cv2.imshow("lenaXorKey",lenaXorKey)
    cv2.imshow("encryptFace",encryptFace)
    cv2.imshow("noFace1",noFace1)
    cv2.imshow("maskFace",maskFace)
    cv2.imshow("extractOriginal",extractOriginal)
    cv2.imshow("extractFace",extractFace)
    cv2.imshow("noFace2",noFace2)
    cv2.imshow("extractLena",extractLena)
    cv2.waitKey()
    cv2.destroyAllWindows()
    
## 
    
