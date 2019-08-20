### Okhttp
#### Intercrptors
Interceptors are a powerful mechanism that can monitor, rewrite, and retry calls.
OkHttp uses lists to track interceptors, and interceptors are called in order.
![](https://github.com/square/okhttp/raw/master/docs/images/interceptors@2x.png)

##### Application interceptors VS Network InterCeptors:
##### Application Interceptors
- Don't need to worry about intermediate responses like redirects and retries.
- Only invoked once,even if response is from the cache.And must be invoked when request.
- Observe the application's original intent. Unconcerned with OkHttp-injected headers like If-None-Match.
- Permitted to short-circuit and not call Chain.proceed().
- Permitted to retry and make multiple calls to Chain.proceed().
##### Network Interceptors
- Oble to operate on intermediate response like redirects and retries.
- Not invoked for cached responses that shot-circuit the network.


# TODO
- Accept-Encoding: gzip

# Thanks:
https://github.com/square/okhttp/blob/master/INTERCEPTORS.md