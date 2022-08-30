# AWS CloudFront
Global content delivery network

**Should have been called** - Amazon CDN
**Use this to**  - Make your websites load faster by spreading out static file delivery to be closer to where your users are.
**It's like**  - MaxCDN, Akamai

Amazon CloudFront can be used to cache static content from an S3 Bucket. This helps improve performance for the delivery of static content to the end user. Adding an auto-scaling group to the solution will enable the web servers to scale based on demand

- Leverages edge locations around the world to provide cached content to edge locaions