# Token-based Authentication

For creating a secure and seamless user authentication flow is important. A common way to achieve this is by using JSON Web Tokens (JWTs) for user sessions. JWTs offer both security and efficiency.
- Access Token: A short-lived token used to authorize specific API requests. It grants access to resources on the server. Due to security reasons, access tokens typically have a short expiration time.
- Refresh Token: A long-lived token used to acquire new access tokens when the current one expires. Refresh tokens typically have a much longer expiration time than access tokens.


## Interceptor Design

1. **Make an HTTP Request**: The client initiates the process by setup up for HTTP request.
2. **Interception**: Invoke request interceptor to check validation of access token before hitting the actual endpoint.
3. **Is Access Token Valid?**: Check access token is valid with `JwtDecoder.isExpired()`
    - Yes: Continue with HTTP request
    - No: Create a new API request with to obtain new accessToken using refresh Token
3. **Refresh Token expired**: Performed a force Logout.

![Alt text](../../../assets/refresh_token.png)

```dart
class AuthDioClient extends QueuedInterceptor{

     @override
  void onRequest(
      RequestOptions options, RequestInterceptorHandler handler) async {
    options.headers.addAll(
        {HttpHeadersConst.kTenantKeyHeader: preferenceHelper.tenantCode});
    String? accessToken =
        await securePreferenceHelper.readValue(AppPreference.accessToken);
    String? refreshToken =
        await securePreferenceHelper.readValue(AppPreference.refreshToken);

    /// If both token is null, then the request is made without the token
    if (accessToken.isNullOrEmpty() && refreshToken.isNullOrEmpty()) {
      handler.next(options);
      return;
    }

    /// If token is valid, then the request is made with the token
    if (TokenValidation.isAccessTokenValid(accessToken)) {
      options.headers[HttpHeadersConst.kHeaderAuthorization] =
          'Bearer $accessToken';
      handler.next(options);
      return;
    }

    /// If token is not valid and can be refreshed, then the token is refreshed
    if (TokenValidation.isRefreshTokenValid(refreshToken)) {
      accessToken = await fetchValidAccessToken(refreshToken!);
      if (TokenValidation.isAccessTokenValid(accessToken)) {
        options.headers[HttpHeadersConst.kHeaderAuthorization] =
            'Bearer $accessToken';
        handler.next(options);
        return;
      }
    }

    /// If token is not valid and cannot be refreshed,
    /// then user should be logged out and redirected to login page


    cleanUpOnLogout();
    return handler.reject(DioException(
        requestOptions: options,
        error: 'Token is not valid and cannot be refreshed',
        type: DioExceptionType.cancel,
        response: Response(requestOptions: options)));
  }

    Future<String?> fetchValidAccessToken(String refreshToken) async {
    String accessToken = '';

    try {
      /// We use a new Dio(to avoid dead lock) instance to request token
      final tokenDio = Dio(BaseOptions(
        baseUrl: dio.options.baseUrl,
        connectTimeout: dio.options.connectTimeout,
        receiveTimeout: dio.options.receiveTimeout,
      ));
      final response = await tokenDio.get('/${Apis.tokenRefresh}',
          options: Options(
            contentType: HttpHeadersConst.kApplicationJson,
          ));
      if (response.statusCode == 200) {
        accessToken = response.data['accessToken'];
        await securePreferenceHelper.writeValue(
            AppPreference.accessToken, accessToken);
        return accessToken;
      } else {
        debugPrint('AuthDioClient: Failed to fetch valid access token');
        await cleanUpOnLogout();
        return null;
      }
    } catch (e) {
      debugPrint(
          'AuthDioClient: Failed to fetch valid access token ${e.toString()}');

      await cleanUpOnLogout();

      return null;
    }
  }
}
```
Add your interceptor implementation to dio interceptor.

```dart
dio.interceptors.add(AuthDioClient(
        securePreferenceHelper: getIt<SecurePreferenceHelper>(),
        preferenceHelper: getIt<PreferenceHelper>(),
        dio: dio))
```

## Additional Notes:
-  Access tokens and refresh tokens are saved encrypted in `flutter_secure_storage`.
-  This interceptor can be customized based on the token refresh logic implemented in the backend.
-  If the backend has specific token refresh endpoints or logic, those can be integrated into the token refresh logic section of the interceptor.