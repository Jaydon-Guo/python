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
    
## 将BGR图像转换为灰度图像
    import cv2
    import numpy as np
    img=np.random.randint(0,256,size=[2,4,3],dtype=np.uint8)
    rst=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
    print("img=\n",img)
    print("rst=\n",rst)
    print("像素点(1,0)直接计算得到的值=",img[1,,0]*0.114+img[1,0,1]*0.587+img[1,0,2]*0.299)
    print("像素点(1,0)使用公式cv2.cvtColor()转换值=",rst[1,0])
    
## 将灰度图像转换为BGR图像
    import cv2
    import numpy as np
    img=np.random.randint(0,256,size=[2,4],dtype=np.uint8)
    rst=cv2.cvtColor(img,cv2.COLOR_GRAY2BGR)
    print("img=\n",img)
    print("rst=\n",rst)
    
## 将图像在BGR和RGB模式之间相互转换
    import cv2
    import numpy as np
    img=np.random.randint(0,256,size=[2,4,3],dtype=np.uint8)
    rgb=cv2.cvtColor(img,cv2.COLOR_BGR2RGB)
    bgr=cv2.cvtColor(rgb,cv2.COLOR_RGB2BGR)
    print("img=\n",img)
    print("rgb=\n",rgb)
    print("bgr=\n",bgr)
    
## 将图像在BGR模式和灰度模式之间相互转换
    import cv2
    lena=cv2.imread("lenacolor.png")
    gray=cv2.cvtColor(lena,cv2.COLOR_BGR2GRAY)
    rgb=cv2.cvtColor(gray,cv2.COLOR_GRAY2BGR)
    #==========打印shape============
    print("lena.shape=",lena.shape)
    print("gray.shape=",gray.shape)
    print("rgb.shape=",rgb.shape)
    #=========显示效果==============
    cv2.imshow("lena",lena)
    cv2.imshow("gray",gray)
    cv2.imshow("rgb",rgb)
    cv2.waitKey()
    cv2.destroyAllWindows()
    
## 将图像从BGR模式转换为RGB模式
    import cv2
    lena=cv2.imread("lenacolor.png")
    rgb=cv2.cvtColor(lena,cv2.COLOR_BGR2RGB)
    cv2.imshow("lena",lena)
    cv2.imshow("rgb",rgb)
    cv2.waitKey()
    cv2.destroyAllWindows()
    
## HSV色彩空间讨论
    import cv2
    import numpy as np
    #==========测试一些OpenCV中蓝色的HSV模式值==========
    imgBlue=np.zeros([1,1,3],dtype=np.uint8)
    imgBlue[0,0,0]=255
    Blue=imgBlue
    BlueHSV=cv2.cvtColor(Blue,cv2.COLOR_BGR2HSV)
    print("Blue=\n",Blue)
    print("BlueHSV=\n",BlueHSV)
    #==========测试一下OpenCV中绿色的HSV模式值==========
    imgGreen=np.zeros([1,1,3],dtype=np.uint8)
    imgGreen[0,0,1]=255
    GreenHSV=cv2.cvtColor(Green,cv2.COLOR_BGR2HSV)
    print("Green=\n",Green)
    print("GreenHSV=\n",GreenHSV)
    #=========测试一下OpenCV中红色的HSV模式值===========
    imgRed=np.zeros([1,1,3],dtype=np.uint8)
    imgRed[0,0,2]=255
    Red=imgRed
    RedHSV=cv2.cvtColor(Red,cv2.COLOR_BGR2HSV)
    print("Red=\n",Red)
    print("RedHSV=\n",RedHSV)
    
## 显示特定颜色值
    import cv2
    import numpy as np
    opencv=cv2.imread("opencv.jpg")
    hsv=cv2.cvtColor(opencv,cv2.COLOR_BGR2HSV)
    cv2.imshow('opencv',opencv)
    #============指定蓝色值的范围============
    minBlue=np.array([110,50,50])
    maxBlue=np.array([130,255,255])
    #确定蓝色区域
    mask=cv2.inRange(hsv,minBlue,maxBlue)
    #通过掩码控制的按位与运算，锁定蓝色区域
    blue=cv2.bitwise_and(opencv,opencv,mask=mask)
    cv.imshow('blue',blue)
    #==========指定绿色值的范围===========
    minGreen=np.array([50,50,50])
    maxGreen=np.array([70,255,255])
    #确定绿色区域
    mask=cv2.inRange(hsv,minGreen,maxGreen)
    #通过掩码控制的按位与运算，锁定绿色区域
    green=cv2.bitwise_and(opencv,opencv,mask=mask)
    cv2.imshow('green',green)
    #==========指定红色值的范围===========
    minRed=np.array([0,50,50])
    
