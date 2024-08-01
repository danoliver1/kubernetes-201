

This script will simulate 5 distinct users making requests to the web pods, with a 5 second wait between requests.

```bash
cat <<'EOF' > test.sh
#!/bin/bash
set -euo pipefail

USERS_TO_SIMULATE=5

FIRST_RUN=1
while true; do 
# first request needs to save the cookie
if [ $FIRST_RUN -eq 1 ]; then 
    for i in $(seq 1 $USERS_TO_SIMULATE); do 
    echo -n "User #$i - "
    curl -s localhost --cookie-jar "user${i}.cookie"; 
    FIRST_RUN=0
    done
# subsequent requests need to send the cookie
else 
    for i in $(seq 1 $USERS_TO_SIMULATE); do
    echo -n "User #$i - "
    curl -s localhost --cookie "user${i}.cookie"; 
    done
fi;
echo "---"
sleep 5
done
EOF
chmod +x test.sh
```{{exec}}


We have not deployed any config for sticky sessions yet so if we run the script, we should see that each "user" will be served a response from a random pod each time the requests run.

```bash
./test.sh
```{{exec}}

Please press `CTRL+C` to stop the script