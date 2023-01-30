# NGINX

NGINX is a HTTP server that excels at handling concurrent requests, and serves files with a low overhead.

When installed, it will typically be located in the `/etc/nginx/` directory. Configuration is managed in the `nginx.conf` file (although you should generally be editing config files that are imported into the main config file rather than directly editing it). When editing the configuration you can run `nginx -t` to check for syntactical errors. You can reload the server with your new configuration using `systemctl restart nginx` or `nginx -s reload`. The `-s` flag is for signals, which also include `stop`, `quit`, and `reopen`.

Nginx's configuration consists of simple and block directives. Simple directives are single line configurations that are terminated by a semi-colon. Block directives contain multiple related directives that are contained within curly brackets. The four block directives for an NGINX config are events, http, server, and main.

## Main Block

The main block includes information like the `user` (should be `www-data`), the number of `worker_processes`, the `pid` number, as well as `include` directives.

## HTTP Block

HTTP blocks contain the values for managing content served via HTTP. You can specify the valid file types served with the `include` directive which will point to `/etc/nginx/mime.types`. If you need to add additional files you can do so from that file.

## Server Blocks

Multiple server blocks can be configured in a config file. Server blocks will include `listen` directives that specify the port to listen on, the `server_name`, and a `return` directive that responds with a HTTP status code and the response that goes along with it. To serve static content, you can place files in the `/srv/` directory, and point to the root directory with the content using the `root` directive.

### Location Directives

The `location` directive can be used for prefix matches. `location /cool { <return directive> }` will match any route that begins with cool. So `url.com/cool_sweater` will serve the same content as `url.com/cool`. For an exact match you would use `location = /cool { <return directive> }`. You can also create regex matches by using the `~` symbol followed by standard regex expressions. Note that regex expressions are typically case sensitive, but you can disable case-sensitivity by appending an `*` after the `~` symbol. Sometimes these different location directives can end up overlapping, and so NGINX will have to prioritize certain matches over others. To give a location directive precedence, you can prepend the `^` symbol before the `~` symbol.

### Variables

Variables can be declared in the server block with the syntax `set $var value;` and they can be assigned in the return value with the syntax `$var`. You can also include variables that are arguments from the url path, by assigning the value as $arg_var where the variable is set from the path `my_url.com/?var=hello_world`.

### Redirects and Rewrites

Redirects are handled with the return directive, the appropriate status code, and the route you are redirecting to. When the user types in the route that brings up the redirect, the page will update to the provided redirect route. By contrast, a `rewrite` directive has the arguments of the route provided by the client, and the content you serve instead, but it appears to the client as if this is the content for the URL they visited. In most cases, it is better to use a redirect.

## Logs

Logs are usually located in `/var/log/nginx/`. Logging activity can be managed from within the `server` block directive including where the logs should live, what is logged, and what the warning level for logs should be.

## Reverse Proxy Configuration

When NGINX is a reverse proxy it acts as a middleman between the client and the server. At the most basic level, you can configure the proxy in the `location` directive by using the `proxy_pass` directive where you would normally provide the `return` directive, with the IP addr/URL (and optionally the port number) that NGINX is proxying to. Other directives you can provide include `proxy_http_version`, and `proxy_set_header`.

One very useful function of a reverse proxy is it can act as a load balancer for your web application. So you can run multiple instances of your app on different ports, and include those instances with the `upstream` directive.
