# Introduction
this repo is an example use case of stubby4j
https://github.com/azagniotov/stubby4j#quick-start-example


Features  | Ports
------------- | -------------
Stubs api  | http://localhost:8882
TLS stubs api  | https://localhost:7443
Dashboard   |   http://localhost:8889/status 

# Pre-requist
- docker-compose
- docker

# How to use
```shell
docker-compose up -d
```
>After changes, No need to re-deploy because there is WITH_WATCH help observe change in yaml/config file

# Get Started

Define 3rd party lists in stubs.yaml

```yaml
includes:
   - example.yaml 
   - ./3rd-party-name/3rd-party-name.yaml 
```
Define each api in example.yaml
- url: regex support
- method: GET/POST/PUT/PATCH/DELETE
- latency: the time in milliseconds the server should wait before responding.
- ... more on https://github.com/azagniotov/stubby4j#quick-start-example
```yaml
-  description: Stub two
   request:
      url: ^/two$
      method: 
         - GET
         - POST

   response:
      status: 200
      latency: 5000
      body: 'TWOTwo!'
```


# Clean Architecture
we should define our 3rd party service as <b>3rd-party-name</b> folder 
then define each api in sub folder for example accout, balance

```
├── ...
├── 3rd-party-name-1            # 3rd party name 
│   ├── account                 # account service 
│   ├── balance                 # balance service 
│   └── 3rd-party-name.yaml     # for define api in 3rd party 
├── 3rd-party-name-2
├── logs                        # logs files
│   ├── stubby4j.log
├── docker-compose.yaml         
├── stubs.yaml                  # include all yaml from all service
```

# Example

## 3rd-party-name/account
> If we define file in request, we need to send request exactly **the same** as below accountRequest.json body

defining api in 3rd-party-name.yaml
```yaml
-  description: 3rd-party account service
   request:
      url: /3rd/get/account
      method: POST
      headers:
         content-type: application/json
      file: ./3rd-party-name/account/accountRequest.json

   response:
      status: 200
      headers:
         content-type: application/json
      file: ./3rd-party-name/account/accountResponse.json
```

3rd-party-name/account/accountRequest.json
```json
{
    "cid": "1234567890123"
}
```

3rd-party-name/account/accountResponse.json
```json
{
    "firstname": "firstname",
    "lastname": "lastname",
    "status": "success",
    "code": 0
}
```

## 3rd-party-name/Balance
> For this case, we can send any requests in json format
```yaml
-  description: 3rd-party balance service
   request:
      url: /3rd/get/balance
      method: POST
      headers:
         content-type: application/json

   response:
      status: 200
      headers:
         content-type: application/json
      file: ./3rd-party-name/balance/balanceResponse.json
```
3rd-party-name/balance/balanceResponse.json
```json
{
    "total": 1000000,
    "name": "ping"
}
```
