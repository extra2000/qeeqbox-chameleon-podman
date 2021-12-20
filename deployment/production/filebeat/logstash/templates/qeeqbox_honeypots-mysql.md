Execute the following request on Kibana's Dev Tools:
```
PUT _index_template/qeeqbox_honeypots-mysql
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
      "_source": {
        "excludes": [],
        "includes": [],
        "enabled": true
      },
      "_routing": {
        "required": false
      },
      "dynamic": "strict",
      "dynamic_templates": [],
      "properties": {
        "@timestamp": {
          "type": "date"
        },
        "honeypot": {
          "type": "object",
          "properties": {
            "client": {
              "type": "object",
              "properties": {
                "ip": {
                  "type": "ip"
                },
                "port": {
                  "type": "integer"
                }
              }
            },
            "login": {
              "type": "object",
              "properties": {
                "password": {
                  "type": "keyword"
                },
                "status": {
                  "type": "keyword"
                },
                "username": {
                  "type": "keyword"
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
            "timestamp": {
              "type": "date"
            }
          }
        },
        "tags": {
          "type": "keyword"
        }
      }
    }
  },
  "index_patterns": [
    "qeeqbox_honeypots-mysql_*"
  ],
  "data_stream": {
    "hidden": false
  },
  "composed_of": []
}
```
