# Nginx

# What is Nginx

It is an open-source web server written in c that could be used as a reverse proxy and load balancer.

- Web Server
    - Serves web content
- Proxy
    - Load balancer
    - Reverse proxy
    - Cache

In layer 4 you have access to IP addresses and ports. On the other hand, in layer 7 you have access to headers, body, GET, or POST requests (if it is HTTP protocol). 

```bash
# Installation
sudo apt-get install nginx
```

# Config A Simple Web Server

The config file is in `/etc/nginx/nginx.conf`

Simple configuration:

```bash
http {
	
	server{
		listen 8080;
		root /home/amin/static_site/;
	}

}

events { }
```

to serve some kind of data in a completely different location:

```bash
http {
	
	server{
		listen 8080;
		root /home/amin/static_site/;
	}

	location /images {
		root /home/amin/Pictures/;
	}

}

events { }
```

to return some special pages and values or to determine some access:

```bash
http {
	
	server{
		listen 8080;
		root /home/amin/static_site/;
	
		# Note that you should have an image
		# directory in path /home/amin/Pictures
		# otherwise you will get 404.
		location /images {
			root /home/amin/Pictures/;
		}
	
		location ~ .jpg$ {
			return 403;
		}
	
		# You can use Regular Expression here 
		# to define lots of rules.

	}

}

events { }
```

to redirect and proxy pass:

```bash
http {
	
	server{
		listen 8080;
		root /home/amin/static_site/;
	
		# Note that you should have an image
		# directory in path /home/amin/Pictures
		# otherwise you will get 404.
		location /images {
			root /home/amin/Pictures/;
		}
	
		location ~ .jpg$ {
			return 403;
		}
	
		# You can use Regular Expression here 
		# to define lots of rules.
		# ...
		
	}

	# We can have as many server context as
	# we want to have.

	server {
		listen 8888;

		location / {
			return 400;
		}

		location /img {
			proxy_pass http://localhost:8080/images/;
		}

	}

}

events { }
```

to start, reload, or stop Nginx:

```bash
# To start nginx 
$ sudo nginx

# To stop nginx
$ sudo nginx -s stop

# To reload nginx
$ sudo nginx -s reload

# Restart the whole nginx system
$ sudo systemctl restart nginx
```

# Location Directive

The location directive within NGINX server block allows to route request to correct location within the file system.

The directive is used to tell NGINX where to look for a resource by including files and folders while matching a location block against an URL. In this tutorial, we will look at NGINX location directives in details.

```bash
location [modifier] [URI] {
	  ..
	  ..
}
```

The modifier in the location block is optional. Having a modifier in the location block will allow NGINX to treat a URL differently. Few most common modifiers are:

- **none**: If no modifiers are present in a location block then the requested URI will be matched against the beginning of the requested URI.
- **=**: The equal sign is used to match a location block exactly against a requested URI.
- **~**: The tilde sign is used for case-sensitive regular expression match against a requested URI.
- **~***: The tilde followed by asterisk sign is used for case insensitive regular expression match against a requested URI.
- **^~**: The carat followed by tilde sign is used to perform longest nonregular expression match against the requested URI. If the requested URI hits such a location block, no further matching will takes place.

A location can be defined by using a prefix string or by using a regular expression.

NGINX will run through the following steps to select a location block against a requested URI.

- NGINX starts with looking for an exact match specified with `location = /some/path/` and if a match is found then this block is served right away.
- If there are no such exact location blocks then NGINX proceed with matching longest non-exact prefixes and if a match is found where ^~ modifier have been used then NGINX will stop searching further and this location block is selected to serve the request.
- If the matched longest prefix location does not contain ^~ modifier then the match is stored temporarily and proceed with following steps.
    - NGINX now shifts the search to the location block containing ~ and ~* modifier and selects the first location block that matches the request URI and is immediately selected to serve the request.
    - If no locations are found in the above step that can be matched against the requested URI then the previously stored prefix location is used to serve the request.

## 1. NGINX location matching all requests

In the following example the prefix location / will match all requests but will be used as a last resort if no matches are found.

```bash
location / {
    ..
}
```

## 2. NGINX location matching exact URL

NGINX always tries to match most specific prefix location at first. Therefore, the equal sign in the following location block forces an exact match with the path requested and then stops searching for any more matches.

```bash
location = /images {
    ..
}
```

The above location block will match with the URL `https://domain.com/images` but the URL `https://domain.com/images/index.html` or `https://domain.com/images/` will not be matched.

## 3. NGINX location block for a directory

The following location block will match any request starting with /images/ but continue with searching for more specific block for the requested URI. Therefore the location block will be selected if NGINX does not find any more specific match.

```bash
location /images/ {
     ..
     ..
}
```

## 4. NGINX location Regular Expression Example

The modifier **^~** in the following location block results in a case sensitive regular expression match. Therefore the URI /images or /images/logo.png will be matched but stops searching as soon as a match is found.

```bash
location ^~ /images {
   ..
   ..
}
```

## 5. NGINX location block for image/css/js file types

The modifier **~*** in the next location block matches any request (case-insensitive) ending with png, ico, gif, jpg, jpeg, css or js. However, any requests to the `/images/` folder will be served by the previous location block.

```bash
location ~* .(png|ico|gif|jpg|jpeg|css|js)$ {
    ..
    ..
}
```

## 6. NGINX location Regular Expression Case Sensitive Match

The modifier **~** in the following location block results in a case sensitive regular expression match but doesn’t stop searching for a better match.

```bash
location ~ /images {
    ..
    ..
}
```

## 7. NGINX location Regular Expression Case Insensitive Match Example

The modifier **~*** in the following location block results in a case insensitive regular expression match but the searching doesn’t stop here for a better match.

```bash
location ~* /images {
     ..
     ..
}
```

## Summary

Understanding NGINX location directive is essential for tracing end points of requested URI in the file system. 

# Resources

> Last update: Until 00:34:00
> 

[2 Hours NginX Crash Course + Bonus Content](https://www.youtube.com/watch?v=hcw-NjOh8r0)

[Nginx location directive examples - JournalDev](https://www.journaldev.com/26342/nginx-location-directive)