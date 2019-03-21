---
layout: post
title: How I would improve the Binance API
date: '2019-02-28T13:00:00+00:00'
tags:
- crytpocurrency
- apis
- software
---
The [Binance API](https://github.com/binance-exchange/binance-official-api-docs) so far seems to be better than most.

### Server time skew
**Issue:** I have found the server time 40 - 50 seconds ahead of my servers times which are calibrated using the NTP protcol. This wouldn't usually matter however it is required as a parameter when calling some endpoints. Therefore a client needs to account for the difference in time between them and the Binance server. This adds extra complexity.

**Solution:** Make your server times more accurate.

### Pedantic signing of request
**Issue:** In order to sign [deposit/withdrawl requests](https://github.com/binance-exchange/binance-official-api-docs/blob/master/wapi-api.md#signed-trade-and-user_data-endpoint-security) the _signature_ needs to come at the end of the URL. This is inconsistent with the other REST API endpoints and completely unnecessary. These types of subtlties, even though documented, do not help with developing client APIs. 

**Solution:** Make query param order unnecessary or at least consistent with different endpoints.
