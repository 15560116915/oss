# Image style

Adding all the changes to the image after the URL makes the URL too long and inconvenient for management and reading. IMG allows you to save common operations as an alias, that is, a style. With the style, a complicated operation can be performed through a short URL.

Multiple styles \(50 at most\) are grouped under a bucket. Each style is effective only within the bucket.

## Style access rules

**URL parameters**

```
<File URL>? x-oss-process=style/<StyleName>
```

Example:

`bucket.aliyuncs.com/sample.jpg? x-oss-process=style/stylename`

This is the default style access method supported by IMG.

**Separators**

```
<File URL><Separator><StyleName>
```

Example:

`bucket.aliyuncs.com/sample.jpg{separator}stylename`

IMG regards the content after the separator in a URL as the style name. This is an optional method provided by IMG. You must set separators in the console. Separators such as `-`, `_`, `/`, and `!` are supported.

-   StyleNameindicates the name of a style.
-   Style creations, deletions, and modifications are all performed in the console.
-   When the requested style does not exist in the specified bucket, the system returns the error NotSuchStyle.

## Set separators

1.  In the left-side bucket list of the [OSS console](https://oss.console.aliyun.com/overview), click the bucket to which you want to set separators.
2.  Click the **Image Processing** tab, and then click **Access Settings**. As shown in the following figure:

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/15674147552882_en-US.png)

3.  In the **Access Settings** dialog box, set the following parameters:

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/15674147552883_en-US.png)

    -   Source Image Protection: After enabling the original image protection, you can only access the image file by passing in the StyleName or using a signature-based method. Direct accesses to the OSS original file or accesses by passing in image parameters and modifying the image style are not allowed.
    -   Customize separator
    Click **OK**.


## Example

In this example, a style is created in the bucket image-demo.

|Style name|Style content|
|----------|-------------|
|panda\_style|image/resize,m\_fill,w\_300,h\_300,limit\_0/auto-orient,0/quality,q\_90/watermark,image\_cGFuZGEucG5n,t\_61,g\_se,y\_10,x\_10|

-   Access through parameters

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m\_fill,w\_300,h\_300,limit\_0/auto-orient,0/quality,q\_90/watermark,image\_cGFuZGEucG5n,t\_61,g\_se,y\_10,x\_10](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,m_fill,w_300,h_300,limit_0/auto-orient,0/quality,q_90/watermark,image_cGFuZGEucG5n,t_61,g_se,y_10,x_10)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/15674147552884_en-US.jpg)

-   Access through URL parameters in style mode

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/panda\_style](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/panda_style)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/15674147552885_en-US.jpg)

-   Access through style separators in style mode

    [http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg@!panda\_style](http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg@!panda_style)

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4792/156741475658723_en-US.jpg)


These three methods bring the same result.

