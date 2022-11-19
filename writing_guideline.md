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
