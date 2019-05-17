---
id: modules
title: Ionic Module
sidebar_label: Ionic Module
---

## HTTP Interceptor
---

### Description 
Global application of request (headers [token, content-type, etc]) and responses (error code handling [401,404,500])

The cycle:

![Image of Cycle](https://kmramirez3.github.io/ionic-documentation/img/http_interceptor.png)

Every time the request or response has been fired, the request/response will cross in the interceptor.

### Method
```js
intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
```

Parameters
```
req	HttpRequest	
next HttpHandler
```

Returns
```js
Observable<HttpEvent<any>>
```

Typically an interceptor will transform the outgoing request before returning `next.handle(transformedReq)`. An interceptor may choose to transform the response event stream as well, by applying additional Rx operators on the stream returned by `next.handle()`.

More rarely, an interceptor may choose to completely handle the request itself, and compose a new event stream instead of invoking `next.handle()`. This is acceptable behavior, but keep in mind further interceptors will be skipped entirely.

It is also rare but valid for an interceptor to return multiple responses on the event stream for a single request. (source: <https://angular.io/api/common/http/HttpInterceptor#intercept>)

```js
const dupReq = req.clone({ headers });
```

`dupReq` will duplicate the request data and will append the headers inside the clone function

```js
return next.handle(dupReq);
```

`next.handle` function will return the modified request and will be sent to the api

### Usage
```js
 intercept(req: HttpRequest<any>, next: HttpHandler): Observable<HttpEvent<any>> {
        const ignore = '/u/auth/login';
        return fromPromise(this.storage.get('token')).pipe(switchMap(token => {
            if (req.url.search(ignore) === -1) {
                const headers = new HttpHeaders({
                    'Authorization': `Bearer ${token}`,
                    'Content-Type': 'application/json' 
                })
                const dupReq = req.clone({ headers });
                return next.handle(dupReq);
            } else {
                return next.handle(req)
            }
        }))
    }
```

Inside the intercept block, there are 3 process that includes:

- Checking for Http request that do not need to modify
```js
const ignore = '/u/auth/login';
```
the value of `ignore` will be skipped in and will directly return the raw request
```js
if (req.url.search(ignore) === -1)
```
The value -1 means the `ignore` variable value is not present in the string url from `req.url` request

- Adding headers to qualified requests

If the `req.url` value equates to -1, the header will append in the request
```js
 const headers = new HttpHeaders({
                    'Authorization': `Bearer ${token}`,
                    'Content-Type': 'application/json'
  })
```
using
```js
const dupReq = req.clone({ headers });
```

- Returning of the request

and lastly, return the request
```js
return next.handle(dupReq);
```
