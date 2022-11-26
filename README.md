# URL Scanner

This is a small shell script that looks through every URL (well, every URL using HTTP or HTTPS) in a directory and its subdirectories, and returns every instance of the provided HTTP code. Codes are provided as an argument to the `-r` option.

It `grep`s recursively for every URL, and removes dupicates by using `sort -u`. It then loops through these URLs, `curl`ing each one. If there is an instance of the provided response, the response code and URL is printed, along with every instance of that URL's occurence. If no response code is provided, it defaults to looking for a 4XX or 5XX response.

## Examples

- Look for 4XX and 5XX responses: `% surl`
- Look for 404 responses: `% surl -r 404`
- Look for 2XX responses: `% surl -r 2`
- Look for 3XX and 404 responses: `% surl -r '3|404'`
