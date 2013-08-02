When writing unit tests with Intern, occasionally you will need to interact with a Web service using XMLHttpRequest. However, because the test runner serves code `localhost:9000` by default, any cross-origin requests will fail. In order to test Ajax requests without using CORS or JSONP, the solution is to set up a reverse proxy to Intern and tell the test runner to load from that URL instead. You can either set up the Web server to only send requests to Intern for your JavaScript files, or you can set up the Web server to send all requests to Intern except for the Web services you’re trying to access.

---

## Option 1: All traffic except Web services to Intern

1. Modify the `proxyUrl` in your Intern configuration to point to the URL where the Web server lives
2. Set up the Web server to reverse proxy to `http://localhost:9000/` by default
3. Add location directives to pass the more specific Web service URLs to the Web service instead

An nginx configuration implementing this pattern might look like this:

```nginx
server {
  server_name proxy.example;

  location /web-service/ {
    proxy_pass http://www.web-service.example;
  }

  location / {
    proxy_pass http://localhost:9000;
  }
}
```

## Option 2: Only JavaScript traffic to Intern

1. Modify the `proxyUrl` in your Intern configuration to point to the URL where the Web server lives
2. Set up the Web server to reverse proxy to `http://localhost:9000/` for the special `/__intern/` location, plus any directories that contain JavaScript

An nginx configuration implementing this pattern might look like this:

```nginx
server {
  server_name proxy.example;
  root /var/www;

  location /js/ {
    proxy_pass http://localhost:9000;
  }

  location /__intern/ {
    proxy_pass http://localhost:9000;
  }

  location / {
    try_files $uri $uri/ =404;
  }
}
```

Reverse proxy configuration information for common Web servers:

* [nginx](http://wiki.nginx.org/HttpProxyModule)
* [Apache 2](https://httpd.apache.org/docs/2.2/mod/mod_proxy.html)
* [IIS](http://www.iis.net/learn/extensions/url-rewrite-module/reverse-proxy-with-url-rewrite-v2-and-application-request-routing)