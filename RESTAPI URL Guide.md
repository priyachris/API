# API Reference Guide
**GET** ``` https://api.acmestore.com/v2/products/{productId}/variants?status=active&sort=price&order=desc&limit=25&page=3 ```


This product variants API endpoint retrieves active variants of a specific product, 
sorted by price in descending order (highest to lowest), and displaying the third page of results with 25 variants per page.

## What does this API do
* Retrieve all variants of a specific product<br>
* Filter product variants by specific criteria<br>
* Paginate through large collections of variants<br>
* Sort variants based on specific attributes<br>
* Implement filtering for mobile applications with limited display capacity<br>

## Breakdown of URL Structure 
![API_URL](https://github.com/user-attachments/assets/b4b8d5fa-6e70-44d1-9183-42c7a49647f8)

## HTTP Request Analysis: Product Variants API

## API Request 
```
GET /v2/products/SKU12345/variants?status=active&sort=price&order=desc&limit=25&page=3
```
### Explanation
**URL Components:**<br>
**Base URL**: ``` https://api.acmestore.com/v2 ``` <br>
An e-commerce API for "Acme Store" (version 2) <br>

**Resource Path**: ```/products/{productId}/variants``` <br>
**Primary Resource:**  ```/products ``` : Represents the collection of **all products** in the system <br>
**Path Parameter:** `/{productId}` : The curly braces {productId} indicate a **variable** placeholder. When making an actual API call, you **must replace** this with a real product identifier. For example: `/products/12345/variants` <br>
**Sub-Resource:** `/variants` :  Represents a collection of variants that **belong to** the specified product. This establishes a parent-child relationship between products and variants <br>


Accessing variants (different versions/models) of a specific product<br>
```{productId}``` is a placeholder that would be replaced with an actual product identifier<br>

**Query Parameters**:<br>
```status=active``` - Only showing variants that are currently available/in stock<br>
```sort=price``` - Arranging results based on price<br>
```order=desc``` - Sorting from highest price to lowest<br>
```limit=25``` - Showing 25 variants per page<br>
```page=3``` - Displaying the third page of results<br>

**Headers**: (Values sent in HTTP headers, not visible in URL) <br> 
```
Host: api.acmestore.com
Accept: application/json
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```
**Host**: Specifies the domain where the request is being sent <br>
**Accept**: Tells the server the client expects JSON data in response <br>
**Authorization**: Contains a JWT (JSON Web Token) for authenticating the request <br>
  * Bearer indicates the authentication scheme <br>
  * The token (eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...) contains encoded credentials<br>
    
### What This Request Does
This request is:<br>
* Authenticating a user/client using a JWT token<br>
* Requesting product variants for product SKU12345<br>
* Filtering to show only active variants<br>
* Sorting results by price from highest to lowest<br>
* Paginating the results (25 items per page, showing page 3)<br>

##  API Response 
This JSON response represents page 3 of product variants for a specific product.<br>

* The client expects to receive a JSON response containing the requested product variant data.<br>
* The server would validate the authentication token before processing the request and returning data.<br>
```
  {
    "id": "VAR-7821",
    "productId": "SKU12345",
    "name": "Premium Edition - Blue",
    "sku": "SKU12345-BLUE-P",
    "price": 89.99,
    "status": "active",
    "inventory": 32,
    "attributes": {
      "color": "blue",
      "size": "medium",
      "edition": "premium"
    }
```

This contains the actual product variant information:
* Each variant has identifiers, price, status, and inventory count<br>
* The example shows a blue, medium-sized "Premium Edition" variant<br>
* The full response would include up to 25 variants (limited by the pagination settings)<br>

### Response Headers: Metadata about the response
  ```
  "metadata": {
  "total": 147,
  "page": 3,
  "limit": 25,
  "pages": 6
}
```
This provides pagination information:

Total of 147 variants match the query criteria<br>
Currently viewing page 3<br>
Each page shows 25 items maximum<br>
There are 6 total pages of results<br>

## OpenAPI/ Swagger Documentation
```
openapi: 3.0.0
info:
  title: Acme Store API
  version: 2.0.0
  description: API for managing e-commerce products and variants
paths:
  /products/{productId}/variants:
    get:
      summary: Retrieves product variants
      description: Gets a list of all variants for a specific product
      parameters:
        - in: path
          name: productId
          required: true
          schema:
            type: string
          description: The unique identifier of the product
        - in: query
          name: status
          schema:
            type: string
            enum: [active, inactive, all]
          description: Filter variants by status
        - in: query
          name: sort
          schema:
            type: string
          description: Field to sort by
        - in: query
          name: order
          schema:
            type: string
            enum: [asc, desc]
          description: Sort order (ascending or descending)
        - in: query
          name: limit
          schema:
            type: integer
            default: 25
          description: Number of items per page
        - in: query
          name: page
          schema:
            type: integer
            default: 1
          description: Page number
      responses:
        '200':
          description: Successful response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/VariantList'
```

![image](https://github.com/user-attachments/assets/af793fe5-dc03-4ea4-9003-79a6a5cc86e6)

