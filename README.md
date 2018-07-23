# Gota Framework!
**Gota Framework** support developer build  services base on **NodeJS** Environment and **TypeScript** language.

# Create A Service Class

## Class Mapping
```javascript
 function Service(mapping: {
    name?: string;
    path: string | Array<string>;
    config?: object;
    models?: Array<any>;
})
```

 - *name*: name of service, default is class name.
 - *path*: url(s) of service, default is **Hyphen(Dash)** case of class name. (https://jamesroberts.name/blog/2010/02/22/string-functions-for-javascript-trim-to-camel-case-to-dashed-and-to-underscore/)
 - *config*: dependency injection (more detail).
 - *models*: auto generate service  (more detail).
 
Use decorator *Service* before a class.

Example:
```javascript
@Service({path:'/user-service'})
export class UserService{
    ..........
}
```
## Method Mapping
```javascript
function ServiceMapping(mapping: {
    path: string | Array<string>;
    requestMethod?: string | Array<string>;
})
```
 - *path*: url(s) of service, default is **Hyphen(Dash)** case of method name. 
 - *requestMethod*: method(s) GET | POST | PATCH | PUT | DELETE, default is GET

Use decorator *Mapping* before a method.
**Return** data or **Awaited** data will be convert to JSON and response to client.

Example:
```javascript
@Service({path:'/user-service'})
export class UserService{
	...
    @ServiceMapping({path: '/users'})
    async readAll():Promise<User[]>{
        ...
    }
    
}
```

## Parameter Mapping
We use one of decorators: *PathParameter, Body, BodyParameter, Query, QueryParameter, Headers, HeadersParameter, Request, Response* before a argument of method.
Framework will find and convert data before method execute.

### PathParameter:
Mapping with url parameter.
Start of PathParameter is ':' character.
```javascript
...
    @ServiceMapping({path: '/users/:userId', requestMethod: 'POST'})
    async get(@PathParameter userId: string):Promise<User[]>{
        ...        
    }
```
PathParameter name in url (path: '/users/:**userId**') and argument name in method (**userId**: string) must be the same.


### BodyParameter 
Example:
```javascript
...
    async create(@BodyParameter firstName : string, @BodyParameter lastName : string, @BodyParameter email: string, @BodyParameter phone: string):Promise<User[]>{
        let user: User = new User(firstName, lastName, email, phone);
        ...        
    }    
...
```
Framework will find  parameters (firstName, lastName, email, phone) in request body and map with arguments in method by naming.

### Body
Example:
```javascript
...
    @ServiceMapping({path: '/users', requestMethod: 'POST'})
    async create(@Body body: object):Promise<User[]>{
        let user: User = new User(body.firstName, body.lastName, body.email, body.phone);
        ...        
    }    
...
```
Framework will collect all parameters in request body and map with a argument (has @Body decorator) in method.

### Query, QueryParameter, Headers, HeadersParameter
(same with below cases)

### Request
Framework will collect all information in request and map with a argument (has @Request decorator) in method.

### Response
Framework will collect all information in request and map with a argument (has @Request decorator) in method.

# Start A Service
```javascript
...
    async create(@BodyParameter firstName : string, @BodyParameter lastName : string, @BodyParameter email: string, @BodyParameter phone: string):Promise<User[]>{
        let user: User = new User(firstName, lastName, email, phone);
        ...        
    }    
...
```
# Dependency injection
## Config
## Autowired
 ```javascript
