---
title: Crypto Mining
description: 
published: true
date: 2022-07-25T00:03:04.585Z
tags: Mining, Crypto
editor: markdown
dateCreated: 2022-07-25T00:03:04.585Z
---

Here, we will explain how mining works, how NiceHash works, how do you get paid and how can you monitor whether your mining system is fully optimized and working with complete reliability and most importantly, how to read NiceHash charts. It will also quickly teach you how to evaluate other miners and compare them to NiceHash QuickMiner.

Cryptocurrency mining on NiceHash (very simple version)
Cryptocurrency mining is nothing else but performing calculations of hash values - it is an one way mathematical function which calculates output Xa out of input Ia. Each input Ia also has a special number appended, which consist of two parts: Ns (nonce-server) and Nm (nonce-miner). Your miner gets job from NiceHash server which contains Ia and Ns. Your miner chooses (usually just doing iteration ++1) Nm for each calculation and then calculates output according to the following formula:

Xa = HASH(Ia + Ns + Nm)

Xa is a very big number. If this number is smaller than Ta (target - also provided in job), then your miner finds a share and corresponding Nm is sent to NiceHash as proof-of-work. This proves that your miner actually performed work to find appropriate nonce. Because HASH is an one way function, there is no other possibility for miner but to try many nonces. Mining with 60 MH/s means that your miner is trying 60 million of nonces per second and making 60 million HASH calculations per second.

Shares
When your miner is lucky to find appropriate nonce Nm, it packs it together with ID of the job and sends to the NiceHash server as share. NiceHash can then:

accept your share as valid,
reject your share as invalid - wrong calculation or
reject your share as stale - it came too late.
Accepted shares
Your found nonce is correct (there was no mistake in calculation) and the share arrived to the server on time (it was not too late). Usually you can notice accepted shares when miner console window informs you, for example:

net | daggerhashimoto | Share #114 accepted (31 ms)

You get paid some fixed amount of BTC for this share. The amount of BTC the share is worth depends on two factors:

how hard the work was and
how much buyers are paying currently.
We will explain only the first one, because second one is part of the NiceHash confidential proprietary algorithm. The hardness of work is defined by Ta which is calculated out of (1 * algo_const) / Da. Da is difficulty. The higher the difficulty, the smaller the number Ta and for miner it is harder (takes longer) to guess appropriate Xa which would be smaller than Ta. In fact, if we increase difficulty by factor 2, the number Ta becomes smaller by factor 2 and your miner consequently finds only half as many shares as before. Each job has certain Da attached (defined by the NiceHash server). When your miner finds share for job that has Da increased by x2, then this share also has hardness factor x2 and is worth twice as much as share found for job with Da x1.

NiceHash adjusts Da (difficulty) dynamically according to your miner's speed:

If your miner is faster and is submitting shares more frequently than expected, NiceHash increases difficulty.
If your miner is slower and is submitting shares rarely, then Nicehash decreases difficulty.
Therefore it does not matter how many shares your miner finds, but rather how many weighted shares your miner finds. This value can be best represented with a two dimensional graph of accepted speed. You can see this chart if you click on All Algorithms and then choose Daggerhashimoto as shown in the following picture: Chart - get speed

We have 6 rigs with following miner speeds: 587 MH/s, 409 MH/s, 261 MH/s, 215 MH/s, 214 MH/s and 121 MH/s. The total speed of all rigs is 1807 MH/s.

When we check average speed over longer period of time (at least several hours for multiple/big rigs and several days for small/single rigs), the speed has to match to our total rig speed as visible on the chart below. The total accepted speed on the chart is lower, because we need to account in 1.14 % reject rate. At the speed of ~1800 MH/s, that is approx. 20 MH/s. When we deduct 20 MH/s from our total rig speed of 1807 MH/s, we get 1787 MH/s. Our chart is showing us 1791 MH/s which is 4 MH/s above - this can be attributed to better luck during that period of time. It also tells us that chosen miner is reporting hashing speed honestly and correctly.

Chart - accepted speed

IMPORTANT! Accepted speed on NiceHash is the most important chart. It tells you how your miner is actually performing. If your chart is showing greatly depressed numbers compared to what your miner is displaying in console, then you are being cheated out! It is a well known fact that some devfee miners artificially inflate console-speed numbers to attract more customers and in most cases they do not deduct devfee speed. So you may get impression that your miner is performing great with high speed, but on NiceHash, your accepted speed chart shows different story. It does not matter what speed your miner reports, but what accepted speed chart on NiceHash says - you are paid directly according to that!

NiceHash Rig Manager displays:

local profitability and
actual profitability.
Local profitability is speed reported from your miners multiplied with current profitability. It is more stable, but it can be wrong if miner is artificially inflating speed numbers. Actual profitability is calculated by multiplying accepted speed with current profitability. That is why it is called actual - this is what you are actually getting paid.

Rejected shares - share above target
When you get this type of rejected share, it means that your miner provided wrong or invalid calculation and as a consequence wrong result. Usually, this happens if you overclock VRAM too much - memory is not stable anymore and errors happen. Naturally, you are not paid for rejected shares of any type. You should not have many of these type of rejects (perhaps only one or two shares here and there). In Rig Manager, you can filter out all other shares and view only average percentage of rejected shares of type target.

In our example, we had only few rejected shares of type target, so the chart has only two spikes and average percentage is so low that, when rounding it to two decimals, it shows 0.00%. This tells us our rigs are properly tweaked and tuned not to produce too many rejects of type share above target.

Chart - rejected speed - target

Rejected shares - job not found (stale)
This type of reject is unavoidable. It depends on many factors, including your network latency to chosen NiceHash server. That's why it is important to choose the one with the lowest latency as instructed here. It can also depend on your chosen miner software. A miner software that is sending old shares for jobs that are not valid anymore, will generate you stale shares. It is important for miner to quickly switch to and start working on the new job when old jobs are stale. Excavator performs this task in approx 1-2 milliseconds when using modern CPUs.

Our stale share ratio is 0.97%. This means that we are losing about 1% of possible income. With approx. 1800 MH/s of total rig speed, this means we are wasting (or throwing away) approx. 18 MH/s of it.

Chart - accepted speed - stale

But, why do jobs become stale anyway? Your miner is performing work for a blockchain - there is a new block every few minutes or seconds. When that happens, previous jobs become stale and cannot be used anymore. In Excavator, when job has suffix (clean) all previous jobs are stale.

And what is the point of shares? A share with extremely high difficulty which is above network difficulty of blockchain is the solution that creates new block on the blockchain.