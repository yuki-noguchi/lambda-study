```
cd localstack
docker compose up -d

cd ../lambda
aws --endpoint-url=http://localhost:4566 \
  lambda create-function --function-name my-lambda \
  --zip-file fileb://function.zip \
  --handler index.handler --runtime nodejs12.x \
  --role arn:aws:iam::000000000000:role/lambda-role 

aws --endpoint-url=http://localhost:4566 s3 mb s3://my-bucket

cd ..
aws --endpoint-url=http://localhost:4566 \
  s3api put-bucket-notification-configuration --bucket my-bucket \
  --notification-configuration file://s3-notif-config.json
aws --endpoint-url=http://localhost:4566 \
  s3api put-object --bucket my-bucket \
  --key dummyfile.txt --body=dummyfile.txt

aws --endpoint-url=http://localhost:4566 logs tail '/aws/lambda/my-lambda' --follow
```