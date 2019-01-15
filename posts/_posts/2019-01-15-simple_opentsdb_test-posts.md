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

![OpenTSDB ready](../../assets/img/post/simple_opentsdb_test_hbase_start.png)

HBase를 실행시켰다면, 이제 opentsdb 폴더 내의 build/ 폴더로 이동하여 OpenTSDB를 실행시킨다. 필자의 경우에는 아래와 같이 OpenTSDB를 실행시킨다. 참고로 --config 인자에는 opentsdb의 설정파일인 opentsdb.conf 파일의 위치를 지정해준다. 명령어 입력 후 아래의 사진처럼 맨 아래에 ‘Ready to Serve on …’ 이라는 문구가 나오고 커서가 정지해 있다면 정상적으로 OpenTSDB가 구동된 것이다.

```
cd /usr/local/opentsdb/build
sudo /usr/local/opentsdb/build/tsdb tsd --config=/usr/local/opentsdb/src/opentsdb.conf
```

![OpenTSDB ready](../../assets/img/post/install_opentsdb_3_tsdb_ready.png)


## [ Writing Data ]

OpenTSDB에 데이터를 입력하는 방법은 크게 3가지가 있다. 첫 번째는 Telnet API를 이용하는 것, 두 번째는 HTTP API를 이용하는 것, 세 번째는 file을 이용하여 batch import를 하는 것이다. 그러나 'Telnet API를 이용'하거나 'file을 이용하여 batch import(결국 Telnet API를 이용하는 방법임)'를 하는 것은 formatting 에러나 storage 에러가 일어나서 데이터 쓰기에 실패한 데이터를 판별할 수 없다. 따라서 [OpenTSDB Docunment - Writing Data](http://opentsdb.net/docs/build/html/user_guide/writing/index.html#input-methods)에서도 HTTP API를 이용하는 것을 권장한다.
<br/><br/>

여러개의 데이터를 입력할 때, request 당 데이터 포인트 수를 정해놓고 입력하는 것이 좋다. 너무 많은 데이터 포인트를 한번에 보내게 되면 request에 대한 응답을 받는데 시간이 오래 걸리기 때문이다. OpenTSDB Document - /api/put 에서는 request 당 50개의 데이터 포인트씩 보내는 것을 추천한다. 이 request 당 데이터 포인트 수는 컴퓨팅 리소스와 설정에 따라 달라질 수 있으니, 상황에 맞게 조절해서 쓰면 된다.
<br/><br/>

### 예제 코드

원래는 위에서 언급한 것처럼 request 당 데이터 포인트 수를 고려하여 코드를 작성해야 한다. 하지만 이 문서에서 다루는 심플 테스트에서는 두 개의 데이터 포인트만을 전송하는 간단한 예제이기 때문에 고려하지 않고 코드를 작성했다는 점을 이해하길 바란다.
<br/><br/>

아래의 코드에서 'TSDB_URL'이라는 변수는 OpenTSDB가 설치되어 있는 주소와 연결된 포트번호를 포함한다. 필자의 경우, 로컬에 포트번호 4242로 OpenTSDB가 연결되어 있으므로 아래와 같이 'http://localhost:4242'로 변수를 지정한 것이다. TSDB_URL을 본인의 상황에 맞게 변경해주고 아래의 코드를 돌렸을 때, 아래의 사진과 같이 Status code가 200 또는 204와 같은 값을 나타내고 빈 Response Content를 나타낼 것이다. 만일 'TSDB_URL'을 잘못 입력했거나, 잘못된 데이터 포인트를 입력했을 때는   다른 Status code 값과 비어있지 않은 Response Content를 나타내게 될 것이다. [자세한 내용은 이곳](http://opentsdb.net/docs/build/html/api_http/index.html#response-codes)을 참고하길 바란다.

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

