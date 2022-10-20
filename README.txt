一、导入依赖（使用哪个第三方文件上传服务导入对应的依赖就行，不用全部导入）：

<dependency>
    <groupId>io.github.xrfzh.cn</groupId>
    <artifactId>aliyun-file-service-starter</artifactId>
    <version>2.2.0-RELEASE</version>
</dependency>
<dependency>
    <groupId>io.github.xrfzh.cn</groupId>
    <artifactId>minio-file-service-starter</artifactId>
    <version>2.2.0-RELEASE</version>
</dependency>
<dependency>
    <groupId>io.github.xrfzh.cn</groupId>
    <artifactId>tencentcloud-file-service-starter</artifactId>
    <version>2.2.0-RELEASE</version>
</dependency>
<dependency>
    <groupId>io.github.xrfzh.cn</groupId>
    <artifactId>qiniuyun-file-service-starter</artifactId>
    <version>2.2.0-RELEASE</version>
</dependency>
<dependency>
    <groupId>io.github.xrfzh.cn</groupId>
    <artifactId>fastdfs-file-service-starter</artifactId>
    <version>2.2.0-RELEASE</version>
</dependency>
<dependency>
    <groupId>io.github.xrfzh.cn</groupId>
    <artifactId>xr-common-utils</artifactId>
    <version>2.2.0-RELEASE</version>
</dependency>

<repositories>
    <repository>
	<id>maven-central[稳定发行版2.2.2-RELEASE]</id>
        <url>https://repo.maven.apache.org/maven2/</url>
    </repository>
    <repository>
        <id>xr-file-service-starter[稳定发行版2.2.0-RELEASE]</id>
        <url>https://github.com/Weiyijian/xr-file-service-starter/tree/master/repository/</url>
    </repository>
</repositories>

——————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
二、application.yml 配置如下（按照如下参数封装的，为避免启动失败，建议填好指定配置中的参数对应的值）：

minio:
  accessKey: your accessKey
  secretKey: your secretKey
  endPoint: http://42.192.150.29:9000
  bucketName: fzh-public

aliyun:
  keyId: your keyId
  keySecret: your keySecret
  endPoint: oss-cn-shenzhen.aliyuncs.com
  bucketName: weiyijian-upload
  moduleName: 文件夹名称（如：java）

tencent-cloud:
  secretId: your secretId
  secretKey: your secretKey
  filePath: https://weiyijian-upload-1312337739.cos.ap-beijing.myqcloud.com
  bucketName: weiyijian-upload-1312337739
  regionName: ap-beijing
  moduleName: 文件夹名称（如：java）

qiniuyun:
  accessKey: your accessKey
  secretKey: your secretKey
  domainOfBucket: qiniuyun.fzhxfw.xyz
  bucketName: weiyijian-upload
  # [{'zone0':'华东'}, {'zone1':'华北'},{'zone2':'华南'},{'zoneNa0':'北美'},{'zoneAs0':'其他'}]
  zoneName: zone2
  # 链接过期时间，单位是秒，3600代表1小时，-1代表永不过期
  expireInSeconds: -1

fdfs:
  # 服务器地址
  tracker-list: 服务器IP:22122
  # 连接超时
  connect-timeout: 60
  # 读取时间
  so-timeout: 60
  # 生成缩略图参数
  thumb-image:
    width: 150
    height: 150
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
三、在自己的项目中新建一个扫描组件的初始化配置类，内容如下：

package com.example.test.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.xr")
public class InitConfig {

}
————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————————
四、注入对应的依赖，调用对应方法即可：
例如：

@Resource
    private AliyunFileService aliyunFileService;

    @PostMapping("/uploadByAliyun")
    public R uploadByAliyun(MultipartFile file) {
        R r = aliyunFileService.uploadFile(file);
        String url = MapUtil.getStr(r, "data");
        System.out.println(url);
        return R.okResult().put("url", url);
    }

    @Resource
    private MinioFileService minioFileService;

    @PostMapping("/uploadByMinio")
    public R uploadByMinio(MultipartFile file) {
        R r = minioFileService.uploadMultipartFile(file);
        String url = MapUtil.getStr(r, "data");
        System.out.println(url);
        return R.okResult().put("url", url);
    }

    @Resource
    private TencentCloudFileService tencentCloudFileService;

    @PostMapping("/uploadByTencentCloud")
    public R uploadByTencentCloud(MultipartFile file) {
        R r = tencentCloudFileService.uploadFile(file);
        String url = MapUtil.getStr(r, "data");
        System.out.println(url);
        return R.okResult().put("url", url);
    }

    @Resource
    private QiniuyunFileService qiniuyunFileService;

    @PostMapping("/uploadByQiniuyun")
    public R uploadByQiniuyun(MultipartFile file) {
        R r = qiniuyunFileService.uploadFile(file);
        String url = MapUtil.getStr(r, "data");
        System.out.println(url);
        return R.okResult().put("url", url);
    }

    @Resource
    private FastDfsFileService fastDfsFileService;

    @PostMapping("/uploadByFastDfs")
    public R uploadByFastDfs(MultipartFile file) {
        R r = fastDfsFileService.uploadFile(file);
        String url = MapUtil.getStr(r, "data");
        System.out.println(url);
        return R.okResult().put("url", url);
    }
注意：文件的远程url被封装在了R对象里面，获取 已上传文件URL 需要使用HuTool工具类的MapUtil.getStr(r,"data")获取。
HuTool依赖：
      <dependency>
          <groupId>cn.hutool</groupId>
          <artifactId>hutool-all</artifactId>
          <version>5.8.7</version>
      </dependency>
