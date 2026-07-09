# Image Generation Lab - Completion

## Overview
This document records the successful completion of the **image-client** lab. 

## Compromises and Adaptations
During this lab, we ran into an interesting challenge regarding regional model availability:
- **Missing Lab Model:** The standard model recommended by the lab instructions was not available in our current Azure region.
- **Switch to FLUX.2-pro:** To compromise, we pivoted to use a partner model from Black Forest Labs, **FLUX.2-pro**, deployed via Azure AI Services (Foundry).
- **Client Refactoring:** Because the FLUX model is exposed via a specific REST API endpoint (`/providers/blackforestlabs/v1/flux-2-pro`), it could not be natively invoked using the standard `client.images.generate()` method from the `openai` Python SDK. 
- **Custom REST Implementation:** We updated `.env` to point to the new endpoint and updated `image-client.py` to use Python's `requests` library. We successfully maintained our secure, passwordless authentication by configuring the `DefaultAzureCredential` token provider with the `https://ai.azure.com/.default` scope and passing the token via the `Authorization: Bearer` header.

## Generation Output
Using the adapted REST client, we requested the FLUX model to generate:
> *"A photograph of a red fox in an autumn forest"*

The model successfully generated the image and we successfully decoded the `b64_json` payload, saving it to our local file system!

![Generated FLUX image - Red Fox](images/image_1.png)

**Status:** Completed successfully! 🎉
