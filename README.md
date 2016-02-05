## BBS Benchmark

To run the BBS benchmarks against [BOSH Lite](https://github.com/cloudfoundry/bosh-lite):

Before you run these tests, stop the brain vm:
```
bosh stop brain_z1 0
```

Run ginkgo:

```
ginkgo -nodes=4 -- \
  -desiredLRPs=5000 \
  -numTrials=10 \
  -numReps=5 \
  -numPopulateWorkers=10 \
  -bbsAddress=https://10.244.16.2:8889 \
  -bbsClientHTTPTimeout=10s \
  -etcdCluster=https://10.244.16.2:4001 \
  -etcdCertFile=$GOPATH/manifest-generation/bosh-lite-stubs/etcd-certs/client.crt \
  -etcdKeyFile=$GOPATH/manifest-generation/bosh-lite-stubs/etcd-certs/client.key \
  -bbsClientCert=$GOPATH/manifest-generation/bosh-lite-stubs/bbs-certs/client.crt \
  -bbsClientKey=$GOPATH/manifest-generation/bosh-lite-stubs/bbs-certs/client.key \
  -encryptionKey="key1:a secure passphrase" \
  -activeKeyLabel=key1 \
  -logFilename=test-output.log \
  -logLevel=info
```

If you'd like to have metrics emitted to DataDog, add the following flags:

```
-dataDogAPIKey=$DATADOG_API_KEY \
-dataDogAppKey=$DATADOG_APP_KEY \
-metricPrefix=$METRIC_PREFIX
```

If you'd like to have metrics saved to an S3 bucket, add the following flags:

```
-awsAccessKeyID=$AWS_ACCESS_KEY_ID \
-awsSecretAccessKey=$AWS_SECRET_ACCESS_KEY \
-awsBucketName=$AWS_BUCKET_NAME \
-awsRegion=$AWS_REGION # defaults to us-west-1
```

If you'd like to change the error tolerance allowed, add the following flag:
```
-errorTolerance=0.025
```
