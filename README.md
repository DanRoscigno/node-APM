# node-APM
mkdir node-APM
cd node-APM/
vi package.json
npm install

npm install elastic-apm-node --save

vi server.js

docker build -t danroscigno/node-web-app .
docker push danroscigno/node-web-app:latest

vi node-express.yaml 
kubectl create -f node-express.yaml 
kubectl get services -n express-demo
