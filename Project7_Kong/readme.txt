Kong Lab 3 — Rate Limiting
Lab Title

Control API Abuse with Kong Rate Limiting

Big Idea

In Kong Lab 2:--> Kong decided who may access the API.

In Kong Lab 3:--> Kong decides how much access they get.

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

For the first version, keep it simple:

rate-limit-plugin.yaml: 


