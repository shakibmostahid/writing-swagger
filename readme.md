# What is Swagger Documentation? 

Swagger is the standard way of documenting REST APIs. *Swagger is a set of open-source tools built around the **OpenAPI Specification** that can help to design, build, document and consume REST APIs.* This is a popular framework which used for documenting the APIs in a common format.

To learn more about Swagger, please follow the [documentation](https://swagger.io/docs/specification/about/ "documentation").

# Why is Swagger is helpful? 

We can think swagger documentation as a blueprint of the APIs. By checking the blueprint developer can get an idea of the task of an API. Swagger gives a visual idea of the APIs, where user doesn’t need to know what technologies is being used to develop the APIs. API is documented in a common format in swagger where HTTP Verb, resopnse samples, request samples, description, etc is present. so anyone can get a clear picture of the API just by checking it. 

# How to Write Swagger Documentation?

Swagger can be written in yml and json. We will use yml here. And, we will use [Swagger Editor tool](https://editor.swagger.io/ "Swagger Editor tool") to write the documentation. Here, we will use an example of User Management APIs, where user details can be Retrieved, Updated and Deleted.

####Let’s Start:
- First we need to define the OpenAPI version, here we will use version 3.

```yaml
openapi: 3.0.0
```
- Then we need to add information about the projects.

```yaml
info:
  title: User Management APIs
  description: This is a sample project for managing users.
  contact:
    name: John Doe
    email: john@example.com
  version: "1.0"
```
Here, `info` attribute holds the projects information. `title` represents project title. `version` represnts the API version.

------------

- Now, we will add the protocol and domain of our API, also we will add other fixed reourceses which is fixed for all the APIs like API version. All the information will be under `servers` attribute.

 ```yaml
 servers:
 - url: https://example.com/v1
 ```
Here, we have set our API domain URL for HTTPS protocol. So, what will happen if our API supports both HTTP and HTTPS. How will we add HTTP protocol here?
We can simply add another value here in the array.

 ```yaml
 servers:
 - url: https://example.com/v1
 - url: http://example.com/v1
 ```
now, swagger will show us both of the servers information in a dropdown.

- what happens if we have multiple domain names? We can add more values into the array. But there’s another way. We can use variables attribute to make it more dynamic.

```yaml
servers:
- url: "{protocol}://{base_url}/v1"
  variables:
    protocol:
      default: https
      enum:
        - https
        - http
    base_url:
      default: example.com
      enum:
        - example.com
        - bd.example.com
```

here, we defined `protocol` and `base_url` as variable into curly braces, these variable name can be anything. Then we set the value of these under `variables` attribute.
After setting the values we can see multiple dropdowns for each variable.

------------

#### Now after adding these lines into swagger editor, the output will be as below:
![swagger_servers_output](https://github.com/shakibmostahid/writing-swagger/blob/main/images/swagger_1.png?raw=true)

------------

#### We need to add the paths information.

- Now, we will add the API details under `paths` attribute. We will add the API summary and reponse sample in the documentation. For example we have an API for getting customer information for given ID, we will add information for it:

```yaml
paths:
  /users/1:
    get:
      summary: Get User details
      operationId: getUser
      responses:
        200:
          description: API to fetch the user information by user id.
          headers:
            Cache-Control:
              schema:
                type: string
                example: no-cache, private
          content:
            application/json:
              schema:
                type: object
                properties:
                  id:
                    type: integer
                    example: 1
                  first_name:
                    type: string
                    example: Rahim
                  last_name:
                    type: string
                    example: Uddin
                  address:
                    type: object
                    properties:
                      area:
                        type: string
                        example: Mirpur
                      city:
                        type: string
                        example: Dhaka
                      country:
                        type: string
                        example: Bangladesh
```

here, we have set our API path `customers/1` under `paths` attribute. Then we have define the request type. This is a GET request so, we have define it as `get`.
Then we have set a short summary for this API in `summary` attribute. `operationId` represents the identity of the operation, this should be unique.
Then we have added response details under `responses` attribute.
First we need to define the status codes, there can be multiple responses with different status code. Here we have set only one for `200` status, we will add one error response later.
The response body needs to define under `content` attribute.
We need to add the content type here; we have added `application/json` as content type.
Then, response schema needs to define under `schema` attribute.
our response is a JSON object, that’s why we have set `type: object`. As we have set type = object so, we need to define the object details under `properties` attribute.

------------

- Now, we will add the headers info for the success response. We have set the response example under `content` attribute of `200` status, we will add the headers informations under `headers` attribute of `200` status:

```yaml
headers:
  Cache-Control:
    schema:
      type: string
      example: no-cache, private
  Date:
    schema:
      type: string
      example: Thu, 17 Nov 2022 09:52:10 GMT
```
We have added only two headers, we can add as many headers as we want.

#### Now after adding the response example lines into swagger editor, the output for the added codes will be as below:
![swagger_response_output](https://github.com/shakibmostahid/writing-swagger/blob/main/images/swagger_2.png?raw=true)
