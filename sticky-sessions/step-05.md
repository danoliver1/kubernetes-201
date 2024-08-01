

To add the config for consistent hash based stickiness run

```bash
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1
kind: DestinationRule
metadata:
  name: web
spec:
  host: web
  trafficPolicy:
    loadBalancer:
      consistentHash:
        httpCookie:
          name: sticky-session
          ttl: 0s
EOF
```{{exec}}

Then run the test script again and you should see that each user will receive a response from the same pod each time.

```bash
./test.sh
```{{exec}}

Open another Terminal tab by pressing the `+` button at the top and terminate one of the pods by running

```bash
kubectl delete pod $(kubectl get pods -l app=web -o jsonpath='{.items[0].metadata.name}') --now
```{{exec}}

You should see that ALL of the users requests are rebalanced to different pods, not just the users that are sticking to the pod that was terminated.

Clean up the sticky session config by running:

```bash
kubectl delete destinationrule web
```{{exec}}