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
