---
title: AWS Cloudfront
description: Amazon Content Delivery network
dateCreated: 2022-05-16T14:22:22.830Z
published: true
editor: markdown
tags: networking, amazon
dateModified: 
---
# AWS CloudFront
>
> CloudFront has servers located in Europe (United Kingdom, Ireland, The Netherlands, Germany, Spain), Asia (Hong Kong, Singapore, Japan, Taiwan and India), Australia, South America, Africa, and several major cities in the United States. In July 2020, the service operated from 205 edge locations on six continents.CloudFront operates on a pay-as-you-go basis.
>
> CloudFront competes with larger CDNs, such as Akamai and Limelight Networks. Upon launch, Larry Dignan of ZDNet News stated that CloudFront could cause price and margin reductions for competing CDNs.
>
> [Wikipedia](https://en.wikipedia.org/wiki/Amazon%20CloudFront)

Global content delivery network - Amazon **CloudFront** is a content delivery network (CDN) operated by Amazon Web Services. Content delivery networks provide a globally-distributed network of proxy servers that cache content, such as web videos or other bulky media, more locally to consumers, thus improving access speed for downloading the content. 

**Should have been called** - Amazon CDN

**Use this to**  - Make your websites load faster by spreading out static file delivery to be closer to where your users are.

**It's like**  - MaxCDN, Akamai

Amazon CloudFront can be used to cache static content from an S3 Bucket. This helps improve performance for the delivery of static content to the end user. Adding an auto-scaling group to the solution will enable the web servers to scale based on demand

- Leverages edge locations around the world to provide cached content to edge locations