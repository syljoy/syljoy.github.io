---
layout: post
title: 数字图像处理练习
date: 2020-03-19
tags: 数字图像处理 matlab
---

# 数字图像处理练习（MATLAB）

- [直方图均衡化处理](#直方图均衡化处理)
- [滤波器大小对比](#滤波器大小对比)
- [均值滤波器和中值滤波器](#均值滤波器和中值滤波器)
- [伽马变换-图像增强](#伽马变换-图像增强)


> yangxiao_test2


### 直方图均衡化处理

1. 绘制下面四幅图的直方图，并进行直方图均衡化处理 

<table align="center">
  <tr>
    <td>
      <img src="/images/posts/2020-03-19-digital_image_processing_yx2/1.1.png" width="197">
    </td>
    <td>
      <img src="/images/posts/2020-03-19-digital_image_processing_yx2/1.2.png" width="197">
    </td>
    <td>
      <img src="/images/posts/2020-03-19-digital_image_processing_yx2/1.3.png" width="197">
    </td>
    <td>
      <img src="/images/posts/2020-03-19-digital_image_processing_yx2/1.4.png" width="197">
    </td>
  </tr>
</table>

  
    ```matlab
    I_11 = imread('1.1.png');
    J_11 = histeq(I_11);
    subplot(221),imshow(I_11);
    title("原图");
    subplot(222),imshow(J_11);
    title('直方图均衡化后');
    subplot(223),imhist(I_11,64);
    title('原图像的直方图');
    subplot(224),imhist(J_11,64);
    title('均衡化后的直方图');
    ```
    
    > imhist(I,n)其中，I为灰度的输入图像，n为指定的灰度级数目，缺省值为256；
    
    运行结果：
    
    ![image-20200319000816518](/images/posts/2020-03-19-digital_image_processing_yx2/image-20200319000816518.png)

###  滤波器大小对比

1. 对本图采用不同的线性滤波器进行处理，滤波器大小分别用3\*3，5\*5，7\*7，9\*9进行处理 。

   ![2](/images/posts/2020-03-19-digital_image_processing_yx2/2.png)

   线性滤波器：

   - 均值滤波器
   - 高斯滤波器
   - 盒子滤波器
   - 拉普拉斯滤波器

   ```matlab
   I_2 = imread('2.png');
   I_2 = rgb2gray(I_2);
   W3 = fspecial('gaussian',[3,3]); 
   G3 = imfilter(I_2, W3, 'symmetric');
   W5 = fspecial('gaussian',[5,5]);
   G5 = imfilter(I_2, W5, 'symmetric');
   W7 = fspecial('gaussian',[7,7]);
   G7 = imfilter(I_2, W7, 'symmetric');
   W9= fspecial('gaussian',[9,9]);
   G9 = imfilter(I_2, W9, 'symmetric');
   subplot(151); imshow(I_2);
   title('原始图像');
   subplot(152); imshow(G3);
   title('3*3高斯滤波后图像');
   subplot(153); imshow(G5);
   title('5*5高斯滤波后图像');
   subplot(154); imshow(G7);
   title('7*7高斯滤波后图像');
   subplot(155); imshow(G9);
   title('9*9高斯滤波后图像');
   ```
   运行结果：

   ![image-20200319003458997](/images/posts/2020-03-19-digital_image_processing_yx2/image-20200319003458997.png)

   > Fspecial函数：用于创建预定义的滤波算子，其语法格式为：
   > $$h = fspecial(type)$$
   > $$h = fspecial(type,parameters,sigma)$$
   >
   > 参数：type，指定算子类型，para指定相应的参数。
   >
   > > type= ‘average’，为均值滤波，参数为n，代表模版尺寸，用向量表示，默认值为[3,3]。
   > > type= ‘disk’，为圆形区域均值滤波，参数为radius代表区域半径，默认值为5
   > > type= ‘gaussian’，为高斯低通滤波器，参数有两个，n表示模版尺寸，默认值为[3,3]，sigma表示滤波器的标准差，单位为像素，默认值为 0.5。
   > > type= ‘laplacian’，为拉普拉斯算子，参数为alpha，用于控制拉普拉斯算子的形状，取值范围为[0,1]，默认值为0.2。
   > > type= ‘log’，为拉普拉斯高斯算子，参数有两个，n表示模版尺寸，默认值为[3,3]，sigma为滤波器的标准差，单位为像素，默认值为0.5
   > > type= ‘prewitt’，为prewitt算子，用于边缘增强，无参数。
   > > type= ‘sobel’，为著名的sobel算子，用于边缘提取，无参数。
   > > type= ‘unsharp’，为对比度增强滤波器，参数alpha用于控制滤波器的形状，范围为[0,1]，默认值为0.2。
   > > type= ‘motion’，为运动模糊算子，有两个参数，表示摄像物体逆时针方向以theta角度运动了len个像素，len的默认值为9，theta的默认值为0;  
<br />
   > imfilter函数：对任意类型数组或多维图像进行滤波，其语法格式为：
   > $$B = imfilter(A,H)$$
   > $$B = imfilter(A,H,option1,option2,...)$$
   > 参数：A是要滤波的图像
   > 			H是滤波器
   > 			option是可选择的参数，见下表：
   >
   > <table>
   > <tbody>
   > 	<tr>
   > 		<td width="108" valign="top"></td>
   > 		<td width="68" valign="top">选项</td>
   > 		<td width="470" valign="top">描述</td>
   > 	</tr>
   > 	<tr>
   > 		<td colspan="1" rowspan="2">filtering_mode</td>
   > 		<td>‘corr’</td>
   > 		<td>通过使用相关来完成，该值为默认。</td>
   > 	</tr>
   > 	<tr>
   > 		<td>‘conv’</td>
   >  <td>通过使用卷积来完成</td>
   > </tr>
   > <tr>
   > 	<td colspan="1" rowspan="4">boundary_options</td>
   > 	<td>‘X’</td>
   > 	<td>输入图像的边界通过用值X（无引号）来填充扩展<br>　　其默认值为0</td>
   > 	</tr>
   > 	<tr>
   > 	<td>‘replicate’</td>
   > 	<td>图像大小通过复制外边界的值来扩展</td>
   > </tr>
   > <tr>
   > 	<td>‘symmetric’</td>
   > 	<td>图像大小通过镜像反射其边界来扩展</td>
   > </tr>
   > <tr>
   > 	<td>‘circular’</td>
   > 	<td>图像大小通过将图像看成是一个二维周期函数的一个周期来扩展</td>
   > </tr>
   > <tr>
   > 	<td colspan="1" rowspan="2">size_options</td>
   > 	<td>‘full’</td>
   > 	<td>输出图像的大小与被扩展图像的大小相同</td>
   > </tr>
   > <tr>
   > 	<td>‘same’</td>
   > 	<td>输出图像的大小与输入图像的大小相同。这可通过将滤波掩模的中心点的偏移限制到原图像中包含的点来实现，该值为默认值。</td>
   > </tr>
   > </tbody>
   > </table>



### 均值滤波器和中值滤波器

1. 对本图进行均值滤波和中值滤波。

![3](/images/posts/2020-03-19-digital_image_processing_yx2/3.png)



```matlab
I_3 = imread('3.png');
I_3 = rgb2gray(I_3);
W_average = fspecial('average',[3,3]); 
img_average = imfilter(I_3, W_average, 'symmetric');
img_med = medfilt2(I_3, [3,3]);

subplot(131); imshow(I_3);
title('原始图像');
subplot(132); imshow(img_average);
title('均值滤波后图像');
subplot(133); imshow(img_med);
title('中值滤波后图像');
```

运行结果：

![image-20200319011400809](/images/posts/2020-03-19-digital_image_processing_yx2/image-20200319011400809.png)



### 伽马变换-图像增强

1. 对一幅篇暗的图像进行线性幂变换，体验$\gamma$对图输出结果的影响。
<table align="center">
  <tr>
    <td>
      <img src="/images/posts/2020-03-19-digital_image_processing_yx2/4.1.png" width="332">
    </td>
    <td>
      <img src="/images/posts/2020-03-19-digital_image_processing_yx2/4.2.png" width="262">
    </td>
  </tr>
  <tr>
    <td>
      图：4.1.png
    </td>
    <td>
      图：4.2.png
    </td>
  </tr>
</table>
​	

 - 定义一个伽马变换的函数

   ```matlab
   function img2 = gamma_transform(img,C,X)
   	img1 = im2double(img);
   	img2 = C*(img1 .^ X); %进行伽马变换
   end
   ```

- 调用函数，对图二进行伽马变换

  ```matlab
  I_41 = imread('4.1.png');
  I_41 = rgb2gray(I_41);
  C1_X1 = gamma_transform(I_41, 1, 1);
  C1_X0_5 = gamma_transform(I_41, 1, 0.5);
  C1_X2 = gamma_transform(I_41, 1, 2);
  C1_X5 = gamma_transform(I_41, 1, 5);
  C1_X8 = gamma_transform(I_41, 1, 8);
  subplot(231); imshow(I_41);
  title('原始图像');
  subplot(232); imshow(C1_X1);
  title('C=1，X=1');
  subplot(233); imshow(C1_X0_5);
  title('C=1，X=0.5');
  subplot(234); imshow(C1_X2);
  title('C=1，X=2');
  subplot(235); imshow(C1_X5);
  title('C=1，X=5');
  subplot(236); imshow(C1_X8);
  title('C=1，X=8');
  ```

  运行结果：

  ![image-20200319015609042](/images/posts/2020-03-19-digital_image_processing_yx2/image-20200319015609042.png)

- 调用函数，对图二进行伽马变换

  ```matlab
  I_42 = imread('4.2.png');
  I_42 = rgb2gray(I_42);
  C1_X1 = gamma_transform(I_42, 1, 1);
  C1_X5 = gamma_transform(I_42, 1, 5);
  C1_X0_8 = gamma_transform(I_42, 1, 0.8);
  C1_X0_5 = gamma_transform(I_42, 1, 0.5);
  C1_X0_2 = gamma_transform(I_42, 1, 0.2);
  subplot(231); imshow(I_42);
  title('原始图像');
  subplot(232); imshow(C1_X1);
  title('C=1，X=1');
  subplot(233); imshow(C1_X5);
  title('C=1，X=5');
  subplot(234); imshow(C1_X0_8);
  title('C=1，X=0.8');
  subplot(235); imshow(C1_X0_5);
  title('C=1，X=0.5');
  subplot(236); imshow(C1_X0_2);
  title('C=1，X=0.2');
  ```

  运行结果：

  ![image-20200319015648074](/images/posts/2020-03-19-digital_image_processing_yx2/image-20200319015648074.png)


<br/>
**转载请注明：**[syljoy](https://syljoy.github.io/) » [数字图像处理练习](https://syljoy.github.io/2020/03/digital_image_processing_yx2/) 