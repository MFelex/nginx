# Nginx

## Serve static HTML
```
nginx.conf

http {
  
  types {
    text/css    css;   # text/html Content-Type uses css extention.
    text/html   html;
  }

  server {
    listen 8080;
    root /var/mysite;  # we put index.html in this directory
  }
}
  
events {}
```

```
$ nginx -s reload 
```

## Include mime type instead of write it one by one 
```
nginx.conf

http {
  
  include mime.types;
  
  server {
    listen 8080;
    root /var/mysite;  # we put index.html in this directory
  }
}
  
events {}
```

## Add more paths
```
nginx.conf

http {
  
  include mime.types;
  
  server {
    listen 8080;
    root /var/mysite;
    
    location /fruits {
      root /var/mysite;  # it appends /fruits to end of /var/mysite automatically
    }
    
    location /carbs {
      alias /var/mysite/fruits;  # in localhost:80/carbs sees /fruits content
    }
     
  }
}
  
events {}
```

## Replace index.html
by default nginx looks for index.html, we put veggies.html instead of index.html and serve it by try_files directive
```
nginx.conf

http {
  
  include mime.types;
  
  server {
    listen 8080;
    root /var/mysite;
    
    location /fruits {
      root /var/mysite;
    }
    
    location /carbs {
      alias /var/mysite/fruits;
    }
    
    location /vegetables {
      root /var/mysite;
      try_files /vegetables/veggies.html /index.html =404;
      # it looks for veggies.html, if it does not found it, server the /var/mysite/index.html and else 404.
    }
    
  }
}
  
events {}
```

## Regural Exoression
```
nginx.conf

http {
  
  include mime.types;
  
  server {
    listen 8080;
    root /var/mysite;
    
    location /fruits {
      root /var/mysite;
    }
    
    location /carbs {
      alias /var/mysite/fruits;
    }
    
    location /vegetables {
      root /var/mysite;
      try_files /vegetables/veggies.html /index.html =404;
      # it looks for veggies.html, if it does not found it, server the /var/mysite/index.html and else 404.
    }
    
    location ~* /count/[0-9] {
      root /var/mysite;
      try_files /index.html =404;
    }
  }
}
  
events {}
```

## Redirect & Rewrite
```
nginx.conf

http {
  
  include mime.types;
  
  server {
    listen 8080;
    root /var/mysite;
    
    location /fruits {
      root /var/mysite;
    }
    
    location /carbs {
      alias /var/mysite/fruits;
    }
    
    location /vegetables {
      root /var/mysite;
      try_files /vegetables/veggies.html /index.html =404;
      # it looks for veggies.html, if it does not found it, server the /var/mysite/index.html and else 404.
    }
    
    rewrite ^/number/(\w+) /count/&1;
    
    location /corps {
      return 307 /fruits;  # Redirect
    }
    
    location ~* /count/[0-9] {
      root /var/mysite;
      try_files /index.html =404;
    }
  }
}
  
events {}
```

## Load Balancer
Run four docker containers on localhost:1111 to :4444 and nginx choose one of them through Round Robin algorithm.
```
nginx.conf

http {
  
  include mime.types;
  
  upstream backendserver {
    server 127.0.0.1:1111
    server 127.0.0.1:2222
    server 127.0.0.1:3333
    server 127.0.0.1:4444
  }
  
  server {
    listen 8080;
    
    location / {
      proxy_pass http://backendserver/;
    }
    
  }
}
  
events {}
```
