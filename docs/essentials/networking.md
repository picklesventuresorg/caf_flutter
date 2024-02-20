# Networking

We use `Retrofit` together with `Dio` for networking. `Dio` is a HTTP client for Dart and `Retrofit` build on type of `Dio` to make type-safe client by code generation.

## Making HTTP requests

### API Paths
We define all API paths in a separate Apis class for better organization and easy maintenance. For example:

```dart
class Apis {
  static const String login = '/login';
}
```

### API Service
We create an abstract class `AuthApi` annotated with @RestApi(). This class defines all authentication related API endpoints our app will communicate with.
```dart
part 'auth_api.g.dart';

@RestApi()
abstract class AuthApi {
  factory AuthApi(Dio dio, {String? baseUrl}) = _AuthApi;

  @POST(Apis.login)
  Future<HttpResponse> getLogin(
      @Query("username") String username, @Query("password") String password);
  
  @POST(Apis.tokenVerification)
  Future<HttpResponse> verifyToken(@Body() Map<String, dynamic> request);

  @GET(Apis.config)
  Future<ResponseData<Config>> getConfig(@Header('X-Tenant-Code') String tenantCode);

  // Other endpoints...
}
```
Don't forget to add `part` file path declaration to generate boilerplate code for us by `Retrofit`. 
Afterwards, run the following command to trigger the code generation:
```bash
flutter pub run build_runner build --delete-conflicting-outputs
```

### Annotations 

- `@RestApi()`: This annotation is used on an interface or abstract class to create an API service.

- `@GET`,`@POST`,`@DELETE`: These annotations are used on methods to define the HTTP method of the request. The URL of the endpoint is passed as a parameter

- `@Query`:  This annotation is used on method parameters to specify query parameters in the URL.

- `@Body`: This annotation is used on a method parameter to specify the request body of the POST or PUT request.

- `@Path`: This annotation is used on a method parameter to replace a placeholder in the URL path.

- `@Header`:This annotation is used on a method parameter to add a additional header to the request.

For more detailed information, please refer to the official Retrofit documentation: [Retrofit](https://pub.dev/packages/retrofit).


### Error Handling
Network error handling is managed by catching `DioException` instances. `DioException` is a custom error type provided by the `Dio` package, which includes various details about the error, such as the response data, request details, and an error message.

When a network request fails, a `DioException` is thrown. This exception is caught in a try-catch block, and then it's converted into a `DataException` using the `DataException.fromDioError(e)` method. 
```dart
try {
  // Network request
} on DioException catch (e) {
  throw DataException.fromDioError(e);
}
```

This `DataException` can then be used to display a user-friendly error message or to handle the error in a specific way based on the error type. You can customize the DataException class to match your API's error responses.


