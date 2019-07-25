# RestoreObject {#reference_mfr_5bx_wdb .reference}

RestoreObject接口用于解冻归档类型（Archive）的文件（Object）。

**说明：** 

-   RestoreObject接口只针对归档类型的Object，不适用于标准类型和低频访问类型的Object。
-   如果针对该Object第一次调用RestoreObject接口，返回202。
-   如果已经成功调用过RestoreObject接口，且Object已完成解冻，再次调用时返回200 OK。

## 解冻过程说明 {#section_qyj_p51_ygb .section}

归档类型的Object在执行解冻前后的状态变换过程如下：

1.  归档类型的Object初始时处于冷冻状态。
2.  提交一次解冻请求后，Object处于解冻中的状态。完成解冻任务通常需要1分钟，最长等待任务完成时间为4小时。
3.  服务端完成解冻任务后，Object进入解冻状态，此时您可以读取Object。解冻状态默认持续24小时，24小时内再次调用RestoreObject接口则解冻状态会自动延长24小时。对于同份归档文件，一次解冻流程内可有效调用7次RestoreObject接口达到最长7天的解冻持续时间。
4.  解冻状态结束后，Object再次返回到冷冻状态。

## 计费说明 {#section_oyj_p51_ygb .section}

状态变换过程中产生的相关费用如下：

-   对处于冷冻状态的Object执行解冻操作，会产生数据取回费用。
-   解冻状态最多延长7天。在此期间不再重复收取数据取回费用。
-   解冻状态结束后，Object又回到冷冻状态，再次执行解冻操作会收取数据取回费用。

## 请求语法 {#section_ivv_mcx_wdb .section}

``` {#codeblock_jfh_4oy_4r4}
POST /ObjectName?restore HTTP/1.1
Host: archive-bucket.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_qj3_scx_wdb .section}

-   首次对处于冷冻状态的Object提交解冻请求

    请求示例

    ``` {#codeblock_jlb_5uf_bsf}
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:28 GMT
    Authorization: OSS e1Unnbm1rgdnpI:y4eyu+4yje5ioRCr****
    ```

    返回示例

    ``` {#codeblock_opb_5hx_d6v}
    HTTP/1.1 202 Accepted
    Date: Sat, 15 Apr 2017 07:45:28 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    ```

-   对处于解冻中状态的Object提交解冻请求

    请求示例

    ``` {#codeblock_4om_u7w_wg9}
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdy4eyu+N****
    ```

    返回示例

    ``` {#codeblock_05i_2aj_usj}
    HTTP/1.1 409 Conflict
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Content-Length: 556
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    <?xml version="1.0" encoding="UTF-8"?>
    <Error>
      <Code>RestoreAlreadyInProgress</Code>
      <Message>The restore operation is in progress.</Message>
      <RequestId>58EAF141461FB42C2B000008</RequestId>
      <HostId>10.101.200.***</HostId>
    </Error>
    ```

-   对处于解冻状态的Object提交解冻请求

    请求示例

    ``` {#codeblock_4nf_dqd_urj}
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Authorization: OSS e1Unnbm1rgdnpI:u6O6FMJnn+WuBwbByZxm1+y4eyu+N****
    ```

    返回示例

    ``` {#codeblock_giw_slb_v97}
    HTTP/1.1 200 Ok
    Date: Sat, 15 Apr 2017 07:45:30 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    ```


## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../cn.zh-CN/SDK 示例/Java/管理文件/解冻归档文件.md)
-   [Python](../../../../cn.zh-CN/SDK 示例/Python/管理文件/解冻归档文件.md)
-   [PHP](../../../../cn.zh-CN/SDK 示例/PHP/管理文件/解冻归档文件.md)
-   [Go](../../../../cn.zh-CN/SDK 示例/Go/管理文件/解冻归档文件.md)
-   [C](../../../../cn.zh-CN/SDK 示例/C/管理文件/解冻归档文件.md)
-   [.NET](../../../../cn.zh-CN/SDK 示例/.NET/管理文件/解冻归档文件.md)

## 错误码 {#section_nfp_nfc_5gb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchKey|404|目标Object不存在。|
|OperationNotSupported|400|目标Object不是归档类型。|
|RestoreAlreadyInProgress|409|您已经成功调用过RestoreObject接口，且服务端正在执行解冻操作。请不要重复提交RestoreObject。|

