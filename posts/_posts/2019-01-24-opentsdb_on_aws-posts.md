---
layout: post
categories: posts
title: AWS에서 OpenTSDB 구축하기
---

이 문서에서는 AWS(Amazon Web Services)에서 S3 기반으로 HBase를 구축하는 방법부터 그 위에 OpenTSDB를 설치하는 방법까지 설명한다.

이 문서는 기본적으로 [HBase on Amazon S3 (Amazon S3 Storage Mode)](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hbase-s3.html)를 참고하여 작성하였다. 


## [ HBase 환경 구축 ]

AWS에서는 Amazon EMR을 통해 HBase를 사용할 수 있다. 그리고 Amazon EMR 버전 5.2.0 이상에서 HBase 실행할 경우 Amazon S3 기반 HBase를 활성화할 수 있다. HDFS가 아닌 S3를 기반으로 HBase를 구축하게 되면 속도는 느리겠지만, 비용 측면에서는 많은 절감을 이룰 수 있다. 이외에도 더 구체적인 Amazon S3 기반 HBase 환경의 장점은 [HBase on Amazon S3 (Amazon S3 Storage Mode)](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-hbase-s3.html)에서 자세히 확인할 수 있다.

### 1. Amazon S3 생성하기

Amazon S3(Simple Storage Service)를 생성하기 위해 우선 AWS 페이지에서 콘솔에 로그인을 한다.

![AWS console login](../../assets/img/post/opentsdb_on_aws_login_console.png)

<br/>

콘솔에 로그인을 하게 되면 아래의 사진과 같은 콘솔 페이지가 나오는데, 여기서 스토리지란에 있는 S3를 클릭한다.

![AWS console login](../../assets/img/post/opentsdb_on_aws_console.png)



### 2. Amazon EMR 생성하기

작성중..

## [ OpenTSDB 설치 ]

### 1. OpenTSDB 설치 및 설정

작성중..

### 2. OpenTSDB Check

작성중..
