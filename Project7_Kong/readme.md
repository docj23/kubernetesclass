Kong Lab 3 — Rate Limiting
Lab Title

Control API Abuse with Kong Rate Limiting

Big Idea

A) In Kong Lab 2:--> Kong decided who may access the API.
B) In Kong Lab 3:--> Kong decides how much access is permitted.

This is the platform lesson:--> A valid user can still be a problem.”

Learning Objectives

By the end of this lab, you, aka Lizzo Devote, should be able to:

    explain what rate limiting is and why it matters
    apply Kong’s rate-limiting plugin using a KongPlugin
    enforce a per-minute request cap
    generate request floods with curl loops or k6a
    observe 200 OK changing into 429 Too Many Requests
    understand the difference between unauthenticated abuse and authenticated overuse

Kong’s KIC docs specifically show rate limiting by creating a KongPlugin with plugin: rate-limiting
and setting config.minute to the allowed number of requests per minute, then associating it with a
Kubernetes resource using konghq.com/plugins


Scenerio

        “You locked the API with authentication. Good.
        Then Chewbacca sends 10,000 Lizzo pic requests.
        Your app is still dead.
        Fix it without touching application code.”
        
        That is a real platform-engineering problem.


Starting Point

You should already have from prior labs:

        hello-app
        hello-service
        hello-ingress
        optional Lab 2 key auth working

They should be able to hit

curl http://<KONG>/hello
or
curl http://<KONG>/hello -H "apikey: super-secret-key"


Phase 1 — Rate Limiting Purpose

Let's think..... Why rate limiting exists
        protect services from accidental floods
        reduce damage from abusive clients
        preserve application availability
        create predictable API behavior

Kong describes rate limiting as controlling how many requests a client can make within a specified period, and frames it as a core availability and traffic-control function


Phase 2 — Create the Rate Limiting Plugin

For the first version, keep it simple: rate-limit-plugin.yaml: https://github.com/BalericaAI/kubernetesclass/blob/main/Project7_Kong/yaml/rate-limit-plugin.yaml

        kubectl apply -f rate-limit-plugin.yaml

What this means
        allow 5 requests per minute
        after that, Kong starts rejecting requests
        policy: local is fine for a basic single-node/small-class lab

Phase 3 — Attach the Plugin

You can attach plugins to an Ingress, Service, HTTPRoute, KongConsumer, or other supported resources using konghq.com/plugins. For this lab, attaching it to the Ingress keeps the mental model simple.

Apply Ingress_Rate_Limit_Ingress.yaml
https://github.com/BalericaAI/kubernetesclass/blob/main/Project7_Kong/yaml/ingress_rate_limit_plugin.yaml

        kubectl apply -f  Ingress_Rate_Limit_Ingress.yaml

Phase 4 — Test Slowly First

First, prove the API still works under normal use.

        curl http://<KONG>/hello
        curl http://<KONG>/hello
        curl http://<KONG>/hello

If Kong Lab 2 key-auth is still enabled:

        curl http://<KONG>/hello -H "apikey: super-secret-key"
        curl http://<KONG>/hello -H "apikey: super-secret-key"
        curl http://<KONG>/hello -H "apikey: super-secret-key"

Phase 5 — Trigger the Limit

Now make them exceed the cap.

Simple bash loop

Without auth: https://github.com/BalericaAI/kubernetesclass/blob/main/Project7_Kong/bash/simpleloop.sh
With auth: https://github.com/BalericaAI/kubernetesclass/blob/main/Project7_Kong/bash/keyloop.sh

Apply

        ./simpleloop.sh
        ./keyloop.sh

What happens?

Phase 6 — Use k6 for a Realer Flood

k6 is an open-source load-testing tool designed for running scripted HTTP load tests, and its docs walk through writing a simple test script and running it locally.

rate-test.js            https://github.com/BalericaAI/kubernetesclass/blob/main/Project7_Kong/javascript/rate-test.js
key_rate-test.js        https://github.com/BalericaAI/kubernetesclass/blob/main/Project7_Kong/javascript/key_rate-test.js

Run it!

k6 run rate-test.js
k6 run key_rate-test.js

What happens?
