---
layout: post
categories: posts
title: Simple OpenTSDB Test - Write & Query(with web UI) Data 
---

이 문서에서는 HTTP API를 이용한 데이터 입력하고, 입력한 데이터를 OpenTSDB web UI를 이용하여 확인하는 방법에 대하여 설명한다.

이 문서는 기본적으로 [OpenTSDB Document](http://opentsdb.net/docs/build/html/index.html#)를 참고하여 작성하였다. 또한 Linux 배포판 중의 하나인 Ubuntu를 기준으로 작성하였다. 실행코드는 python2를 기준으로 작성하였다.


## [ OpenTSDB 실행 ]

작성중..


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
    
    print("[PUT]: %s" % (json.dumps(buf, ensure_ascii=False, indent=4)))
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



## [ Querying Data with OpenTSDB web UI]

작성중..

