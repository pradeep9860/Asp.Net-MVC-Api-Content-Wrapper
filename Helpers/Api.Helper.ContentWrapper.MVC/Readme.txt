# Api.Helper.ContentWrapper.MVC

The Api.Helper.ContentWrapper.MVC is a global exception handler and response wrapper for ASP.NET Web APIs. It uses a ExceptionFilterAttribute to capture exceptions and uses a DelegatingHandler to capture HTTP response to build a consistent response object for both successful and error requests.

## Prerequisite

Install Newtonsog.Json package

## Installing

1) Declare the following namespace within WebApiConfig.cs

using Api.Helper.ContentWrapper.MVC;
using Api.Helper.ContentWrapper.MVC.Filters;

2) Register the following within WebApiConfig.cs

             // Web API configuration and services
            config.Filters.Add(new Api.Helper.ContentWrapper.MVC.Filters.ApiExceptionFilter());
            config.Filters.Add(new Api.Helper.ContentWrapper.MVC.Filters.ValidateModelStateAttribute());
            config.MessageHandlers.Add(new Api.Helper.ContentWrapper.MVC.WrappingHandler());


3) Done.

## Sample Output 

The following are examples of response output:

Here's the format for successful request with data:

```
{
    
	"Version": "1.0.0.0",
    
	"StatusCode": 200,
     
	"IsSuccess": true,

	"Message": "Request successful.",
    
	"Result": [
		"value1",
        
		"value2"
	]

}
  
```

Here's the format for successful request without data:

```
{
    
	"Version": "1.0.0.0",
    
	"StatusCode": 201,
	"IsSuccess": true, 
	"Message": "Student with ID 6 has been created."

}
```

Here's the format for error request with validation errors:

```
{
    
	"Version": "1.0.0.0",
    
	"StatusCode": 400,
	"IsSuccess": false,
    
	"Message": "Request responded with exceptions.",
    
	"ResponseException": {
        
		"IsError": true,
        
		"ExceptionMessage": "Validation Field Error.",
        
		"Details": null,
        
		"ReferenceErrorCode": null,
        
		"ReferenceDocumentLink": null,
        
		"ValidationErrors": [
            
			{
                
				"Field": "LastName",
                
				"Message": "'Last Name' should not be empty."
            
			},
            
			{
                
				"Field": "FirstName",
                
				"Message": "'First Name' should not be empty."
            
			}
        ]
    
	}

}
``` 

Here's the format for error request

```
{
    
	"Version": "1.0.0.0",
    
	"StatusCode": 404,
    
	"Message": "Unable to process the request.",

	"IsSuccess": false,
    
	"ResponseException": {
        
		"IsError": true,
        
		"ExceptionMessage": "The specified URI does not exist. Please verify and try again.",

	    "Details": null,
        
		"ReferenceErrorCode": null,
        
		"ReferenceDocumentLink": null,
        
		"ValidationErrors": null
    
	}

} 
```  
          
 

## Using Custom Exception

This library isn't just a custom wrapper, it also provides some objects that you can use for defining your own exception. For example, if you want to throw your own exception message, you could simply do:

```
throw new ApiException("Your Message",401, ModelState.AllErrors());
```

The ApiException has the following parameters that you can set:

```
ApiException(string message,
             int statusCode = 500,
             IEnumerable<ValidationError> errors = null, 
             string errorCode = "", 
             string refLink = "")
```


## Defining Your Own Response Object

Aside from throwing your own custom exception, You could also return your own custom defined Response json by using the ApiResponse object in your API controller. For example:

```
return new APIResponse(201,"Created");
```

The APIResponse has the following parameters:

```
APIResponse(int statusCode, 
	    string message = "", 
	    object result = null, 
            ApiError apiError = null, 
            string apiVersion = "1.0.0.0")
```

## Source Code


 

## Author

  

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md) file for details
