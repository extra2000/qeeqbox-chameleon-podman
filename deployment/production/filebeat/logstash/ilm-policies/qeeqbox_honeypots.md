Execute the following request on Kibana's Dev Tools:
```
PUT _ilm/policy/qeeqbox_honeypots
{
  "policy": {
    "phases": {
      "hot": {
        "actions": {
          "rollover": {
            "max_age": "365d",
            "max_primary_shard_size": "50gb"
          },
          "set_priority": {
            "priority": 100
          },
          "forcemerge": {
            "max_num_segments": 1,
            "index_codec": "best_compression"
          },
          "shrink": {
            "number_of_shards": 1
          },
          "readonly": {}
        },
        "min_age": "0ms"
      }
    }
  }
}
```
