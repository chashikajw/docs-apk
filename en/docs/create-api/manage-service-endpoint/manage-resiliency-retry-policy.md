# Retry Policies

The Retry Policy configuration defines how the APK should handle failed API requests. It allows you to specify the number of retries, the base retry interval, and the HTTP status codes for which the retries should be attempted. 

<table>
    <thead>
      <tr>
        <th>Configuration</th>
        <th>Description</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="white-space: nowrap;"><code>count</code></td>
        <td>Specifies the maximum number of retry attempts for API backend requests. If the backend returns any of the specified response codes, the APK will automatically retry the request according to the specified count.</td>
      </tr>
      <tr>
        <td style="white-space: nowrap;"><code>baseIntervalInMillis</code></td>
        <td>Specifies the base retry interval in milliseconds. The APK will wait for this interval before attempting the next retry.</td>
      </tr>
      <tr>
        <td style="white-space: nowrap;"><code>statusCodes</code></td>
        <td>Specifies an array of HTTP status codes (as integers) for which the APK should attempt retries. If the backend returns any of these status codes, the APK will trigger the retry mechanism.</td>
      </tr>
    </tbody>
</table>


You can configure retry policy to a backend using following methods:


## Via Rest API Interface

When deploying APIs add the configurations to the `apk-conf` file as shown in the following sample.

```
endpointConfigurations:
 production:
  endpoint: "http://backend-service.ns:443"
  resiliency:
   retryPolicy:
    count: 3
    baseIntervalMillis: 200
    statusCodes:
    - 500
    - 501
    - 502
```

## Via CRs

Define the Backend resource for the API as below and apply to the respective namespace.
```
apiVersion: dp.wso2.com/v1alpha1
kind: Backend
metadata:
  name: sample-backend
  namespace: ns
spec:
  protocol: http
  retry:
    baseIntervalMillis: 200
    count: 3
    statusCodes:
    - 500
    - 501
    - 502
  services:
  - host: backend-service.ns
    port: 443
```

For more information, see [x-envoy-max-retries](https://www.envoyproxy.io/docs/envoy/v1.24.1/configuration/http/http_filters/router_filter#config-http-filters-router-x-envoy-max-retries) in the official Envoy documentation.

