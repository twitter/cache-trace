## Anonymized Cache Request Traces from Twitter Production

### Trace Overview
This repository describes the traces from Twitter's in-memory caching ([Twemcache](https://github.com/twitter/twemcache)/[Pelikan](https://github.com/twitter/pelikan)) clusters. The current traces were collected from one instance of 54 clusters in Mar 2020. The traces are one-week-long. 
More details are described in the following paper and blog. 
* [Juncheng Yang, Yao Yue, Rashmi Vinayak, A large scale analysis of hundreds of in-memory cache clusters at Twitter. _14th USENIX Symposium on Operating Systems Design and Implementation (OSDI 20)_, 2020](https://www.usenix.org/conference/osdi20/presentation/yang). 
* blog post 


### Trace Format 
The traces are compressed with [zstd](https://github.com/facebook/zstd), to decompress run `zstd -d /path/file`. 
The decompressed traces are plain text structured as comma-separated columns. Each row represents one request in the following format.


  * `timestamp`: the time when the cache receives the request, in sec 
  * `anonymized key`: the original key with anonymization 
  * `key size`: the size of key in bytes 
  * `value size`: the size of value in bytes 
  * `client id`: the anonymized clients (frontend service) who sends the request
  * `operation`: one of get/gets/set/add/replace/cas/append/prepend/delete/incr/decr 
  * `TTL`: the time-to-live (TTL) of the object set by the client, it is 0 when the request is not a write request.  


Note that during key anonymization, we preserve the namespaces, for example, if the anonymized key is `nz:u:eeW511W3dcH3de3d15ec`, the first two fields `nz` and `u` are namespaces, note that the namespaces are not necessarily delimited by `:`, different workloads use different delimiters with different number of namespaces. 

A sample of the traces are attached under samples. 


### Trace Download 
The full traces are large (3.2 TB in compressed form, 14 TB uncompressed), and can be downloaded from the following places. 

  * Carnegie Mellon University PDL lab cluster: https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/open_source
  * SNIA 
  * Storj 
  * Baidu pan 

These traces are splitted into smaller files of 1000000000 lines (100000000 for SNIA) each and compressed with zstd, so a file with name X_cache.0.zst means this file contains the first 1000000000 requests of X_cache cluster trace. 

### Choice of traces for different evaluations 
miss ratio related (admission, eviction)

write-heavy workloads 


TTL-related 
  mix of small and large TTLs 


  small TTLs only 


Object sizes 
  Small objects 


  Large objects 


### Misc 
  * Please join our [discussion channel](http://groups.google.com/group/?) for questions and updates. 
  * We provide a **[trace bibliography](bibliography.bib)** of papers that have used and/or analyzed the traces, and encourage anybody who publishes one to add it to the bibliography by creating an issue or pull request on GitHub. 


### Acknowledgement 
  We thank SNIA and Carnegie Mellon University PDL lab for hosting the traces. 


### License
![Creative Commons CC-BY license](https://i.creativecommons.org/l/by/4.0/88x31.png)
The data and trace documentation are made available under the
[CC-BY](https://creativecommons.org/licenses/by/4.0/) license.
By downloading it or using them, you agree to the terms of this license.


