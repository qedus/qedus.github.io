---
layout: post
title: How I would improve the Bittrex API
date: '2019-02-27T11:00:00+00:00'
tags:
- crytpocurrency
- apis
- software
---
The [Bittrex API](https://bittrex.github.io/api/v1-1) is not bad at all however there are a few things that I would do different with the benefit of having used it extensively in order to make the developer/user experience better. The issues I have come accross are vagule in order of annoyance with the most annoying feature at the top.

### Don't use SignalR for pushing data to clients
**Issue:** SignalR isn't that well supported outside Microsoft centric environments and there aren't enough robust clients for it in a variety of languages.

**Solution:** Use WebSockets instead with a standardised JSON specification such as the [JSON:API Specification](https://jsonapi.org/format/#document-jsonapi-object).
Or better yet use WebSockets with Google Protobuf or similar to cut down on bandwidth, have strongly described endpoint types and allow code autogeneration.

### Define your JSON objects that are pushed to the client
**Issue:** The JSON response from SignalR can't easily take advantage of statically typed languages because you never know what the shape of the returned object will be.
For example any sort of object can be received from the SignalR endpoint. Below are two examples of objects that can be received from the SignalR stream:
```javascript
// This could be returned.
{
   "R":true,
   "I":"1"
}

// Or this could be returned.
{
   "C":"d-6BAD5V78-E,0|BNFj5,0|BNFj6,2|fK,E89L8",
   "M":[
      {
         "H":"CoreHub",
         "M":"updateExchangeState",
         "A":[
            {
               "MarketName":"BTC-ETH",
               "Nounce":953081,
               "Buys":[
                  {
                     "Type":0,
                     "Rate":0.03522162,
                     "Quantity":73.13170000
                  },
                  {
                     "Type":1,
                     "Rate":0.02853588,
                     "Quantity":0.0
                  }
               ],
               "Sells":[
                  {
                     "Type":1,
                     "Rate":0.03611225,
                     "Quantity":0.0
                  },
                  {
                     "Type":1,
                     "Rate":0.03612664,
                     "Quantity":0.0
                  },
                  {
                     "Type":0,
                     "Rate":0.04180796,
                     "Quantity":0.12164500
                  },
                  {
                     "Type":0,
                     "Rate":0.04183300,
                     "Quantity":0.02682065
                  }
               ],
               "Fills":[

               ]
            }
         ]
      }
   ]
}
```
In order to parse these objects into a statically typed structure you need to first peak into it to ascertain the object that is returned. Only then can you unmarshal the object into the right structure.

**Solution:** If any object can be recieved from a stream then ensure that the object type is described early on in the JSON structure using a defined format. For example:
```javascript
{
   "type":"market",
   "data":{
      "asks":22,
      "bids":33
   }
}
```
This would allow a static language to unmarshal the enclosing object to discover that the enclosed `data` object is of `type` "market".

### Spelling mistakes with endpoint fields
**Issue:** `Nonce` is spelt incorrectly as `Nounce` in several endpoints.

**Solution:** Check your spelling.

### Inconsistent location of endpoint fields within objects
**Issue:** The nonce from the SignalR stream is embedded in different JSON objects at different locations. This makes it non trivial to extract the nonce from each returned object.

**Solution:** Have a standardised place to put the nonce.
