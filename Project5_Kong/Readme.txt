
Lab objective

Students will:

deploy a simple “hello” application
create a Service for it
expose it with Kong
test access through a route like /hello

Lab flow
Phase 1 — Deploy the application

Use a tiny echo app or hello app.

Example deployment:

Hello App: https://github.com/BalericaAI/kubernetesclass/blob/main/Project5_Kong/yaml/helloapp.yaml

Phase 2 — Create the Service

Hello Service: https://github.com/BalericaAI/kubernetesclass/blob/main/Project5_Kong/yaml/helloservice.yaml

Verify

    kubectl get pods
    kubectl get svc

Phase 3 — Install Kong

Install via Helm: 

    helm repo add kong https://charts.konghq.com
    helm repo update
    
    kubectl create namespace kong
    
    helm install kong kong/ingress -n kong

Then verify:
    kubectl get pods -n kong
    kubectl get svc -n kong

Questions:
  1) What is the controller pod?
  2) What is the proxy Service?
  3) Waht is the namespace separation?

Phase 4 — Expose the app through Kong with Ingress

Example Ingress:

Kong Ingress: https://github.com/BalericaAI/kubernetesclass/blob/main/Project5_Kong/yaml/kong_helloingress.yaml

Expected result:

        Hello from Kong Lab 1

KIC documentation explains that it processes Kubernetes resources marked for its ingress class and translates them into Kong configuration.

