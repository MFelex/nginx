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
