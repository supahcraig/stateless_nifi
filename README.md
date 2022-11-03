# Stateless NiFi



Extremely high level instructions:

1.  Create a Streams Messaging Manager data hub
2.  Create a new Source connector under "Connect"
3.  Select StatelessNifiSourceConnector
4.  Click the pencil icon next to the `flow.snapshot` config parameter
5.  Browse to `Stateless_Nifi_Source.json` (found in this repo), click `Save`
6.  Adjust the configutarion
  * give it a name
  * `output.port` should match the name of the output port in the flow definition.  `send_to_kafka` if you're using the flow defs found in this repo
  * `topics` is the name of the Kafka topic you want to publish to.  It does not need to be created ahead of time
7.  Click validate, next, deploy


For the sink connector, similar steps to the source connector, most notably click `Save and Enhance` instead of `Save` in order to bring in the nifi parameter context.   Sample values are included in the parameter values.

* `parameter.pcontext:bucket_uri` ==> your bucket name, of the form `s3a://your-bucket-name`
* `parameter.pcontext:bucket_folder` ==> the subfolder within your bucket, of the form `/folder/structure/`
* `parameter.pcontext:cdp_username` ==> your CDP workload username
* `parameter.pcontext:cdp_password` ==> your CDP workload password
* `input.port` ==> should be the name of the input port.  `receive_kafka` if you're using the flow def found in this repo
* `topics` ==> should match the topic name your source connector is publishing to

CLick validate, next, deploy.


This process writes parquet to your S3 location.  If you want to query that data, run this from an Impala virtual warehouse, being sure to change the location to your bucket/folder.


```
CREATE EXTERNAL TABLE default.snifi (id STRING, ts STRING) 
STORED AS PARQUET
LOCATION 's3a://goes-se-sandbox01/cnelson2/stateless_nifi_parquet';

select * from default.snifi limit 10;
```


