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

Stage 1: Service Deployment

falcon: https://github.com/BalericaAI/kubernetesclass/blob/main/Project8_OPA_Kong/yaml/falcon.yaml
lindworm: 
