AWS proxy module
================

Implements proxying of authenticated requests to S3.

```nginx
  server {
    listen     8000;

    location /fw-pub-pkgs {
      proxy_pass https://s3.amazonaws.com; #also works with HTTP

      inbound_aws_access_key 01234567890123456789;
      inbound_aws_secret_key 0123456789012345678901234567890123456789;

      aws_access_key INSERT_ACCESSKEY_HERE;
      aws_secret_key INSERT_SECRETKEY_HERE;
      #To use IAM Role/STS, insert the token and uncomment x-amz-security-token header below
      #aws_security_token 'INSERT_TOKEN_HERE';

      proxy_set_header Authorization $s3_auth_token;
      proxy_set_header x-amz-date $aws_date;
      #proxy_set_header x-amz-security-token $aws_token;

  }

  }
```

Request signing & Amazon Cloudfront Service
-------------------------------------------


If Nginx is behind Amazon's CloudFront CDN service, you need to add this setting : 

```nginx
proxy_set_header x-amz-cf-id "";
```

into nginx.conf in order to clear X-Amz-Cf-Id header before signing the request to Amazon S3 bucket.


More info here : 

http://s3.amazonaws.com/doc/s3-developer-guide/RESTAuthentication.html


Credits:
========
Based on http://nginx.org/pipermail/nginx/2010-February/018583.html and suggestion of moving to variables rather than patching the proxy module.

License
-------
This project uses the same license as ngnix does i.e. the 2 clause BSD / simplified BSD / FreeBSD license
