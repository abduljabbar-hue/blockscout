Setup Blockscout on EC2 (Browser Access)
1. Connect to EC2
ssh -i "key.pem" ubuntu@<EC2-IP>

2. Install Docker
sudo apt update
sudo apt install docker.io docker-compose -y

3. Clone Blockscout
git clone https://github.com/blockscout/blockscout.git
cd blockscout/docker-compose

4. Edit ENV files

Go to docker-compose/envs/

frontend.env
NEXT_PUBLIC_API_HOST=<EC2-IP>
NEXT_PUBLIC_API_PROTOCOL=http
NEXT_PUBLIC_WS_PROTOCOL=ws
CORS_ALLOWED_ORIGINS=*

backend.env
ETHEREUM_JSONRPC_HTTP_URL=http://<PRIVATE-IP>:8545
ETHEREUM_JSONRPC_WS_URL=ws://<PRIVATE-IP>:8546

5. Edit Proxy Config

Go to docker-compose/proxy/
Update nginx.conf or default.conf → replace any domain/localhost with your private IP
Example:

proxy_pass http://172.31.22.1:4000;

6. Run Blockscout
make start


or

docker-compose up -d

7. Allow EC2 Access

In AWS Security Group:

Add inbound rule → HTTP (port 80) from 0.0.0.0/0

8. Access in Browser
http://<EC2-Public-IP>

9. Fix Common Issues

Bad Gateway: Wrong IP in env/proxy

CORS Error: Add CORS_ALLOWED_ORIGINS=*

WS Error: RPC not reachable

Check logs:

docker-compose logs -f
