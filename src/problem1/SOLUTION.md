Provide your CLI command here:

cat transaction-log.txt | grep TSLA | grep sell | awk -F'"' '{print "https://example.com/api/" $4}' | xargs -I {url} sh -c 'echo "Requesting api : {url
}" && curl -X GET "{curl}"'