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
2. Run provider and consumer connectors using sample configs.
3. Use curl or API calls to:
   - Create asset, policy, and contract definition.
   - Fetch catalog from provider.
   - Initiate contract negotiation.
   - Start transfer once agreement is finalized.
   - Retrieve EDR.
   - Pull data from EDR endpoint.
