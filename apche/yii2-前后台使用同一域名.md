```
RewriteEngine On

# End the processing, if a rewrite already occurred

RewriteRule ^(frontend|admin)/web/ - [L]

# Handle the case of backend, skip ([S=1]) the following rule, if current matched

RewriteRule ^admin(/(.*))?$ backend/web/$2 [S=1]

# handle the case of frontend

RewriteRule .* frontend/web/$0


# Uncomment the following, if you want speaking URL

#RewriteCond %{REQUEST_FILENAME} !-f

#RewriteCond %{REQUEST_FILENAME} !-d

#RewriteRule ^([^/]+/web)/.*$ $1/index.php
```
