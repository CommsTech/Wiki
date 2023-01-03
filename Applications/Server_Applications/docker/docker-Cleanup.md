# [Cleanup Docker](https://christitus.com/cleanup-docker/ "See on original website")

[![✇](https://freshrss.commsnet.org/f.php?e35a1391)Linux on Chris Titus Tech | Tech Content Creator](https://freshrss.commsnet.org/i/?get=f_7 "Filter") 

December 15th 2022 at 7:00 pm

If you are running docker you will accumulate a lot of unused images and volumes as you update running containers. This makes maintenance a necessity.

> Running Docker? Be sure and cleanup unused images and volumes with:  
> docker volume rm $(docker volume ls -f dangling=true -q)  
> docker rmi $(docker images --quiet --filter "dangling=true")
> 
> — Chris Titus Tech (@christitustech) [December 16, 2022](https://twitter.com/christitustech/status/1603766983274729472?ref_src=twsrc%5Etfw)

## Do you need this?

Check your portainer status page and look for “unused” images and containers. If you are seeing a lot of these, then you are wasting space and I’d recommend running this script to clean them up!

## The Cleanup Script

Run this bash script every week to ensure the cleanup gets performed.

```
#!/bin/bash

docker volume rm $(docker volume ls -f dangling=true -q)
docker rmi $(docker images --quiet --filter "dangling=true")
```

## Walkthrough Video