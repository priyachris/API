# The API Documentation Guide: The Contract Approach

> A comprehensive guide for technical writers to document APIs using the contract metaphor.

## Table of Contents

- [Introduction: APIs as Contracts](#introduction-apis-as-contracts)
- [Foundational Contract Elements](#foundational-contract-elements)
- [Documenting the API Contract](#documenting-the-api-contract)
- [Visual Documentation Techniques](#visual-documentation-techniques)
- [Contract Violation Documentation](#contract-violation-documentation)
- [Versioning and Evolution Documentation](#versioning-and-evolution-documentation)
- [Teaching API Concepts to Writers](#teaching-api-concepts-to-writers)
- [Practical Documentation Exercises](#practical-documentation-exercises)
- [Advanced Contract Documentation](#advanced-contract-documentation)
- [Resources and Tools](#resources-and-tools)

## Introduction: APIs as Contracts

### The Contract Metaphor

An API (Application Programming Interface) establishes a formal agreement between software systems—a contract that explicitly defines expected behaviors, inputs, outputs, and constraints. Understanding this metaphor transforms how you approach API documentation.

**Key Contract Principles in APIs:**
- **Explicit Terms**: Clearly defined behaviors and expectations
- **Mutual Obligations**: Responsibilities for both providers and consumers
- **Enforcement Mechanisms**: Validation and error handling
- **Amendment Procedures**: Versioning and evolution strategies

### Example: The Restaurant Contract

Consider a restaurant as an API:
**Real API Contract Example:**
**Payment API Contract Example**
```
{
  "endpoint": "/v1/payments",
  "method": "POST",
  "provider_obligations": {
    "availability": "99.9% uptime",
    "response_time": "< 300ms for 95% of requests",
    "data_security": "PCI DSS compliant"
  },
  "consumer_obligations": {
    "authentication": "Valid API key required",
    "request_format": "JSON with required fields",
    "rate_limits": "Max 100 requests per minute"
  }
}
```
# Payment resource

The Payment resource represents a financial transaction in the system.

## Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| /v1/payments | GET | Retrieve a list of payments |
| /v1/payments | POST | Create a new payment |
| /v1/payments/{id} | GET | Retrieve a specific payment |
| /v1/payments/{id} | PUT | Update a payment |
| /v1/payments/{id} | DELETE | Cancel a payment |

These endpoints form the core of the payment contract, allowing you to create, retrieve, update, and delete payment resources according to specified terms.
## Create Payment Request

### Request Format

```http
POST /v1/payments HTTP/1.1
Host: api.example.com
Authorization: Bearer {your_api_key}
Content-Type: application/json
{
  "amount": 100.00,
  "currency": "USD",
  "payment_method_id": "pm_123456789",
  "description": "Payment for order #1234"
}
```
| Parameter | Type | Description | Constraints |
|-----------|------|-------------|------------|
| amount | number | The payment amount | > 0 |
| currency | string | Three-letter currency code | ISO 4217 format |
| payment_method_id | string | The payment method to use | Must be a valid ID |

| Parameter | Type | Description | Default |
|-----------|------|-------------|---------|
| description | string | Payment description | null |
| metadata | object | Custom metadata | {} |

### 3. Response Format

**Documentation Example:**

```markdown
## Create Payment Response

### Success Response (200 OK)

```json
{
  "id": "pmt_987654321",
  "amount": 100.00,
  "currency": "USD",
  "status": "succeeded",
  "created_at": "2023-07-15T14:22:31Z",
  "payment_method_id": "pm_123456789",
  "description": "Payment for order #1234"
}
```
| Field | Type | Description |
|-------|------|-------------|
| id | string | Unique identifier for the payment |
| amount | number | The payment amount |
| currency | string | Three-letter currency code |
| status | string | Payment status ("succeeded", "pending", "failed") |
| created_at | string | ISO 8601 datetime of creation |
| payment_method_id | string | The payment method used |
| description | string | Payment description |

### 4. Authentication Requirements

**Documentation Example:**

# Authentication

## API Key Authentication

All requests to the Payments API must include an API key in the Authorization header:

```
Authorization: Bearer your_api_key

API Key Acquisition
Navigate to the Developer Dashboard
Go to "API Keys" section
Generate a new key for your environment (test/production)
Security Requirements
API keys must be kept secure and never exposed in client-side code
Each application should use its own API key
Rotate keys every 90 days (recommended)
```
| Status Code | Error Code | Description |
|-------------|------------|-------------|
| 401 | unauthorized | No API key provided |
| 401 | invalid_key | API key is not valid |
| 403 | insufficient_permissions | API key lacks required permissions |
## Documenting the API Contract

### Contract Structure Template

**Example Template:**


# API Contract: [Resource Name]

## Contract Overview
- **Version**: [e.g., 1.0]
- **Release Date**: [Date]
- **Stability**: [Stable/Beta/Experimental]

## Provider Obligations
- **Availability**: [e.g., 99.9% uptime]
- **Performance**: [e.g., 95% of requests processed under 300ms]
- **Data Retention**: [e.g., Transaction data retained for 90 days]

## Consumer Obligations
- **Authentication**: [Required auth method]
- **Request Format**: [Expected format]
- **Rate Limits**: [e.g., 100 requests per minute]

## Endpoints
[List of endpoints with methods]

## Detailed Operations
[Each operation with request/response formats]

## Error Scenarios
[Common errors and remediation]

## Contract Evolution
- **Change Policy**: [How changes are communicated]
- **Deprecation Policy**: [Timeline for feature deprecation]
- **Version Lifecycle**: [Support timeline for this version]

# API Contract: Payments API

## Contract Overview
- **Version**: 2023-08-15
- **Release Date**: August 15, 2023
- **Stability**: Stable

## Provider Obligations
- **Availability**: 99.95% uptime
- **Performance**: 95% of requests processed under 250ms
- **Data Security**: PCI-DSS Level 1 compliance
- **Idempotency**: Guaranteed for requests with Idempotency-Key header

## Consumer Obligations
- **Authentication**: OAuth 2.0 or API key required for all requests
- **Request Validation**: All required fields must be provided and valid
- **Rate Limits**: Maximum 120 requests per minute per API key
- **Compliance**: No storage of full card data in your systems

## Endpoints
| Endpoint | Method | Description |
|----------|--------|-------------|
| /v1/payments | GET | List payments |
| /v1/payments | POST | Create payment |
| /v1/payments/{id} | GET | Retrieve payment |
| /v1/payments/{id}/refund | POST | Refund payment |

## Detailed Operation: Create Payment
[Detailed request/response information follows]

## Error Scenarios
- **Invalid Card**: Returns 402 Payment Required with error_code "card_declined"
- **Rate Limit Exceeded**: Returns 429 Too Many Requests with retry_after header
- **System Unavailable**: Returns 503 Service Unavailable during maintenance windows

## Contract Evolution
- **Change Policy**: Non-breaking changes with 30 days notice
- **Deprecation Policy**: Deprecated features supported for 6 months
- **Version Lifecycle**: This version will be supported until August 15, 2025

```
Rate Limit: 100 requests/minute

Request Rate over Time:
    
  Requests
  per second
     │
  25 │                ┌───┐
     │                │   │
  20 │                │   │
     │                │   │
  15 │        ┌───┐   │   │
     │        │   │   │   │
  10 │    ┌───┼───┼───┼───┼───┐
     │    │   │   │   │   │   │
   5 │┌───┼───┼───┼───┼───┼───┼───┐
     ││   │   │   │   │   │   │   │
   0 ││   │   │   │   │   │   │   │
     └┴───┴───┴───┴───┴───┴───┴───┴───> Time (seconds)
       10  20  30  40  50  60  70  80

                     │   │
                     │   │
                     └───┘
                 429 Too Many 
                   Requests
                 (Contract Violation)
```


# Error Catalog

## Error Format

All API errors return a consistent JSON structure:

```json
{
  "error": {
    "type": "error_type",
    "code": "specific_error_code",
    "message": "Human-readable message",
    "param": "related_parameter",
    "documentation_url": "https://example.com/docs/errors/specific_error_code"
  }
}
```
| Status Code | Error Type | Description |
|-------------|------------|-------------|
| 400 | invalid_request | The request was malformed or missing required parameters |
| 401 | authentication_error | Authentication failed or was not provided |
| 403 | permission_error | The authenticated user lacks required permissions |
| 404 | resource_not_found | The requested resource does not exist |
| 429 | rate_limit_error | Too many requests in a given time period |

### 2. Contract Violation Response Examples

**Documentation Example:**


## Contract Violation Examples

### Example 1: Invalid Parameter

Request with invalid currency code:

```http
POST /v1/payments HTTP/1.1
Host: api.example.com
Authorization: Bearer valid_api_key
Content-Type: application/json

{
  "amount": 100.00,
  "currency": "INVALID",
  "payment_method_id": "pm_123456789"
}

HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": {
    "type": "invalid_request",
    "code": "invalid_currency",
    "message": "Currency code 'INVALID' is not valid. Please use ISO 4217 format (e.g., USD, EUR).",
    "param": "currency",
    "documentation_url": "https://example.com/docs/errors/invalid_currency"
  }
}

HTTP/1.1 429 Too Many Requests
Content-Type: application/json
Retry-After: 30

{
  "error": {
    "type": "rate_limit_error",
    "code": "too_many_requests",
    "message": "Rate limit exceeded. Please retry after 30 seconds.",
    "documentation_url": "https://example.com/docs/errors/rate_limits"
  }
}
```
### 3. Error Remediation Guide

**Documentation Example:**

## Error Remediation Guide

### invalid_currency

**Problem**: The provided currency code does not conform to ISO 4217 standard.

**Solution**:
1. Ensure the currency code is exactly 3 letters (e.g., USD, EUR, GBP)
2. Verify the currency is supported by checking the [Supported Currencies](#supported-currencies) list
3. Use uppercase for all currency codes

```javascript
// Incorrect
const payment = {
  amount: 100,
  currency: "us dollars" // Invalid format
};

// Correct
const payment = {
  amount: 100,
  currency: "USD" // Valid ISO 4217 code
};
```
too_many_requests
Problem: Your application has exceeded the permitted number of requests in the given time period.

Solution:

Implement exponential backoff with the Retry-After header value
Add rate limiting in your client application
Consider request batching for bulk operations
```
// Implementation example using exponential backoff
async function makeRequestWithBackoff(url, options, maxRetries = 3) {
  for (let attempt = 0; attempt <= maxRetries; attempt++) {
    try {
      const response = await fetch(url, options);
      
      if (response.status !== 429) return response;
      
      const retryAfter = response.headers.get('Retry-After') || Math.pow(2, attempt);
      console.log(`Rate limited. Retrying in ${retryAfter} seconds...`);
      await new Promise(resolve => setTimeout(resolve, retryAfter * 1000));
    } catch (error) {
      if (attempt === maxRetries) throw error;
    }
  }
}
```
## Versioning and Evolution Documentation

### 1. Versioning Strategy Documentation

**Documentation Example:**

# API Versioning Strategy

Our API uses date-based versioning to provide a clear, predictable evolution path.

## Version Format

Versions are specified using the date format: `YYYY-MM-DD`

Example: `2023-08-15`

## How to Specify API Version

### In HTTP Header (Preferred)

```http
X-API-Version: 2023-08-15

In URL Path
https://api.example.com/2023-08-15/payments
```
Version Lifecycle
Preview: New versions are released as previews for at least 30 days
Stable: After preview period, versions are considered stable
Deprecated: When replaced by newer versions (announced 6 months in advance)
Sunset: End-of-life (minimum 1 year after deprecation notice)

| Version | Status | Released | Deprecation Date | Sunset Date |
|---------|--------|----------|------------------|-------------|
| 2023-08-15 | Stable | Aug 15, 2023 | Not scheduled | Not scheduled |
| 2023-01-10 | Deprecated | Jan 10, 2023 | Aug 15, 2023 | Aug 15, 2024 |
| 2022-05-30 | Sunset | May 30, 2022 | Jan 10, 2023 | Jan 10, 2024 |

### 2. Change Notification Documentation

**Documentation Example:**


# Change Log

## Version 2023-08-15

**Release Date**: August 15, 2023

### Breaking Changes

- Removed `legacy_id` field from Payment response
- Changed authentication mechanism from API key to OAuth 2.0 (API keys still supported until 2024-02-15)

### New Features

- Added support for installment payments via new `installments` parameter
- Introduced `metadata` object for custom data storage
- Added new endpoint: `GET /v1/payments/summary` for aggregated reporting

### Improvements

- Increased rate limits from 100 to 120 requests per minute
- Reduced average response time by 40%
- Enhanced error messages with more specific remediation guidance

## Version 2023-01-10

**Release Date**: January 10, 2023

### Breaking Changes

- [Change details...]

# Migration Guide: 2023-01-10 to 2023-08-15

This guide helps you migrate from API version 2023-01-10 to 2023-08-15.

## Authentication Changes

### Before: API Key in Query Parameter

```http
GET /v1/payments?api_key=your_api_key HTTP/1.1
Host: api.example.com

GET /v1/payments HTTP/1.1
Host: api.example.com
Authorization: Bearer your_oauth_token

or

GET /v1/payments HTTP/1.1
Host: api.example.com
Authorization: ApiKey your_api_key

Response Format Changes
Before: Payment Response
{
  "id": "pmt_123456",
  "legacy_id": "old_123456",  // Deprecated field
  "amount": 100.00,
  "status": "completed"
}

After: Payment Response

{
  "id": "pmt_123456",
  // No legacy_id field
  "amount": 100.00,
  "status": "completed",
  "metadata": {}  // New field
}
Code Samples for Migration
Node.js Example
// Before (v2023-01-10)
const getPayment = async (paymentId) => {
  const response = await fetch(
    `https://api.example.com/v1/payments/${paymentId}?api_key=${API_KEY}`
  );
  const data = await response.json();
  return {
    id: data.id,
    legacyId: data.legacy_id, // This field will be missing in the new version
    amount: data.amount
  };
};

// After (v2023-08-15)
const getPayment = async (paymentId) => {
  const response = await fetch(
    `https://api.example.com/v1/payments/${paymentId}`,
    {
      headers: {
        Authorization: `Bearer ${OAUTH_TOKEN}` // Or `ApiKey ${API_KEY}`
      }
    }
  );
  const data = await response.json();
  return {
    id: data.id,
    // No more legacyId
    amount: data.amount,
    metadata: data.metadata || {} // Handle new field
  };
};
```

Practical Documentation Exercises
1. Contract Documentation Template Exercise
Example Exercise Instructions:

# Exercise: Create an API Contract Documentation Template

## Background
You are a technical writer at FinTech Inc., developing a documentation framework for a new payments API. Your task is to create a comprehensive template that explicitly frames the API as a contract between systems.

## Requirements
Your template should include sections for:

1. Contract Overview
2. Provider Obligations
3. Consumer Obligations
4. Endpoint Reference
5. Request/Response Formats
6. Error Scenarios
7. Contract Evolution

## Deliverables
1. Markdown template with all required sections
2. Brief explanation of your template design decisions
3. Example of one endpoint fully documented using your template

## Evaluation Criteria
- Completeness: All contract elements are addressed
- Clarity: Template is easy to understand and follow
- Flexibility: Template can adapt to different API types
- Contract Focus: Explicit framing of API as a contract
## Resources and Tools

### 1. API Documentation Platforms

**Comparison Table:**

# API Documentation Platforms

| Platform | Features | Best For | Pricing |
|----------|----------|----------|---------|
| **Swagger UI** | - OpenAPI specification<br>- Interactive documentation<br>- Try-it-out functionality | Teams using OpenAPI/Swagger | Free, open-source |
| **ReadMe** | - Developer portal<br>- API explorer<br>- Analytics<br>- Versioning | Full-featured developer experience | Starts at $99/mo |
| **Redoc** | - Clean, responsive design<br>- OpenAPI-based<br>- Customization options | Simple, clean documentation | Free, open-source |
| **Stoplight** | - Visual API design<br>- Mock servers<br>- Style guides<br>- Collaborative editing | API design-first workflows | Free tier available |
| **Postman** | - API testing<br>- Documentation generation<br>- Collection sharing | API development teams | Free tier available |

# API Documentation Style Guide

## Contract Language

### Do's and Don'ts

✅ DO:
- Use precise, specific language for contract terms
- Clearly differentiate between required and optional parameters
- Specify constraints with exact values
- Provide complete request and response examples

❌ DON'T:
- Use ambiguous language like "might," "maybe," or "sometimes"
- Leave authentication requirements implicit
- Omit error scenarios and remediation
- Use inconsistent terminology

# Learning Resources for API Technical Writers

## Books

1. **"The Design of Web APIs" by Arnaud Lauret**
   - Focuses on API design from a user experience perspective
   - Excellent coverage of the "API as contract" concept

2. **"API Documentation Made Easy" by Peter Gruenbaum**
   - Specifically for technical writers
   - Practical exercises for learning API documentation

3. **"Modern Technical Writing" by Andrew Etter**
   - Covers documentation as code approaches
   - Lightweight processes for technical documentation




  
