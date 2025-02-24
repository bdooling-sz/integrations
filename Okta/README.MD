
# SecZetta / Okta Integration

## Contents

- [SecZetta / Okta Integration](#seczetta---okta-integration)
  * [Contents](#contents)
  * [Overview](#overview)
    + [Architecture Overview](#architecture-overview)
  * [Supported Features](#supported-features)
  * [Prerequisites](#prerequisites)
    + [Okta  API](#okta--api)
      - [Authentication](#authentication)
    + [Examples](#examples)
    + [Generating a Okta API Key](#generating-a-okta-api-key)
      - [Okta Admin Dashboard](#okta-admin-dashboard)
    + [Generating the SecZetta Rest API Workflow](#generating-the-seczetta-rest-api-workflow)
        * [Example JSON Response](#example-json-response)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

## Overview

Okta connects any person with any application on any device.

It's an enterprise-grade, identity management service, built for the cloud, but compatible with many on-premises applications. With Okta, IT can manage any employee's access to any application or device. Okta runs in the cloud, on a secure, reliable, extensively audited platform, which integrates deeply with on-premises applications, directories, and identity management systems.

The Okta solution was born of the unique challenges of how technology has grown and shifted in the growing diversity of devices, identity issues, security, employee mobility, vender partnership, and the exponential growth of unique application options.

Okta features include Provisioning, Single Sign-On (SSO), Active Directory (AD) and LDAP integration, the centralized deprovisioning of users, multifactor authentication (MFA), mobile identity management, and flexible policies for organization security and control.

All of these functions are brought together through a network of pre-integrated applications called the Okta Integration Network (OIN). The OIN provides diverse integration options, enabling SSO login for every app your users need to access during their work day.

### Architecture Overview

#### EXAMPLE: 

![Architecture](img/okta-sz-architecture.png "This is a sample image.")

- Step 1: Push to Okta to create account
- Step 2: Okta responds back with a 200 RESPONSE
- Step 3: User is granted access to Okta account containing SAAS Apps
- Step 4: Okta returns Okta ID and Provisioning Status to SecZetta Profile

## Supported Features

- Push SecZetta Profile to Okta for Account Creation and Application / Resource Access
- Push SecZetta Risk Rating to Okta for Authorization / Authentication Triggers

## Prerequisites

All Okta integrations will need these 3 prereq.

1. An active SecZetta account and tenant where you have administrative privileges. To set up a new SecZetta account, please reach out to [SecZetta Support (info@seczetta.com)](mailto:info@seczetta.com)

2. Super Admin privledges in the Okta Admin Dashboard

3. An Active Okta API token

### Okta  API

#### Authentication

Okta API tokens are used to authenticate requests to Okta APIs. When calling an Okta API endpoint, you need to supply a valid API token in the HTTP Authorization header, with a valid token specified as the header value. You need to prefix the value with the SSWS identifier, which specifies the proprietary authentication scheme that Okta uses.

### Examples

`Authorization: SSWS 00QCjAl4MlV-WPXM...0HmjFx-vbGua`

### Generating a Okta API Key

In order to generate an Okta API Key follow these steps: 

1. Navigate to the Admin Console side of Okta. 

2. Click into **Security**

3. Click the **API** sub-category

4. Click the **Token** heading

5. Click **Create Token** 

6. Name the Token

7. Copy the Token

#### Okta Admin Dashboard

![Architecture](img/okta-api-token.png "This is a sample image.")

With the token created, you can begin using it, supplying it in the `Authorization Header` of calls to Okta API endpoints. See Create a user by API for an easy way to explore making calls to Okta using the Okta APIs.

### Generating the SecZetta Rest API Workflow

In order to generate the SecZetta API Workflow follow these steps:

1. Navigate to the SecZetta Admin Dashboard

2. Under Lifecycle Click **Workflow**

3. Click **Create Workflow**

4. Click **Action**

5. Click the **Rest API** button

6. Update the Request **Http verb** to **POST**

7. Update the endpoint to **https:// YOUR OKTA DOMAIN.com/api/v1/users?activate=true**

8. Add the below **Headers**
    
   | HEADER | VALUE |
   |--------|-------|
   | Content-Type | application/json |
   | Authorization | SSWS (YOUR OKTA API TOKEN) |
   | Accept | application/json |
    
9. Add the JSON body - Example Below:
   
   ![JSON](img/okta-seczetta-workflow.png "This is a sample image.")
   
10. Add the Following Response Data Mappings

  | PATH | ATTRIBUTE |
  |------|-----------|
  | Status | api_status_code (api_status_code) |
  | id | api_response_1 (api_response_1) |
  | Status | api_response_1 (api_response_2) |
    
11. Click **Save**

The key data points that are synced to SecZetta are `Okta User ID` and `Okta Provisioning Status`.

![Architecture](img/okta-integration-details.png "This is a sample image.") 


##### Example JSON Response

```json
{
    "id": "00u12w2w3ycGIJ1MK0h8",
    "status": "PROVISIONED",
    "created": "2021-11-11T18:25:15.000Z",
    "activated": "2021-11-11T18:25:16.000Z",
    "statusChanged": "2021-11-11T18:25:16.000Z",
    "lastLogin": null,
    "lastUpdated": "2021-11-11T18:25:16.000Z",
    "passwordChanged": null,
    "type": {
        "id": "otylz09d4l2xU70ub0h7"
    },
    "profile": {
        "firstName": "Bruce",
        "lastName": "Wayne",
        "mobilePhone": null,
        "secondEmail": null,
        "project_name": "Identity Assessment - Sept 2020",
        "login": "bruce.wayneg@acme.com",
        "email": "bruce.wayne@acme.com"
    },
    "credentials": {
        "provider": {
            "type": "OKTA",
            "name": "OKTA"
        }
    }
}
```

> The API will respond back with a `200 OK` if the request was successful
