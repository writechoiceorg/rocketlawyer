# Quick Start Guide: Upgrading to RocketDocument v2

This guide will help you quickly upgrade from RocketDocument v1 to v2, ensuring your integration remains compatible with the latest version. Follow these steps to update your setup.

## Step 1: Update API Endpoints
Update all your RocketDocument API endpoints from v1 to v2. Here are some key examples:

- **Create an interview:**
  - **Old:** `POST /rocketdoc/v1/interviews`
  - **New:** `POST /rocketdoc/v2/interviews`
  
- **Get a document:**
  - **Old:** `GET /rocketdoc/v1/documents/:documentId`
  - **New:** `GET /rocketdoc/v2/documents/:documentId`
  
- **Get interviews:**
  - **Old:** `GET /rocketdoc/v1/interviews`
  - **New:** `GET /rocketdoc/v2/interviews`

> **More information on v2 endpoints**
> v2 endpoints provide more detailed information compared to v1. Explore these new endpoints to enhance the user experience.

## Step 2: Update Embedded UX URL
Change the RocketDocument Embedded UX URL from v1 to v2:

- **For Sandbox:**
  - **Old:** `https://rocket-document.sandbox.rocketlawyer.com/rocket-document.js`
  - **New:** `https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js`
  
- **For Production:**
  - **Old:** `https://rocket-document.rocketlawyer.com/rocket-document.js`
  - **New:** `https://rocket-document.rocketlawyer.com/v2/rocket-document.esm.js`

## Step 3: Adapt the `<script>` Tag
When embedding the RocketDocument v2 Embedded UX, update the `<script>` tag as follows:

```html
<script 
  type="module"
  src="https://rocket-document.sandbox.rocketlawyer.com/v2/rocket-document.esm.js">
</script>
```

## Step 4: Update Monitored Embedded UX Events
Adjust the monitored events in the Embedded UX. Initially, RocketDocument v2 will fire both legacy v1 and new v2 events. However, starting January 2nd, 2025, only the new v2 events will be active. Ensure that your system is ready to handle these changes.

By following these steps, you'll successfully upgrade to RocketDocument v2, taking advantage of the latest features and improvements. If you still have any doubts, contact our Support Team [add email].