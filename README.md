# htaccess-forwarding

HTAccess for Traffic Forwarding. In simple terms, this `.htaccess` file is the easiest way to forward/mask URLs. For instance, let's say you have an API endpoint that you don't want others to know about, and you want to mask that endpoint (using a different name), this can be achieved using this method:

```apache
RewriteEngine On

# Validate if users access subdomain.domain.tld without using parameters
RewriteCond %{HTTP_HOST} ^subdomain\.domain\.tld$ [NC] # Replace 'subdomain', 'domain', and 'tld' with the domain you want to display to the user
RewriteCond %{QUERY_STRING} !^.
RewriteRule ^ - [F]

# Redirect to jogja.wablas.com if users use parameters
RewriteCond %{HTTP_HOST} ^subdomain\.domain\.tld$ [NC] # Replace 'subdomain', 'domain', and 'tld' with the domain you want to display to the user
RewriteRule ^(.*)$ https://subdomain-original.domain.tld/$1 [P] # Replace 'subdomain-original.domain.tld' with your actual domain to redirect from your masked subdomain to the actual domain, so that the provided parameters will be forwarded

ProxyPassReverse / https://subdomain-original.domain.tld/ # Also, replace this section
