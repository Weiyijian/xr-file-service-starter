һ������������ʹ���ĸ��������ļ��ϴ��������Ӧ���������У�����ȫ�����룩��

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

����������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������
����application.yml �������£��������²�����װ�ģ�Ϊ��������ʧ�ܣ��������ָ�������еĲ�����Ӧ��ֵ����

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
  moduleName: �ļ������ƣ��磺java��

tencent-cloud:
  secretId: your secretId
  secretKey: your secretKey
  filePath: https://weiyijian-upload-1312337739.cos.ap-beijing.myqcloud.com
  bucketName: weiyijian-upload-1312337739
  regionName: ap-beijing
  moduleName: �ļ������ƣ��磺java��

qiniuyun:
  accessKey: your accessKey
  secretKey: your secretKey
  domainOfBucket: qiniuyun.fzhxfw.xyz
  bucketName: weiyijian-upload
  # [{'zone0':'����'}, {'zone1':'����'},{'zone2':'����'},{'zoneNa0':'����'},{'zoneAs0':'����'}]
  zoneName: zone2
  # ���ӹ���ʱ�䣬��λ���룬3600����1Сʱ��-1������������
  expireInSeconds: -1

fdfs:
  # ��������ַ
  tracker-list: ������IP:22122
  # ���ӳ�ʱ
  connect-timeout: 60
  # ��ȡʱ��
  so-timeout: 60
  # ��������ͼ����
  thumb-image:
    width: 150
    height: 150
������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������
�������Լ�����Ŀ���½�һ��ɨ������ĳ�ʼ�������࣬�������£�

package com.example.test.config;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan("com.xr")
public class initConfig {

}
������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������������
�ġ�ע���Ӧ�����������ö�Ӧ�������ɣ�
���磺

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
ע�⣺�ļ���Զ��url����װ����R�������棬��ȡ ���ϴ��ļ�URL ��Ҫʹ��HuTool�������MapUtil.getStr(r,"data")��ȡ��
HuTool������
      <dependency>
          <groupId>cn.hutool</groupId>
          <artifactId>hutool-all</artifactId>
          <version>5.8.7</version>
      </dependency>
