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


OPA Constraint Template: https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/OPA/opa_constraint_ports.yaml
OPA Constraint Ports: 



