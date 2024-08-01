

We will now try with the Stateful Session envoy filter

```bash
kubectl apply -f - <<EOF
apiVersion: networking.istio.io/v1alpha3
kind: EnvoyFilter
metadata:
  name: stateful-session-filter
spec:
  workloadSelector:
    labels:
      istio: sticky-session-test-gateway
  configPatches:
    - applyTo: HTTP_FILTER
      match:
        context: GATEWAY
        listener:
          filterChain:
            filter:
              name: envoy.filters.network.http_connection_manager
              subFilter:
                name: envoy.filters.http.router
      patch:
        operation: INSERT_BEFORE
        value:
          name: envoy.filters.http.stateful_session
          typed_config:
            "@type": type.googleapis.com/envoy.extensions.filters.http.stateful_session.v3.StatefulSession
            session_state:
              name: envoy.http.stateful_session.cookie
              typed_config:
                "@type": type.googleapis.com/envoy.extensions.http.stateful_session.cookie.v3.CookieBasedSessionState
                cookie:
                  name: stateful_session
                  path: /
                  ttl: 86400s
EOF

```{{exec}}

Then run the test script again and you should see that each user will receive a response from the same pod each time.

```bash
./test.sh
```{{exec}}

**Open another Terminal tab** by pressing the `+` button at the top and terminate one of the pods by running

```bash
kubectl delete pod $(kubectl get pods -l app=web -o jsonpath='{.items[0].metadata.name}') --now
```{{exec}}

Switching back to the tab running the test script, you should see that ONLY the users that were sticking to the pod that was terminated are rebalanced to different pods.

Press `CTRL+C` to stop the script