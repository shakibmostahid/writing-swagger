# What is Swagger Documentation? 

Swagger is the standard way of documenting REST APIs. *Swagger is a set of open-source tools built around the **OpenAPI Specification** that can help to design, build, document and consume REST APIs.* This is a popular framework which used for documenting the APIs in a common format.

To learn more about Swagger, please follow the [documentation](https://swagger.io/docs/specification/about/ "documentation").

# Why is Swagger is helpful? 

We can think swagger documentation as a blueprint of the APIs. By checking the blueprint developer can get an idea of the task of an API. Swagger gives a visual idea of the APIs, where user doesn’t need to know what technologies is being used to develop the APIs. API is documented in a common format in swagger where HTTP Verb, resopnse samples, request samples, description, etc is present. so anyone can get a clear picture of the API just by checking it. 

# How to Write Swagger Documentation?

Swagger can be written in yml and json. We will use yml here. And, we will use [Swagger Editor tool](https://editor.swagger.io/ "Swagger Editor tool") to write the documentation. Here, we will use an example of User Management APIs, where user details can be Retrieved, Updated and Deleted.

#### Let’s Start:
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

#### Now after adding the response example lines into swagger editor, the output will be as below:

![swagger_response_output](https://github.com/shakibmostahid/writing-swagger/blob/main/images/swagger_2.png?raw=true)

------------

- We might need the example of `user` object on others API. For example, we will return the same user object after creating an user. Then what we can do is copy pasting the response content. That would definitely work, but the contents of the file keeps increasing. It would be better if we can avoid the duplication.
The solution for this problem is to add common definitions in the global `components` attribute and add the references using `$ref` of the definitions wherever we need. 
Let's write the user schema under `components` attribute:

```yaml
components:
  schemas:
    User:
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

Here we have added the user details under `schemas` atrribute which parent element is `components`. We have named our schema with `User`. Then we have added the schema details. 

Now, our next task is to add the references in the success response content. We need to remove the user schema from there, and need to add only one line to refer the object. So the changes would be like: 

```yaml
content:
  application/json:
    schema:
      "$ref": "#/components/schemas/User"
```

We have just set the reference under `schema` by setting the value of `$ref` attribute. The format is `#/components/<type>/<OjbectName>`. Our type is `schemas` and our object name is `User`. That's why we have set the value `#/components/schemas/User`.

After adding these changes, the response object would be as previous. But there will be shown a new component named `Schemas`.
That would look like:

![swagger_schema_output](https://github.com/shakibmostahid/writing-swagger/blob/main/images/swagger_3.png?raw=true)

------------

- In the example, we have used user's `id` in path with the value 1. So, the path become, `/users/1`, which looks like a static route. But our route is dynamic and it is based on the user's unique id. Swagger give us the ability to make the path dynamic. We need to add placeholder in the `id` portion. To add place holder we use curly braces `{}`, and inside the curly braces we add the variable name. Now, we will make changes in the `path` section like this:

```yaml
path:
  users/{id}:
    get:
```

Now after adding this, we will see an semantic error saying `Declared path parameter "id" needs to be defined as a path parameter at either the path or operation level`.

To fix the error, we need to add set the information for the `{id}` in the `parameters` attribute. We can have multiple variables for a single route, all the information should be added under `parameters`. We can use variable in query params, body, header as well.

Here we have only one variable `id`, so we will add the information for it only.

```yaml
paths:
  /users/{id}:
    get:
      summary: Get User details
      operationId: getUser
      parameters:
        - in: path
          name: id
          schema:
            type: integer
            example: 1
          required: true
```

Here, as our variable location is in path, that's why we have set `path` value for `in` attribute. Then, we have entered the schema details for `id` variable. We have set example `1` as user's id. 

After making these changes, there will be a new row with parameters information in the output section. It will look like:


![swagger_parameters](https://github.com/shakibmostahid/writing-swagger/blob/main/images/swagger_4.png?raw=true)

------------

