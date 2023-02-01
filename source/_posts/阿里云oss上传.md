---
title: 阿里云oss文件上传
description: 阿里云oss文件上传
data: 2022-12-11 19:50:00
updata: 2022-12-11 19:50:00
tags: 
  - java
categories:
  - java笔记
---
# 阿里云oss文件上传

### 所需依赖

```xml
        <dependency>
            <groupId>com.aliyun.oss</groupId>
            <artifactId>aliyun-sdk-oss</artifactId>
            <version>3.10.2</version>
        </dependency>
```

### 其次获取到目标阿里云的AccessKey ID和AccessKey Secret填入yml中，如图所示

```yaml
aliyun:
  oss:
    file:
      endpoint: oss-cn-hangzhou.aliyuncs.com   // 节点，此处为（华东1）杭州
      keyid: LxxxxxxxxxxxxxW
      keysecret: txxxxxxxxxxxRM
      bucketname: leebook     // 目标桶名称
```

### 编写OSS工具类OssUtils

```java
package com.ll.aliyunoss.utils;

import org.springframework.beans.factory.InitializingBean;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.stereotype.Component;

/**
 * Created by a handsome man
 *
 * @Author: mental_test
 * @Date: 2022/12/08/22:42
 * @Description:
 */

/**
 * 交给spring管理
 */
@Component
public class OssUtils implements InitializingBean {

    /**
     * 读取配置文件
     */
    @Value("${aliyun.oss.file.endpoint}")
    private String endpoint;

    @Value("${aliyun.oss.file.keyid}")
    private String keyid;

    @Value("${aliyun.oss.file.keysecret}")
    private String keysecret;

    @Value("${aliyun.oss.file.bucketname}")
    private String bucketname;

    /**
     * 定义公开静态常量
     * 当下面值初始化完成后接口会执行
     * 通过下面的公共方法就可以在别处引用变量
     */
    public static String ENDPOINT;
    public static String KEY_ID;
    public static String KEY_SECRET;
    public static String BUCKET_NAME;

    @Override
    public void afterPropertiesSet() throws Exception {
        ENDPOINT = endpoint;
        KEY_ID = keyid;
        KEY_SECRET = keysecret;
        BUCKET_NAME = bucketname;
    }
}

```

### 具体执行代码

```java
   @Override
    public String uploadFile(MultipartFile file) {
        // 获取oss参数
        String endpoint = OssUtils.ENDPOINT;
        String keyId = OssUtils.KEY_ID;
        String keySecret = OssUtils.KEY_SECRET;
        String bucketName = OssUtils.BUCKET_NAME;


        try {
            OSS ossClient = new OSSClientBuilder().build(endpoint, keyId, keySecret);

            // 文件上传工作流
            InputStream inputStream = file.getInputStream();

            // 获取文件名称
            String filename = file.getOriginalFilename();



            // **若文件名重复，新的会自动覆盖旧的文件，所以需要对文件进行区分
            // 此处我用的是时间日期分类
            String datePath = new DateTime().toString("yyyy-MM-dd");
            // 拼接
            filename = datePath + filename;

            // 调用oss方法实现上传
            // 参数1：Bucket名称
            // 参数2：上传到oss的文件路径和文件名
            // 参数3： 上传文件输入流
            ossClient.putObject(bucketName,filename,inputStream);
            // 关闭流
            ossClient.shutdown();

            // 上传完文件后将文件路径返回
            String url = "https://" + bucketName + "." + endpoint + "/" + filename;

            return url;
        } catch (Exception e) {
            e.printStackTrace();
            return null;
        }
    }
```

