# Twitter timeline cache workloads 
## Timeline Cache
One of the biggest cache use case at Twitter is storing timelines for low-latency serving, the types of timelines include (but not limited to) 
* Home timeline 
* User homepage timeline 
* Conversation timeline 
* others 

These timelines are cached using an internal fork of Redis with customized list implementations. Some example operations include `push (append)`, `range(read a sub list)`, `trim (reduce list size)`. 

The traces in this open-source collection cover the most two common timelines usages at Twitter, fanout timeline and non-fanout timeline. 

Fanout timeline is a write-heavy timeline cache, where each object is a list storing the Tweets (id) published by the people you follow. Every time, someone you follow publishes a Tweet, it gets inserted into this list. Meanwhile, when you open your homepage, your timeline list is read to render the honempage for you. Because the people you follow publishes much faster than the times you open Twitter, this workload is write-heavy. 


Non-fanout timeline such as user timeline which stores the Tweets published by a user, this more likely to be a read-heavy workload. 


## Trace description 
Each trace has three files, 
a `xx.keys` file, which stores all the pkeys in Redis before trace collection; 
a `xx.keylen` file, which stores the length of the list for each pkey in keys file;
a `xx.log.zst` file, which stores all the requests, with the following format, 

```
timestamp  database  operation  pkey  pkey_len  operation_specific_logging 
```

Note that pkey_len is the length of key, not the length of the list. Operations include 
`RPUSHXTRIMEX`, `RPUSHTRIMEX`, `LMREM`, `LRANGE`, `LRANGEEX`, `LRESET`, `DEL`, here is how to decrypt the operation names.  


* R: right/end of the list 
* PUSH: push to the end of the list
* TRIM: trim from left 
* EX: has expiration 
* X: exists (do the operation only if the primary key is cached)
* REM: remove entries in the list
* RANGE: get a range of data
* DEL: remove pkey
* RESET: remove all entries in the list 


Take `RPUSHXTRIMEX` as an example, it pushes to the right end of the list, trims to len X, and only operates if the pkey is cached. The operation specific logging is "list_cap, ttl, size_of_pushed_elements (in bytes)". 
Similarly, the operation specific logging for `LMREM` is "count, element"; for `LRANGE` and `LRANGEEX` is "start, stop, ttl, incr, max_ttl"


## Trace download 
Currently only the first set of traces are available, you can download from 
https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/.redis/.fanout.buz-04-31892.keylen
https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/.redis/.fanout.buz-04-31892.keys
https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/.redis/.fanout.buz-04-31892.log.zst

Note that log files are anonymized and compressed with zstd. 


