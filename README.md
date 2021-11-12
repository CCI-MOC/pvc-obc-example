Deploy this repository to create:

- An `ObjectBucketClaim`
- A `PersistentVolumeClaim`
- An `awscli` `Pod` which (a) mounts the `PersistentVolumeClaim` on `/data` and
  (b) exposes the access information from the `Secret` and `ConfigMap`
  created by the `ObjectBucketClaim` as environment variables.

Deploy this repository like:

```
oc apply -k .
```

To interact with the object bucket:

1. List available buckets:

    ```
    aws --endpoint-url https://$BUCKET_HOST \
      --ca-bundle /run/secrets/kubernetes.io/serviceaccount/service-ca.crt \
      s3api list-buckets
    ```

2. Upload a file to the bucket:

    ```
    aws --endpoint-url https://$BUCKET_HOST \
      --ca-bundle /run/secrets/kubernetes.io/serviceaccount/service-ca.crt \
      s3api put-object --bucket $BUCKET_NAME --key test-object --body /etc/bashrc
    ```

3. Cat a file from the bucket:

    ```
    aws --endpoint-url https://$BUCKET_HOST \
      --ca-bundle /run/secrets/kubernetes.io/serviceaccount/service-ca.crt \
      s3api get-object --bucket $BUCKET_NAME --key test-object /dev/stdout
    ```
