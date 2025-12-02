# Full-Mini-Dataspace-Flow-EDC-

This repository demonstrates a minimal but complete data exchange flow using Eclipse Dataspace Components (EDC).
The goal is to show how two basic connectors—a provider and a consumer—can negotiate, initiate, and complete a data transfer


## Purpose

This repository serves as a compact demonstration of:
- Setting up two EDC connectors
- Running a full contract negotiation cycle
- Executing a complete data transfer using EDC standards
- Pulling data through a secured, token-based EDR endpoint

## How to Reproduce 

1. Clone and build the official EDC Samples.
   
   Prerequisites

   JDK 17+ , Git, curl

 (Optional) jq for formatted JSON output

  If you're using Windows you either need to adapt the paths or use WSL2.
 
 Eclipse EDC Samples repository
git clone https://github.com/eclipse-edc/Samples.git

cd Samples


2. Run provider and consumer connectors using sample configs
   
   Build the provider connector

```bash
./gradlew transfer:transfer-03-consumer-pull:provider-proxy-data-plane:build
```

### Run the connectors

To run the provider, just run the following command

```bash
java -Dedc.fs.config=transfer/transfer-03-consumer-pull/resources/configuration/provider.properties \
    -jar transfer/transfer-03-consumer-pull/provider-proxy-data-plane/build/libs/connector.jar
```
To run the consumer, just run the following command

```
java -Dedc.fs.config=transfer/transfer-00-prerequisites/resources/configuration/consumer-configuration.properties \
  -jar transfer/transfer-00-prerequisites/connector/build/libs/connector.jar
```

3. Use curl or API calls to:
   - Create asset, policy, and contract definition.
     
     
            
     
            curl -d @transfer/transfer-01-negotiation/resources/create-asset.json \
              -H 'content-type: application/json' http://localhost:19193/management/v3/assets \
              -s | jq
            
               
               
               curl -d @transfer/transfer-01-negotiation/resources/create-policy.json \
                 -H 'content-type: application/json' http://localhost:19193/management/v3/policydefinitions \
                 -s | jq
     
               curl -d @transfer/transfer-01-negotiation/resources/create-contract-definition.json \
                 -H 'content-type: application/json' http://localhost:19193/management/v3/contractdefinitions \
                 -s | jq

   - Fetch catalog from provider.
  
     
              
              
                  curl -X POST "http://localhost:29193/management/v3/catalog/request" \
                      -H 'Content-Type: application/json' \
                      -d @transfer/transfer-01-negotiation/resources/fetch-catalog.json -s | jq
              

   
   - Initiate contract negotiation.
   

         
   
            curl -d @transfer/transfer-01-negotiation/resources/negotiate-contract.json \
              -X POST -H 'content-type: application/json' http://localhost:29193/management/v3/contractnegotiations \
                 -s | jq
         
   - Start transfer once agreement is finalized.
     
        Poll negotiation until the response shows:

         "state": "FINALIZED"
     
         Extract: "contractAgreementId"

        Insert the contractAgreementId into:
      
        transfer/transfer-03-consumer-pull/resources/start-transfer.json




         curl -X POST "http://localhost:29193/management/v3/transferprocesses" \
           -H 'Content-Type: application/json' \
           -d @transfer/transfer-03-consumer-pull/resources/start-transfer.json -s | jq
     
   - Retrieve EDR (Endpoint Data Reference)
  
      curl "http://localhost:29193/management/v3/edrs/<TRANSFER_ID>/dataaddress" -s | jq
      
      
      Copy:
      
      endpoint
      
      authorization token


   - Pull data from the EDR endpoint
     
         curl "<EDR_ENDPOINT>" \
           -H "Authorization: <TOKEN>" -s | jq


This returns the final proxied dataset.

![Flow Screenshot](https://github.com/maryamed14/Full-Mini-Dataspace-Flow-EDC-/blob/main/screenshots/Screenshot%202025-12-02%20192436.png)


