# JavaScript客户端签名直传 {#concept_frd_4gy_5db .concept}

本文主要介绍如何基于POST Policy的使用规则在客户端通过JavaScript代码完成签名，然后通过表单直传数据到OSS。

## 步骤 1：下载应用服务器代码 {#section_ugn_1ky_5db .section}

[下载地址](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-direct.zip)

本示例采用Plupload 直接提交表单数据（即[PostObject](../../../../../cn.zh-CN/API 参考/关于Object操作/PostObject.md#)）到OSS，可以运行在PC浏览器、手机浏览器、微信等。您可以同时选择多个文件上传，并设置上传到指定目录和设置上传文件名称是随机文件名还是本地文件名。您还可以通过进度条查看上传进度。

## 步骤 2：修改配置文件 {#section_jty_1tm_q2b .section}

将下载包解压后，修改upload.js文件：

```
accessid= '<yourAccessKeyId>';
accesskey= <yourAccessKeySecrety>';
.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': accessid, 
        ....
    };

//如果是STS方式====
accessid = 'STS.ACCESSKEYID';
accesskey = 'STS.ACCESSSECRET';
token = 'STS.token';

.....
new_multipart_params = {
        ....
        'OSSAccessKeyId': accessid, 
        'x-oss-security-token':token
        ....
    };
//===========
host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com';
```

-   $id：您的AccessKeyId
-   $key：您的AcessKeySecret
-   $host：格式为`BucketName.Endpoint`，例如`post-test.oss-cn-hangzhou.aliyuncs.com`

    **说明：** 关于Endpoint的介绍，请参见[Endpoint（访问域名）](../../../../../cn.zh-CN/开发指南/基本概念介绍.md#section_s3j_nmt_tdb)。


## 步骤 3：设置CORS {#section_jgc_3mk_p2b .section}

客户端进行表单直传到OSS时，会从浏览器向OSS发送带有`Origin`的请求消息。OSS对带有`Origin`头的请求消息会进行跨域规则（CORS）的验证。因此需要为Bucket设置跨域规则以支持Post方法。

具体操作步骤请参见[设置跨域访问](../../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置跨域访问.md#)。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21672/155020996512308_zh-CN.png)

**说明：** **来源**设置为 \* 是为了使用方便，不确保安全性。建议您填写自己需要的域名。

## 步骤 4：体验JavaScript客户端签名直传 {#section_ypv_ytm_q2b .section}

1.  将应用服务器代码zip包解压到Web根目录下。
2.  在Web浏览器中输入`<Web应用服务器地址>/oss-h5-upload-js-direct/index.html`，例如`http://abc.com:8080/oss-h5-upload-js-direct/index.html`。
3.  选择一个或多个文件进行上传。
4.  上传成功后，通过控制台查看上传结果。

## 核心代码解析 {#section_vyn_czq_p2b .section}

因为OSS支持POST协议，所以只要在Plupload发送POST请求时带上OSS签名即可。核心代码如下：

```
var uploader = new plupload.Uploader({
    runtimes : 'html5,flash,silverlight,html4',
    browse_button : 'selectfiles',
    //runtimes : 'flash',
    container: document.getElementById('container'),
    flash_swf_url : 'lib/plupload-2.1.2/js/Moxie.swf',
    silverlight_xap_url : 'lib/plupload-2.1.2/js/Moxie.xap',
    url : host,
    multipart_params: {
        'Filename': '${filename}',
        'key' : '${filename}',
        'policy': policyBase64,
        'OSSAccessKeyId': accessid,
        'success_action_status' : '200', //让服务端返回200，不设置则默认返回204
        'signature': signature,
        'x-oss-security-token':token
    },
　....
}
```

上述代码中，`’Filename’: ‘${filename}’`表示上传后保持原来的文件名。如果您想上传到特定目录如abc下，且文件名不变，请修改代码如下：

```
multipart_params: {
        'Filename': 'abc/' + '${filename}',
        'key' : '${filename}',
        'policy': policyBase64,
        'OSSAccessKeyId': accessid,
        'success_action_status' : '200', //让服务端返回200，不设置则默认返回204
        'signature': signature,
    },
```

-   设置成随机文件名

    如果想在上传时固定设置成随机文件名，后缀保持跟客户端文件一致，可以将函数改为：

    ```
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

-   设置成用户的文件名

    如果想在上传时固定设置成用户的文件名，可以将函数改为：

    ```
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   设置上传目录

    您可以将文件上传到指定目录下。下面的代码是将上传目录改成abc/，注意目录必须以正斜线（/）结尾。

    ```
    function get_dirname()
    {
        g_dirname = "abc/"; 
    }
    ```

-   上传签名

    上传签名主要是对policyText进行签名，最简单的例子如下：

    ```
    var policyText = {
        "expiration": "2020-01-01T12:00:00.000Z", // 设置Policy的失效时间，如果超过失效时间，就无法通过此Policy上传文件
        "conditions": [
        ["content-length-range", 0, 1048576000] // 设置上传文件的大小限制，如果超过限制，文件上传到OSS会报错
        ]
    }
    ```


## 总结 {#section_gzz_dnm_q2b .section}

在客户端通过JavaScript代码完成签名，无需过多配置，即可实现直传，非常方便。但是客户端通过JavaScript把AccesssKeyID 和AccessKeySecret写在代码里面有泄露的风险，建议采用[服务端签名后直传](cn.zh-CN/最佳实践/Web端直传实践/服务端签名后直传.md#)。

