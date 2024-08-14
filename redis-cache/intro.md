
<br>

### Welcome!

In this scenario we'll set up a distributed cache using a [Redis](https://github.com/redis/docker-library-redis/tree/master) pod with [Istio](https://istio.io) config to secure communication.

#### Redis vs Valkey

Redis Inc. changed the open source license during March 2024 to prevent cloud providers wrapping Redis and selling it as a service. This change doesn't restrict usage of Redis as a cache for an application so it is fine to keep using it for this purpose. 

Valkey is a fork of Redis that has been adopted by the Linux Foundation. Contributors include Amazon Web Services (AWS), Google Cloud, Oracle, Ericsson, and Snap Inc. 

More info here [https://www.linuxfoundation.org/press/linux-foundation-launches-open-source-valkey-community](https://www.linuxfoundation.org/press/linux-foundation-launches-open-source-valkey-community)

Redis and Valkey can be used interchangeably at the moment but this could change in the future.

Note: Redis can be swapped for [Valkey](https://github.com/valkey-io/valkey) just by changing the pod's image to `valkey/valkey`.
