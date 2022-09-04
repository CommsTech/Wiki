# [Security Onion](https://securityonionsolutions.com/)



## Link Security Onion to Alienvault
# [AlienVault-OTX](https://docs.securityonion.net/en/2.3/alienvault-otx.html#alienvault-otx "Permalink to this headline")

We can easily pull in [Alienvault OTX](https://otx.alienvault.com/) pulses into Security Onion and have [Zeek](https://docs.securityonion.net/en/2.3/zeek.html#zeek) utilize them for the [Intel Framework](https://docs.zeek.org/en/master/frameworks/intel.html) by leveraging [Stephen Hosom](https://github.com/hosom/bro-otx)’s work with Alienvault OTX integration.

**Warning**
 - Please keep in mind SecurityOnion does not officially support the use of this script, so installation is at your own risk. Also note that some users have reported issues with the OTX feeds causing [Zeek](https://docs.securityonion.net/en/2.3/zeek.html#zeek) to crash.

## [Installation](https://docs.securityonion.net/en/2.3/alienvault-otx.html#installation "Permalink to this headline")

In order to begin, we will need to make sure we satisfy a few prerequisites:

**Alienvault OTX API key** - can be obtained for free at: [https://otx.alienvault.com](https://otx.alienvault.com/)

**Security Onion standalone/sensor** (running [Zeek](https://docs.securityonion.net/en/2.3/zeek.html#zeek))

**External internet access** - to retrieve updated pulses ([https://otx.alienvault.com/api/v1/pulses/subscribed](https://otx.alienvault.com/api/v1/pulses/subscribed))

Download the installation script:

`wget https://raw.githubusercontent.com/weslambert/securityonion-otx/master/securityonion-otx`

![[Pasted image 20220904093335.png]]
Run the script:

`sudo bash securityonion-otx`
**Note** You will need your OTX API code for this step

![[Pasted image 20220904093735.png]]
![[Pasted image 20220904093755.png]]
I recieved an error for Zeek not starting. I did a restart and It came right up. The error message indicated to check `/nsm/zeek/logs/current/reporter.log` for clues

I did this by `cat /nsm/zeek/logs/current/reporter.log`


After using the above script, `/opt/so/conf/zeek/policy/custom/zeek-otx/` will contain all necessary files including `otx.dat` where all pulses will be populated.

We can test our configuration by adding another piece of intel to the end of `/opt/so/conf/zeek/policy/custom/zeek-otx/otx.dat`. For example:

google.com[literal tab]Intel::DOMAIN[literal tab]Test-Google-Intel[literal tab]https://google.com[literal tab]T

As long as our syntax is correct, we should not need to restart Zeek. We can check for errors in `/nsm/zeek/logs/current/reporter.log`.

Let’s see if we can get an intel hit by doing the following:

`curl google.com`

Next, we need to check `/nsm/zeek/logs/current/intel.log` for entries relating to our indicator:

`grep google /nsm/zeek/logs/current/intel.log`

We should have received a Zeek Notice as well, so let’s check that:

`grep google /nsm/zeek/logs/current/notice.log`

After successful testing, we can remove our addition from `/opt/so/conf/zeek/policy/custom/zeek-otx/otx.dat` or just run `sudo python3 /usr/sbin/zeek-otx.py` again.

By default, pulses will be retrieved on an hourly basis. To change this to a different value, simply alter the interval in `/etc/cron.d/zeek-otx`.