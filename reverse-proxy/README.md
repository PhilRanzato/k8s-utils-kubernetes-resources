# Reverse Proxy

Since the frontend is accessed in your web browser, a reverse proxy is needed to make the calls to the backend resolvable in all contexts.

What we are going to do, is to proxy all requests to, let's say, `/api` to the backend.

Without a proxy, if the frontend tries to connect to `http://backend-service/api` the `backend-service` is resolved by your web browser (unsuccessfully).
