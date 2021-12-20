Execute the following request on Kibana's Dev Tools:
```
PUT _index_template/qeeqbox_honeypots-http
{
  "version": 1,
  "template": {
    "settings": {
      "index": {
        "lifecycle": {
          "name": "qeeqbox_honeypots"
        },
        "codec": "best_compression",
        "query": {
          "default_field": [
            "honeypot.client.ip"
          ]
        }
      }
    },
    "mappings": {
      "dynamic": true,
      "numeric_detection": false,
      "date_detection": true,
      "dynamic_date_formats": [
        "strict_date_optional_time",
        "yyyy/MM/dd HH:mm:ss Z||yyyy/MM/dd Z"
      ],
      "_source": {
        "enabled": true,
        "includes": [],
        "excludes": []
      },
      "_routing": {
        "required": false
      },
      "dynamic_templates": [
        {
          "fields": {
            "mapping": {
              "type": "keyword"
            },
            "path_match": "honeypot.fields.*"
          }
        }
      ],
      "properties": {
        "@timestamp": {
          "type": "date"
        },
        "tags": {
          "type": "keyword"
        },
        "honeypot": {
          "type": "object",
          "properties": {
            "timestamp": {
              "type": "date"
            },
            "property": {
              "type": "object",
              "properties": {
                "organization": {
                  "type": "object",
                  "properties": {
                    "name": {
                      "type": "constant_keyword"
                    }
                  }
                },
                "project": {
                  "type": "object",
                  "properties": {
                    "name": {
                      "type": "constant_keyword"
                    }
                  }
                },
                "sensor": {
                  "type": "object",
                  "properties": {
                    "name": {
                      "type": "constant_keyword"
                    },
                    "ip": {
                      "type": "ip"
                    }
                  }
                }
              }
            },
            "module": {
              "type": "object",
              "properties": {
                "name": {
                  "type": "constant_keyword"
                }
              }
            },
            "client": {
              "type": "object",
              "properties": {
                "ip": {
                  "type": "ip"
                }
              }
            },
            "request": {
              "type": "object",
              "properties": {
                "fields": {
                  "type": "object"
                }
              }
            }
          }
        }
      }
    }
  },
  "index_patterns": [
    "qeeqbox_honeypots-http-*",
    "qeeqbox_honeypots-https-*"
  ],
  "data_stream": {}
}
```
