﻿<a name="br1"></a>Project Wireless Communications

Kotsiaridis Konstantinos 2547

September 2021

1 Project’s Purpose

The purpose of this project was to search the code of the ath9k driver in order

2 Implementation Overview

In order to change the distribution of the channel, we manipulated the priority

1




<a name="br2"></a>2




<a name="br3"></a> The ﬁles in which we added functionality are the below:

AP -> drivers/net/wireless/ath/ath9k/main.c

->drivers/net/wireless/ath/ath9k/mac.h

To test the above functionality we experiment using the iperf tool.

3 Steps to Final Implementation

For the AP part, we searched for AIFS parameter and where it can be changed

` `For the COORDINATOR part, we tried to change the beacon packet by

3




<a name="br4"></a>(\_\_ieee80211\_beacon\_get()).

` `The ﬁnal step of our research was to transfer internally the information that

4 Experiments/Scenarios

The ﬁrst scenario we had to implement was a simple channel distribution be-

For the second scenario we had to implement two diﬀerent sub-scenarios:

` `1) The goal of this sub-scenario is to add one more link to the experiment.

4



<a name="br5"></a> 2) The goal of this second scenario is to add INTERFIERER to the list of

5