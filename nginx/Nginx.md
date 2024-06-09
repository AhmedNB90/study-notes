Sure! Here’s a detailed guide to Nginx configuration in Markdown format:

---

# Nginx Configuration Guide

## 1. Configuration File Structure

### Main Configuration File (`nginx.conf`)
- **Location:** `/etc/nginx/nginx.conf`
- **Purpose:** Holds global settings and includes references to other configuration files.
- **Structure:** Typically includes directives within `main`, `http`, `server`, and `location` contexts.

### Site-Specific Configuration
- **Location:** `/etc/nginx/sites-available/` (symlinked to `/etc/nginx/sites-enabled/`)
- **Purpose:** Allows for modular management of site configurations.

## 2. Key Directives and Contexts

### Main Context
- **Global settings** that apply to the entire Nginx instance.
- **Common Directives:**
  ```nginx
  worker_processes auto;            # Automatically adjust to number of CPU cores
  worker_connections 1024;          # Maximum number of simultaneous connections per worker
  pid /var/run/nginx.pid;           # PID file location
  include /etc/nginx/modules-enabled/*.conf;  # Include additional modules
  ```

### HTTP Context
- **Settings for handling HTTP requests.**
- **Common Directives:**
  ```nginx
  http {
      include /etc/nginx/mime.types;  # Define MIME types
      default_type application/octet-stream;
      log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                        '$status $body_bytes_sent "$http_referer" '
                        '"$http_user_agent" "$http_x_forwarded_for"';
      access_log /var/log/nginx/access.log main;  # Specify access log file and format
      sendfile on;                                # Enable efficient file transfer
      keepalive_timeout 65;                       # Keep connections alive for 65 seconds
      gzip on;                                    # Enable gzip compression
      include /etc/nginx/conf.d/*.conf;           # Include additional configuration files
      include /etc/nginx/sites-enabled/*;         # Include site-specific configurations
  }
  ```

### Server Context
- **Defines a virtual server** for handling requests.
- **Common Directives:**
  ```nginx
  server {
      listen 80;                          # Listen on port 80
      server_name example.com www.example.com;  # Specify the server's domain names
      root /var/www/example;              # Set the document root
      index index.html index.htm;         # Default index files

      location / {
          try_files $uri $uri/ =404;      # Try to serve the requested URI, else return 404
      }

      location /images/ {
          alias /data/images/;            # Use an alias for the /images/ directory
      }
  }
  ```

### Location Context
- **Matches URIs to specific configuration rules.**
- **Modifiers:**
  - **None:** Exact match.
  - **`=`:** Exact match for the specified URI.
  - **`~`:** Case-sensitive regex match.
  - **`~*`:** Case-insensitive regex match.
  - **`^~`:** Prefer this match if the URI starts with the specified string.
- **Common Directives:**
  ```nginx
  location /images/ {
      alias /data/images/;               # Map /images/ to /data/images/
      autoindex on;                      # Enable directory listing
  }

  location ~* \.(jpg|jpeg|png|gif)$ {     # Match image files (case-insensitive)
      expires 30d;                       # Cache images for 30 days
      add_header Cache-Control "public, no-transform";  # Add cache control headers
  }
  ```

## 3. Common Configuration Tasks

### Serving Static Content
- **Use `root` or `alias` to specify the directory containing static files.**
- **Enable `autoindex` for directory listings.**
  ```nginx
  server {
      listen 80;
      server_name static.example.com;

      location / {
          root /var/www/static;           # Set document root for static files
          autoindex on;                   # Enable directory listing
      }

      location /images/ {
          alias /data/images/;            # Use an alias for images
      }
  }
  ```

### Reverse Proxy
- **Forward client requests to a backend server using `proxy_pass`.**
- **Include headers like `Host` and `X-Real-IP`.**
  ```nginx
  server {
      listen 80;
      server_name app.example.com;

      location / {
          proxy_pass http://localhost:8080;  # Forward requests to backend
          proxy_set_header Host $host;       # Preserve the original host header
          proxy_set_header X-Real-IP $remote_addr;  # Forward the real client IP
          proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  # Forward the original client IP
      }
  }
  ```

### SSL/TLS Configuration
- **Configure SSL certificates and protocols.**
- **Redirect HTTP to HTTPS using `return 301`.**
  ```nginx
  server {
      listen 80;
      server_name secure.example.com;
      return 301 https://$host$request_uri;  # Redirect HTTP to HTTPS
  }

  server {
      listen 443 ssl;
      server_name secure.example.com;

      ssl_certificate /etc/ssl/certs/example.com.crt;     # SSL certificate file
      ssl_certificate_key /etc/ssl/private/example.com.key;  # SSL certificate key
      ssl_protocols TLSv1.2 TLSv1.3;                      # Supported SSL protocols
      ssl_ciphers HIGH:!aNULL:!MD5;                       # Secure SSL ciphers

      location / {
          root /var/www/secure;            # Set document root for secure content
          index index.html;                # Default index file
      }
  }
  ```

### Load Balancing
- **Define an `upstream` block to specify backend servers.**
- **Use different load balancing algorithms (`round_robin`, `least_conn`, `ip_hash`).**
  ```nginx
  http {
      upstream backend {
          server backend1.example.com weight=3;  # Backend server with weight
          server backend2.example.com;
          server backend3.example.com;
      }

      server {
          listen 80;
          server_name loadbalancer.example.com;

          location / {
              proxy_pass http://backend;  # Forward requests to the upstream backend
          }
      }
  }
  ```

## 4. Security and Access Control

### Restricting Access
- **Use `allow` and `deny` to control access based on IP addresses.**
  ```nginx
  location /admin/ {
      allow 192.168.1.0/24;  # Allow local subnet access
      deny all;              # Deny all other access
  }
  ```

### Rate Limiting
- **Control the rate of requests using `limit_req_zone` and `limit_req`.**
  ```nginx
  http {
      limit_req_zone $binary_remote_addr zone=one:10m rate=1r/s;  # Define rate limit zone

      server {
          listen 80;
          server_name rate.example.com;

          location / {
              limit_req zone=one burst=5;  # Apply rate limiting with burst
              proxy_pass http://localhost:8080;  # Forward requests to backend
          }
      }
  }
  ```

## 5. Log Configuration

### Access and Error Logs
- **Customize log formats and specify log file locations.**
  ```nginx
  http {
      log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

      access_log /var/log/nginx/access.log main;  # Access log with custom format
      error_log /var/log/nginx/error.log warn;    # Error log with warning level

      server {
          listen 80;
          server_name log.example.com;

          location / {
              root /var/www/html;         # Set document root
              index index.html;           # Default index file
          }
      }
  }
  ```

## 6. Performance Tuning

### Buffering and Timeouts
- **Adjust buffer sizes and timeout settings for optimal performance.**
  ```nginx
  http {
      client_body_buffer_size 128k;      # Buffer size for client request body
      client_max_body_size 1m;           # Maximum allowed size of client request body
      send_timeout 30s;                  # Timeout for sending response to client
      keepalive_timeout 65s;             # Keep connection alive timeout
      sendfile on;                       # Enable sendfile for efficient file transfers
      tcp_nopush on;                     # Optimize sendfile for large files
      tcp_nodelay on;                    # Disable Nagle’s algorithm for low latency

      server {
          listen 80;
          server_name performance.example.com;

          location / {
              root /var/www/html;         # Set document root
              index index.html;           # Default index file
          }
      }
  }
  ```

### Worker Processes and Connections
- **Set the number of worker processes and maximum connections per worker.**
  ```nginx
  worker_processes auto;  # Automatically adjust to the number of CPU cores
  events {
      worker_connections 1024;  # Maximum number of simultaneous connections per worker
  }


  ```

## 7. Advanced Configuration

### Using Includes for Modular Configurations
- **Organize complex configurations by including external files.**
  ```nginx
  http {
      include /etc/nginx/conf.d/*.conf;  # Include all files in conf.d directory
      include /etc/nginx/sites-enabled/*;  # Include all enabled site configurations
  }
  ```

### Scripting with Lua (Optional)
- **Extend Nginx capabilities with custom Lua scripts.**
  ```nginx
  http {
      server {
          listen 80;
          server_name lua.example.com;

          location /lua {
              content_by_lua_block {
                  ngx.say("Hello, Lua!");  # Lua script to respond with "Hello, Lua!"
              }
          }
      }
  }
  ```

## 8. Best Practices for Configuration

- **Keep it Modular:** Use `include` directives to break configurations into manageable pieces.
- **Security:** Always prioritize secure configurations, especially with SSL/TLS and access controls.
- **Testing and Validation:** Always test configurations with `nginx -t` before reloading.
- **Documentation:** Comment and document your configurations for clarity and maintenance.

## Useful Commands

- **Test Configuration Syntax:**
  ```bash
  sudo nginx -t
  ```
- **Reload Configuration Without Downtime:**
  ```bash
  sudo systemctl reload nginx
  ```

## Conclusion

Understanding and mastering the Nginx configuration syntax and directives is key to efficiently managing web servers and services with Nginx. Use this guide to get started and delve deeper into each aspect of Nginx configuration.

---
