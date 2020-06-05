# CORS one liner command exploiter

A one liner Bash command cheatsheet which finds CORS missconfiguration in every possible method. Simply replace https://example.com with the URL you want to target. This will help you scan for CORS vulnerability without the need of an external tool.

## 1 Basic Origin Reflection payload - Automatically send request to every crawled endpoint of the website

`gau 'https://example.com' | while read url;do target=$(curl -s -I -H "Origin: https://evil.com" -X GET $url) | if grep 'https://evil.com'; then [Potentional CORS Found]echo $target;fi;done`

## 1.2 Basic Origin Reflection payload - Focus in only one endpoint

`curl -s -I -H "Origin: https://evil.com" -X GET 'https://example.com' | if grep 'https://evil.com'; then echo [Potentional CORS Found]; fi`

## 2 Trusted null Origin payload - Automatically send request to every crawled endpoint of the website
`gau 'https://example.com' | while read url;do target=$(curl -s -I -H "Origin: null" -X GET $url) | if grep 'Access-Control-Allow-Origin: null'; then [Potentional CORS Found]echo $target;fi;done`

## 2.2 Trusted null Origin payload - Focus in only one endpoint
`curl -s -I -H "Origin: null" -X GET 'https://example.com' | if grep 'Access-Control-Allow-Origin: null'; then echo [Potentional CORS Found];fi`

## 3 Whitelisted null origin value payload - Automatically send request to every crawled endpoint of the website
`gau 'https://example.com' | while read url;do target=$(curl -s -I -X GET $url) | if grep 'Access-Control-Allow-Origin: null'; then [Potentional CORS Found]echo $target;fi;done`

## 3.2 Whitelisted null origin value payload - Focus in only one endpoint
`curl -I -X GET 'https://example.com' | if grep 'Access-Control-Allow-Origin: null';then echo [Potential CORS Found];fi`

# Workflow
If the one-liner bash command displays output, it means that the website is vulnerable to the respective CORS missconfiguration. If no output is displayed while executed, no vulnerability was detected.

# Requirement

- Download Gua from https://github.com/lc/gau (only for the automatic crawler payloads)
- Download cURL `sudo apt install curl` or `sudo apt-get install curl`
