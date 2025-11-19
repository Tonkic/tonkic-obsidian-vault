```bash
#!/bin/bash

# --- 用户配置 ---
DOMAINS="YOUR_DOMAIN_HERE"

TOKEN="YOUR_TOKEN_HERE"

INTERFACE="ppp0"



SCRIPT_DIR=$(dirname "$(readlink -f "$0")")
LOG_FILE="${SCRIPT_DIR}/duck.log"

IP_ADDRESS=$(ip addr show ${INTERFACE} | grep 'inet ' | awk '{print $2}' | cut -d'/' -f1)


if [ -z "${IP_ADDRESS}" ]; then
  echo "$(date): ERROR - Could not find IP address on interface ${INTERFACE}." > "${LOG_FILE}"
  exit 1
fi

URL="https://www.duckdns.org/update?domains=${DOMAINS}&token=${TOKEN}&ip=${IP_ADDRESS}"

echo "Attempting to update DuckDNS for domain(s) ${DOMAINS} with IP ${IP_ADDRESS} from interface ${INTERFACE}..."


curl --noproxy '*' -sk -o "${LOG_FILE}" "${URL}"


echo "" >> "${LOG_FILE}"


RESPONSE=$(cat "${LOG_FILE}" | tr -d '\n') 
if [ "${RESPONSE}" = "OK" ]; then
  echo "Update successful! IP set to ${IP_ADDRESS}. Response: OK"
else
  echo "Update failed! Check log for details. Response: ${RESPONSE}"
fi

```

