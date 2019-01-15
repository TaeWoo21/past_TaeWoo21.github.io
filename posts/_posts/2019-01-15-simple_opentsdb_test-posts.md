---
layout: post
categories: posts
title: Simple OpenTSDB Test - OpenTSDB 실행부터 Write & Query(with web UI) Data 
---

이 문서에서는 설치된 OpenTSDB를 실행하는 방법부터 HTTP API를 이용한 데이터 입력하고, 입력한 데이터를 OpenTSDB web UI를 이용하여 확인하는 방법까지 설명한다.

이 문서는 기본적으로 [OpenTSDB Document](http://opentsdb.net/docs/build/html/index.html#)를 참고하여 작성하였다. 또한 Linux 배포판 중의 하나인 Ubuntu를 기준으로 작성하였다. 실행코드는 python2를 기준으로 작성하였다.


## [ OpenTSDB 실행 ]

설치된 OpenTSDB를 실행시키기 전에는 무조건 hbase_start.sh 쉘스크립트를 이용하여 HBase를 먼저 실행시켜야 한다. HBase를 동작시킨 뒤, 약 10초 정도 뒤에 OpenTSDB를  실행시키면 된다. 참고로 HBase와 OpenTSDB를 실행시킬때는 sudo 권한을 주고 실행시켜야 한다.
<br/><br/>

HBase가 설치되어있는 폴더 내의 bin/ 폴더를 보면 hbase_start.sh 쉘스크립트가 있다. 필자의 경우에는 아래와 같이 HBase를 실행시킨다. HBase를 실행시켰을 때, 아래와 같은 모습이 나오면 성공적으로 실행된 것이다.

```
cd /usr/local/hbase/hbase-2.1.2/bin
sudo /usr/local/hbase/hbase-2.1.2/bin/start-hbase.sh
```

HBase를 실행시켰다면, 이제 opentsdb 폴더 내의 build/ 폴더로 이동하여 OpenTSDB를 실행시킨다. 필자의 경우에는 아래와 같이 OpenTSDB를 실행시킨다. 참고로 --config 인자에는 opentsdb의 설정파일인 opentsdb.conf 파일의 위치를 지정해준다. 명령어 입력 후 아래의 사진처럼 맨 아래에 ‘Ready to Serve on …’ 이라는 문구가 나오고 커서가 정지해 있다면 정상적으로 OpenTSDB가 구동된 것이다.

```
cd /usr/local/opentsdb/build
sudo /usr/local/opentsdb/build/tsdb tsd --config=/usr/local/opentsdb/src/opentsdb.conf
```

![OpenTSDB ready](../../assets/img/post/install_opentsdb_3_tsdb_ready.png)


## [ Writing Data ]

```python
import json
import requests
import time
from datetime import datetime

TSDB_URL = 'http://localhost:4242'
METRIC = 'simple_test'

TIME_1 = '2019-01-15 13:19:24'
VALUE_1 = 7
TAGS_1 = {
    'content': 'data_A',
    'made_by': 'TW'
}

TIME_2 = '2019-01-15 14:52:18'
VALUE_2 = 10.3
TAGS_2 = {
    'content': 'data_B',
    'made_by': 'TW'
}

def str_to_timestamp(t_string):
    d_time = datetime.strptime(t_string, '%Y-%m-%d %H:%M:%S')
    timestamp = int(time.mktime(d_time.timetuple()))
    return timestamp

def make_datapoint(metric, time, value, tags):
    dp = dict()
    dp['metric'] = metric
    dp['timestamp'] = str_to_timestamp(time)
    dp['value'] = value
    dp['tags'] = tags
    return dp

def make_puturl(url):
    if url[-1] is '/':
        put_url = url + 'api/put'
    else:
        put_url = url + '/api/put'
    return put_url

def sendbuf(session, url, buf):
    put_url = make_puturl(url)
    print("[URL]: %s" % put_url)
    headers = {'content-typ': 'application/json'}
    response = session.post(put_url, data=json.dumps(buf), headers=headers)
    
    print("[PUT]: \n%s" % (json.dumps(buf, ensure_ascii=False, indent=4)))
    print("[Status Code]: %s" % (response.status_code))
    print("[Response Content]: %s" % (response.content))

    
if __name__ == '__main__':
    buf = []
    dp_1 = make_datapoint(METRIC, TIME_1, VALUE_1, TAGS_1)
    dp_2 = make_datapoint(METRIC, TIME_2, VALUE_2, TAGS_2)
    buf.append(dp_1)
    buf.append(dp_2)
    
    with requests.Session() as s:
        sendbuf(s, TSDB_URL, buf)
```

![build.sh success](../../assets/img/post/simple_opentsdb_test_put_result.png)

## [ Querying Data with OpenTSDB web UI]

작성중..

