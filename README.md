# elasticsearch-cluster
One of the most often asked question about the ELK stack is how can i monitor the elastic nodes itself. Monitoring the nodes here includes all indexes, all the data nodes, index size, total index size, etc. One tool that i use for my implementations is Cerebro.

Monitor our cluster-
```
http://elasticsearch.elasticsearch-cluster.svc.cluster.local:9200
```
Add sample data-  ref: https://github.com/oliver006/elasticsearch-test-data
```
python es_test_data.py --es_url=http://elasticsearch.example.com:9200  --num_of_shards=4
```
We are using index_name=test_data

Add replica-
```
curl -XPUT http://elasticsearch.example.com:9200/test_data/_settings -d '{"settings": {"index": {"number_of_replicas": 2}}}'
```

Get details-
```
curl -XGET http://elasticsearch.example.com:9200/_cat/shards/test_data\?v
```

Add node-
```
kubectl patch statefulsets elasticsearch -n elasticsearch-cluster -p '{"spec":{"replicas":4}}'
```
Delete node-
```
kubectl patch statefulsets elasticsearch -n elasticsearch-cluster -p '{"spec":{"replicas":3}}'
```
It will take some time. Observe: `kubectl get pod -n elasticsearch-cluster -w`

