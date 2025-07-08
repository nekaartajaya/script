Tutorial Cloudflare Zerotrust Tunnel

1. Login into Cloudflare Dashboard and go to Zero Trust

2. In Zero Trust, go to Network -> Tunnel -> Create New Tunnel

3. In the instalation for connector section, select debian and follow the command to isntall cloudflaired with token

4. In our server, install cloudflared with command from Zero Trust

5. After succeed, change ingress nginx controller port type to NodePort or LoadBalancer

6. Then, create ingress for every apps we have in kubernetes cluster, then change hostname with hostname from Zero Trust that you want to create

7. After that, go to Zero Trust Tunnel, add public hostname, and create hostname that you want
8. In Service section, select Type to HTTP, and URL to NodePort or LoadBalancer from ingress nginx controller, example : localhost:30010

9. Finally, open hostname from your browser
