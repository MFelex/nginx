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
    root /var/mysite;
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
    root /var/mysite;
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
    
    
  }
}
  
events {}
```
