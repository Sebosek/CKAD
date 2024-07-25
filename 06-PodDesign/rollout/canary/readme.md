# Canary release

Using the canary release strategy we want to achive two things:
 1. Route traffice to both versions
 2. Route only a small percentage of traffice to version 2

## Route traffice to both versions
Define a common label `app: front-end` and update service selector to match this new label.
The service now routes to both versions.

## Route only a small percentage of traffice to version 2
Service distributes traffic between services equally. To achive let's say 10% request hitting the version 2, we can reduce number of pods to 1 pod and scale up nmber of pods of v1 to 9.
When we're successfull we can update primary deployment to new version and delete the canary deployment.

To scale number of replicas use `kubectl scale deployment --replicas=1 frontend-v2`