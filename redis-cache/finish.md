
<br>

### WELL DONE !

In this scenario we set up a distributed cache using a [Redis](https://github.com/redis/docker-library-redis/tree/master) pod with [Istio](https://istio.io) config to secure communication.

The communication between the client and cache is encrypted using Istio's mTLS and access to the Redis cache is restricted to only allow the client.