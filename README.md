# URL Scanner

This is a small shell script that looks through every URL (well, every URL using HTTP or HTTPS) in a directory and its subdirectories, and returns every instance of a 4XX or 5XX response.

It looks like this:

```bash
#!/bin/zsh
for URL in $(grep -rEhoI "(http|https)://[a-zA-Z0-9./?=_-]*" | sort -u) # get all URLs and remove duplicates
do
    RESPONSE=$(curl -sI -o /dev/null -w "%{http_code}" $URL)    # get response code
    if [[ "$RESPONSE" =~ ^(4|5) ]]                              # only look at 4xx and 5xx responses
    then
        echo "$RESPONSE | $URL"
        grep -rIn "$URL" | awk -F: '{print "    " $1 ": " $2}'  # print file name and line number
        echo ""
    fi
done

```

It starts by `grep`ing recursively for every URL, and removes dupicates by using `sort -u`.

It then loops through these URLs, `curl`ing each one. If there is a 4XX or 5XX response, the response code and URL is printed, along with every instance of that URL's occurence.
