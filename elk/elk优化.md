```json
PUT _settings
    {
    "index": {
    "blocks": {
    "read_only_allow_delete": "false"
    }
    }
    }

GET _count
{
  "query": {
    "range": {
      "@timestamp": {
       "lt":"2019-05-10"
      }
    }
  }
}


DELETE /xinhua-log






POST xinhua-log/_delete_by_query?conflicts=proceed&scroll_size=10000
{
  "query": {
    "range": {
      "@timestamp": {
       "lt":"2019-05-10"
      }
    }
  }
}

curl -XPOST http://localhost:9200/searchrecord/_forcemerge?only_expunge_deletes=true

```

### 过往索引文档数据，存放到内存中导致内存溢出
