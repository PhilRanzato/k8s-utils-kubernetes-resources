# Reverse Proxy between ReactJS and GOlang

The reverse proxy chosen to connect this ReactJS frontend to its GOlang backend is Nginx.

We are configuring the nginx as:

```nginx
    http {
      upstream backend {
        server "go:80";
      }

      upstream frontend {
        server react:3000;
      }

      server {
        listen 8080;
        server_tokens off;
        # disable any limits to avoid HTTP 413 for large image uploads
        client_max_body_size 0;
        # Add extra headers
        add_header X-Frame-Options DENY;
        add_header Content-Security-Policy "frame-ancestors 'none'";
        location / {
          proxy_pass http://frontend/;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          # When setting up Harbor behind other proxy, such as an Nginx instance, remove the below line if the proxy already has similar settings.
          proxy_set_header X-Forwarded-Proto $scheme;
          proxy_buffering off;
          proxy_request_buffering off;
        }
        location /api/ {
          proxy_pass http://go/api/;
          proxy_set_header Host $host;
          proxy_set_header X-Real-IP $remote_addr;
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
          # When setting up Harbor behind other proxy, such as an Nginx instance, remove the below line if the proxy already has similar settings.
          proxy_set_header X-Forwarded-Proto $scheme;
          proxy_buffering off;
          proxy_request_buffering off;
        }
      }
    }
```

In this way we redirect all traffic that uses the `/api/` endpoint to the golang backend, while all the other traffic is redirected to the frontend.
