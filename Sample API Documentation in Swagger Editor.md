# How to Create Swagger Documentation for a REST API

Swagger documentation is integral for REST APIs as it outlines HTTP verb methods (GET, POST, PUT, PATCH, DELETE) along with request and response parameters and their types. You’ll achieve this using Swagger Editor, a free online tool designed for creating accurate API documentation.

## Steps to Create Swagger Documentation

### 1. Choose the Endpoint

Firstly, determine which endpoint you want to document. For example, let’s start with **authentication** endpoints. The **Authentication** API provides endpoints for user authentication and token management. It allows clients to log in and refresh access tokens using simple POST requests.

## Create the swagger documentation (swagger editor)
Once you identified the endpoint for which you want to create the documentation, it’s time to create it.

Open a new tab in your browser and go to this URL: Swagger Editor. You will see the page below.
<img width="500" alt="1" src="https://github.com/user-attachments/assets/13a1bd6a-2340-4e3a-926e-4d95b1a94138" />
Our
This is the default example of Open API documentation for a Petstore API. Your goal is to create Open API 3.0.x documentation for your endpoint.

You wonder what is the difference between Open API and Swagger 2.0 documentation. Mainly there are 2 differences.

* **Swagger 2.0** is the old definition for REST API and it is written in **JSONformat**.<br>
* **Open API 3.0.X** is the new definition for REST API and it is written in **YAML**.****
  
But you will be writing for OpenAPI 3.0.

Now, Select and remove everything from the editor, and you will see this page.

![Screenshot 2025-05-02 223534](https://github.com/user-attachments/assets/3ce23dc8-8e19-4e47-bac6-bed6c603300d)

copy and paste the below code in the swagger editor
```yaml
openapi: 3.0.0
info:
  title: Authentication API
  description: API for user authentication and authorization
  version: 1.0.0

paths:
  /auth/login:
    post:
      summary: User login
      tags:
        - Authentication
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - email
                - password
              properties:
                email:
                  type: string
                  format: email
                password:
                  type: string
                  format: password
      responses:
        '200':
          description: Login successful
          content:
            application/json:
              schema:
                type: object
                properties:
                  token:
                    type: string
                  refreshToken:
                    type: string
        '401':
          description: Invalid credentials

  /auth/refresh:
    post:
      summary: Refresh access token
      tags:
        - Authentication
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - refreshToken
              properties:
                refreshToken:
                  type: string
      responses:
        '200':
          description: Token refreshed
        '401':
          description: Invalid refresh token
```

In this code we will talk about 2 parts:

* info <br>
* paths<br>

**info**
In the “info” section, you have to provide the title, description, and version of the documentation, You will receive an error which you will fix later on.
<img width="941" alt="image" src="https://github.com/user-attachments/assets/00b92628-8f5a-4beb-a7b9-6696f39a9847" />

**paths**
In the “paths” section, you define the final part of the endpoint URL, specify the HTTP verb methods ( POST), and outline the parameters, including their types, as well as the structure for request and response bodies.

<img width="952" alt="image" src="https://github.com/user-attachments/assets/e0c09edb-a7da-45e2-82a2-435553c7240d" />

Let’s go through the YAML code line-by-line, explaining each keyword and element used in the OpenAPI specification for the Authentication API:

```openapi: 3.0.0```
**openapi**: This indicates the version of the OpenAPI spec being used, which is 3.0.0 here. It sets the format for how the rest of the API documentation is structured.

```info:
  title: Authentication API
  description: API for user authentication and authorization
  version: 1.0.0
```
  **info:** This object provides metadata about the API. It includes details to help users understand what the API is about.<br> 
 **title:** The name of the API. Here it is "Authentication API."<br> 
  **description:** A short description explaining the purpose and functionality of the API. It describes that the API is for user authentication and authorization.
  **version:** Indicates the version of the API. In this case, it is version "1.0.0."

```paths:
```
**paths:** This key is a container for the available paths and operations in the API. It maps a list of resources, each having one or more operation objects guiding their behavior.
```  /auth/login:
        post:
```
**/auth/login:** This is the endpoint path. It defines a specific URL route where clients can send HTTP requests to perform operations. The specific operation here is to "login."

**post:** This HTTP method is used to create new resources or authenticate users. Here, it is used for login functionality where users send their credentials to gain access.

```summary: User login
      tags:
        - Authentication
```
**summary:** A short explanation of what the operation does. It summarizes the action, which is "User login."

**tags:** Tags are used for organizing operations logically. They help in grouping related operations such as Authentication.
```
      requestBody:
        required: true
        content:
          application/json:
```
**requestBody:** This section specifies the details of the request body that the endpoint expects.

   * **required:** A boolean indicating whether a request body is mandatory. Here, true means it is required.
   * **content:** Describes the media type of the request body. In this case, it is application/json.

```      schema:
              type: object
              required:
                - email
                - password
              properties:
                email:
                  type: string
                  format: email
                password:
                  type: string
                  format: password
```
**schema:** Defines the structure and constraints of the request body.
   * **type: object:** Indicates that the request body must be a JSON object.
   * **required:** Specifies the fields required in the JSON object. Here email and password are required.
   * **properties:** Lists the fields in the request object and their types.
   * **email:** The type is string, and the format is email, indicating the value should be a valid email.
   * **password:** The type is string, and the format is password, implying a string with unspecified constraints for passwords.
     
```
      responses:
        '200':
          description: Login successful
          content:
            application/json:
              schema:
                type: object
                properties:
                  token:
                    type: string
                  refreshToken:
                    type: string
```
**responses:** This section defines the possible responses from the endpoint.
* **'200':** An HTTP status code for success. It signifies a successful login.
* **description:** A human-readable description of what the response means.
* **content:** Specifies the type of content returned. Here, application/json is returned on success.
* **schema:** Describes the structure of the response JSON object.
* **properties:** Defines the fields returned.
* **token:** A string type, typically a JWT or session token.
* **refreshToken:** A string type used to refresh access.
     ```   '401':
                description: Invalid credentials
     ```
**'401':** An HTTP status code indicating unauthorized access. It means credentials are invalid.
      * **description:** Describes the condition under which this response is returned.

```
/auth/refresh:
```
**/auth/refresh:** This is another endpoint path that allows users to refresh their access token.

```  
post:
      summary: Refresh access token
      tags:
        - Authentication
      requestBody:
        required: true
        content:
          application/json:
            schema:
              type: object
              required:
                - refreshToken
              properties:
                refreshToken:
                  type: string
```
This structure is similar to the /auth/login endpoint, with the key action being "Refresh access token." It requires a refreshToken in the request body to issue a new access token.
```    
responses:
        '200':
          description: Token refreshed
```
**'200'** indicates a successful refresh of the token, with the new token usually provided in the response body.

  ```  
'401':
          description: Invalid refresh token
```
**'401':** Indicates that the refresh token provided is invalid, hence access is not authorized.
This documentation serves as a reference for how clients should interact with the Authentication API, detailing request and response structures, required parameters, and possible outcomes. It helps developers implement the API efficiently and ensures that API behavior is predictable and consistent.





