# node-APM

For Kubernetes see the [advanced docs](https://www.elastic.co/guide/en/apm/agent/nodejs/3.x/express.html#express-advanced-configuration) as the APM serrver details should be passed in via Kubernetes secrets or environment variables.

mkdir node-APM
cd node-APM/
vi package.json
npm install

npm install elastic-apm-node --save

vi server.js

docker build -t danroscigno/node-web-app .
docker push danroscigno/node-web-app:latest

echo -n 'jbFLkVXglRlFWzrxaf' > ELASTIC_APM_SECRET_TOKEN

echo -n 'https://c2198b9a492d42a1b4faab380227701f.apm.us-east4.gcp.elastic-cloud.com:443' > ELASTIC_APM_SERVER_URL

echo -n 'node-example' > ELASTIC_APM_SERVICE_NAME

kubectl create secret generic apm-details \
  --from-file=./ELASTIC_APM_SECRET_TOKEN \
  --from-file=./ELASTIC_APM_SERVER_URL \
  --from-file=./ELASTIC_APM_SERVICE_NAME \
  --namespace=express-demo

kubectl create -f node-express.yaml 

kubectl get pods -n express-demo

kubectl logs express-demo-deployment-699b66866-r6b85 -n express-demo

