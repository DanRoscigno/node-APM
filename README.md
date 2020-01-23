# node-APM

For this example I took the [Dockerizing a Node.js web app](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/) guide and added one line to the sample code:
```
const apm = require('elastic-apm-node').start()
```

Here is the original code:
![Original Code](https://github.com/DanRoscigno/node-APM/raw/master/images/Node-1.png)

and here is the modified code:
![Original Code](https://github.com/DanRoscigno/node-APM/raw/master/images/Node-2.png)

Follow the instructions in Kibana Home -> Add APM, with a Kubernetes twist, as 
instead of adding the APM details to the `.js` file pass in Kubernetes secrets.

![Kibana Home > Add APM](https://github.com/DanRoscigno/node-APM/raw/master/images/APM-1.png)

For Kubernetes see the [advanced docs](https://www.elastic.co/guide/en/apm/agent/nodejs/3.x/express.html#express-advanced-configuration) as the APM serrver details should be passed in via Kubernetes secrets or environment variables.

In this example, the code is Node.jjs, so select Node.jjs, and run the `npm install` command as shown:
![Select Language](https://github.com/DanRoscigno/node-APM/raw/master/images/APM-2.png)

The next block shows the APM details for your Elasticsearch Service in Elastic Cloud.  Since this is a Kubernetes example, use a Kubernetes Secret rather than adding the details to your `.js` file.  There are three details needed:

 - serviceName
 - secretToken
 - serverUrl

Copy each of these out and use them to create a secret named apm-details as shown below:
![APM details](https://github.com/DanRoscigno/node-APM/raw/master/images/APM-3.png)

Create a namespace for the app
```
kubectl create -f namespace.yaml
```

Using the details from the above screenshot, create a Kubernetes secret:
```
echo -n 'jbFLkVXglRlFWzrxaf' > ELASTIC_APM_SECRET_TOKEN

echo -n 'https://c2198b9a492d42a1b4faab380227701f.apm.us-east4.gcp.elastic-cloud.com:443' > ELASTIC_APM_SERVER_URL

echo -n 'node-example' > ELASTIC_APM_SERVICE_NAME

kubectl create secret generic apm-details \
  --from-file=./ELASTIC_APM_SECRET_TOKEN \
  --from-file=./ELASTIC_APM_SERVER_URL \
  --from-file=./ELASTIC_APM_SERVICE_NAME \
  --namespace=express-demo
```

![Original Code](https://github.com/DanRoscigno/node-APM/raw/master/images/APM-4.png)
![Original Code](https://github.com/DanRoscigno/node-APM/raw/master/images/APM-5.png)
mkdir node-APM
cd node-APM/
vi package.json
npm install

npm install elastic-apm-node --save

vi server.js

docker build -t danroscigno/node-web-app .
docker push danroscigno/node-web-app:latest


Deploy the application
```
kubectl create -f node-express.yaml 

kubectl get pods -n express-demo

kubectl logs express-demo-deployment-699b66866-r6b85 -n express-demo
```

Generate traffic
```
curl http://localhost:31080
curl http://localhost:31080/foo
```
