Kong Lab 3: Bonus — “Two Kingdoms, One Gate”

“Two applications.
Two users.
One gateway.

Your job:----> Prevent the wrong user from accessing the wrong system.

Kong handles identity.
OPA decides authorization.”


Theme

    Falcon and Lindworm are two internal platforms.
    Not all users are equal.
    Not all access is allowed.
    And Kong is no longer enough…
    OPA now judges intent.


Big Idea

Students must combine:

    Kong routing
    Kong authentication (API keys)
    Kong rate limiting (optional carryover)
    OPA policy enforcement

This is your first real platform system.


Mission Objective

Students must enforce:

| User         | Allowed Access |
| ------------ | -------------- |
| chewbacca    | falcon ONLY    |
| darth malgus | lindworm ONLY  |


If violated:--->  Request must be denied.  No Lizzo banana for YOU!

Stage 0: Namespaces

Deploy namespaces.yaml https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/namespaces.yaml

Deploy Apps into Correct Namespaces

Falcon Deployment  https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/falcon_deployment.yaml
Lindworm Deployment https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/Lindworm_Deployment.yaml



Stage 1: Service Deployment

falcon: https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/falcon.yaml
lindworm: https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/lindworm.yaml

Services: https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/services.yaml



Stage 2 Kong Routing Requirement

Routes must exist:

/falcon
/lindworm

Make Kong USERS!!!

Stage 3 Consumers
        chewbacca
        darth-malgus
API Keys

Example expectation:

| User         | API Key      |
| ------------ | ------------ |
| chewbacca    | wookie-power |
| darth malgus | sith-control |

https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/chewbaccakey.yaml
https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/malguskey.yaml


REMEMBER: Kong vs OPA!!!!

Kong---> authentication
OPA---> authorization decisions

Phase 4 — OPA Gatekeeper Constraints

You must implement:

Rule:

allow {
  input.user == "chewbacca"
  input.path == "/falcon"
}

allow {
  input.user == "darth-malgus"
  input.path == "/lindworm"
}

Now the real enforcement layer.

We enforce:

falcon namespace → only port 12000 allowed
lindworm namespace → only port 3653 allowed


OPA Constraint Template: https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/OPA/opa_constraint_ports.yaml
OPA Constraint Ports: https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/OPA/opa_constraint_ports.yaml

Phase 5 — Kong Layer (Still Required)

Students must:

        expose /falcon → falcon namespace service
        expose /lindworm → lindworm namespace service

Then:

        apply API key auth
        map:
        chewbacca → falcon
        darth malgus → lindworm

Test it!!!

curl http://<KONG>/falcon -H "apikey: wookie-power"
curl http://<KONG>/lindworm -H "apikey: sith-control"

Try this!
curl http://<KONG>/lindworm -H "apikey: wookie-power"


