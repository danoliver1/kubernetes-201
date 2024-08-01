

To add the config for consistent hash based stickiness run

```bash
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: web
spec:
  host: web.default.svc.cluster.local
  trafficPolicy:
    loadBalancer:
      consistentHash:
        httpCookie:
          name: sticky-session
          ttl: 0s
EOF
```{{exec}}

Then run the test script again

```bash
./test
```{{exec}}

Open another Terminal tab by pressing the `+` button at the top.

Terminate one of the pod by running

```bash
kubectl delete pod $(kubectl get pods -l app=web -o jsonpath='{.items[0].metadata.name}') --now
```{{exec}}

You should see that ALL of the users requests are rebalanced to different pods, not just the users that are sticking to the pod that was terminated.

Clean up the stickiness config by running:

```bash
kubectl delete destinationrule web
```{{exec}}