# Multipart-related commands {#concept_drn_rzs_vdb .concept}

Ossutil allows you to list the uploaded files and unfinished upload tasks \(UploadIDs\) in multipart uploads, and delete the upload files and unfinished upload tasks \(UploadIDs\) of a specified object.

For more information about the multipart upload, see [Multipart upload](../../../../intl.en-US/Developer Guide/Upload files/Multipart upload.md#).

**Note:** When uploading/copying a large file, ossutil automatically uses resumable upload without running the UploadPart command.

-   List an UploadID

    Run the `ls` command with the -m option to list all unfinished upload tasks \(UploadIDs\) of an object with a specified prefix, and run the `ls` command with the -a option to list all uploaded files and unfinished upload tasks \(UploadIDs\) of an object with a specified prefix.

    ```
    $ ossutil ls oss://bucket1/obj1 -m
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    UploadId Number is: 2
    0.070070(s) elapsed
    ```

    ```
    $ ossutil ls oss://bucket1/obj1 -a
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-05 14:36:21 +0000 CST        241561      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1/test.txt
    2016-04-08 14:50:47 +0000 CST       6476984      Standard    4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/obj1/sample.txt
    Object Number is: 2
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    UploadId Number is: 2
    0.091229(s) elapsed
    ```

-   Delete the data of a specified object

    Run the rm command to delete the unfinished upload tasks \(UploadIDs\) of a specified object.

    For example, if bucket1 contains the following objects, run the `ls` command to list all uploaded files and unfinished upload tasks \(UploadIDs\):

    ```
    $ ossutil ls oss://bucket1 -a
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-05 14:06:29 +0000 CST        201933      Standard   7E2F4A7F1AC9D2F0996E8332D5EA5B41        oss://bucket1/dir1/obj11
    2015-06-05 14:36:21 +0000 CST        241561      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1/test.txt
    2016-04-08 14:50:47 +0000 CST       6476984      Standard    4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/sample.txt
    Object Number is: 3
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    2017-01-13 03:45:25 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
    2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
    UploadId Number is: 4
    0.191289(s) elapsed
    ```

    Run the rm command with the -m option to delete the specified unfinished upload tasks \(UploadIDs\):

    ```
    $./ossutil rm -m oss://bucket1/obj1/test.txt
    Succeed: Total 1 uploadIds. Removed 1 uploadIds. 
    0.900715(s) elapsed
    ```

    Run the rm command with the -m and -r options to delete all unfinished upload tasks \(UploadIDs\) with a specified prefix:

    ```
    $./ossutil rm -m oss://bucket1/ob -r 
    Do you really mean to remove recursively multipart uploadIds of oss:bucket1/ob(y or N)? y 
    Succeed: Total 4 uploadIds. Removed 4 uploadIds. 
    1.922915(s) elapsed
    ```

    Run the rm command with the -a and -r options to delete all uploaded files and unfinished upload tasks \(UploadIDs\) with a specified prefix:

    ```
    $./ossutil rm  oss://hello-hangzws-1/obj -a -r
    Do you really mean to remove recursively objects and multipart uploadIds of oss://obj(y or N)? y
    Succeed: Total 1 objects, 3 uploadIds. Removed 1 objects, 3 uploadIds.
    ```


