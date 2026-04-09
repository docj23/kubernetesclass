Kong Lab 2 — Security Plugins (API Protection)
Lab Title

Protect Kubernetes Services with Kong Authentication

Big Idea

In Kong Lab 1:---> Kong decided where traffic goes

In Kong Lab 2:---> Kong decides who is allowed to send traffic

Goal:  “Oh… I can block access to services without touching application code.”

That's O'Block OG gangster power 

Learning Objectives

By the end of this lab, you will:

    enforce authentication at the gateway level
    understand consumer identity in Kong
    configure an API key authentication plugin
    observe 401 vs 200 behavior
    troubleshoot broken auth configurations


Starting Point (From Kong Lab 1)

You should already have:

        hello-app (Deployment)
        hello-service
        hello-ingress
        working endpoint:

Verify: curl http://<KONG>/hello

Response: Hello from Kong Lab 1

Phase 1 — Prove It's Open (Baseline)

Notice.... curl http://<KONG>/hello .... anyone can do this.... maybe you have Lizzo pics there.....

Phase 2 — Enable API Key Authentication

        Kong uses plugins for security.
        
        We will use: --> key-auth plugin
        This forces requests to include an API key.


Apply API Plugin to Ingress: https://github.com/BalericaAI/kubernetesclass/blob/main/Project6_Kong/yaml/kong_plugin_api.yaml

    kubectl apply -f kong_plugin_api.yaml

Attach this yaml:  https://github.com/BalericaAI/kubernetesclass/blob/main/Project5_Kong/yaml/kong_helloingress.yaml

    kubectl apply -f apply_key_ingress.yaml

Phase 3 — Test (Should Fail)

Verify: curl http://<KONG>/hello

What happened?

Phase 4 — Give Chewbacca Access
Apply Chewbacca: https://github.com/BalericaAI/kubernetesclass/blob/main/Project6_Kong/yaml/chewbacca.yaml

kubectl apply -f chewbacca.yaml


Phase 5 — Create API Key Credential
Apply API Key Cred: https://github.com/BalericaAI/kubernetesclass/blob/main/Project6_Kong/yaml/chewbacca_key.yaml

    kubectl apply -f chewbacca_key.yaml

Phase 6 — Test With API Key

curl http://<KONG>/hello -H "apikey: super-secret-key"
What happens?

Let's verify 

        kubectl get kongplugin
        kubectl get kongconsumer
        kubectl get kongcredential
        kubectl describe ingress hello-ingress



