---
layout: post
categories: posts
title: OpenTSDB Install-(2) HBase 설치
---

이 문서는 기본적으로 [OpenTSDB 설치 Document](http://opentsdb.net/docs/build/html/installation.html)를 참고하여 작성하였다. 또한 Linux 배포판 중의 하나인 Ubuntu를 기준으로 작성하였다.

## [ HBase 다운로드 및 설치 ]
OpenTSDB는 기본적으로 HBase 위에서 동작하는 시계열 데이터베이스이기때문에 HBase 설치가 필요하다. HBase를 동작시키는 모드는 크게 'Standalone 모드'와 'Distributed 모드'로 나뉜다. 이 문서에서는 OpenTSDB를 설치하기 위한 과정을 간단히 설명하기 위해 'Standalone 모드'로 동작시키는 것을 기준으로 작성할 것이다. ~~(사실 Distributed 모드로 시스템을 구축해본 적이 없기도 하다. 원하시는 독자분들은 이곳을 참고해보시길 http://hbase.apache.org/0.94/book/standalone_dist.html)~~
<br/>

![HBase logo](../../assets/img/post/install_opentsdb_2_hbase_logo.png)


### 1. Download
구글에서 apache hbase download를 검색하거나 직접 HBase 다운로드 페이지에 접속한다. 이 글을 작성하는 시점(2019-01-09)에서 다운로드 페이지는 아래의 링크와 같다.<br/><br/>
[https://hbase.apache.org/downloads.html](https://hbase.apache.org/downloads.html)
<br/>

HBase 다운로드 페이지로 가면 아래의 이미지에서와 같이 각 버전별 다운로드 URL 링크가 있다. 다운로드 파일에는 src(source packetage) 파일과 bin(binary packetage) 파일 형식이 있지만, 특별한 설정을 하려고 하는게 아닌 이상 간편한게 최고니까 bin 파일을 다운받로록 하자.
<br/>

![hbase download page](../../assets/img/post/install_opentsdb_2_hbase_download_page1.png)

해당하는 링크를 누르고나서 파일이 다운로드 되는 것이 아니라 아래와 같은 화면이 나올 수 있는데, 당황하지말고 페이지에서 하라는대로 다운로드 url을 눌러준다.
<br/>

![hbase download page2](../../assets/img/post/install_opentsdb_2_hbase_download_page2.png)

이렇게 필자는 hbase-2.1.2-bin.tar.gz 파일을 다운받았다.

### 2. Install
작성중...

### 3. Configuration
작성중...

### 4. Check
작성중...