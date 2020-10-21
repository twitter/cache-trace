## Anonymized Cache Request Traces from Twitter Production

### Trace Overview
This repository describes the traces from Twitter's in-memory caching ([Twemcache](https://github.com/twitter/twemcache)/[Pelikan](https://github.com/twitter/pelikan)) clusters. The current traces were collected from 54 clusters in Mar 2020. The traces are one-week-long. 
More details are described in the following paper and blog. 
* [Juncheng Yang, Yao Yue, Rashmi Vinayak, A large scale analysis of hundreds of in-memory cache clusters at Twitter. _14th USENIX Symposium on Operating Systems Design and Implementation (OSDI 20)_, 2020](https://www.usenix.org/conference/osdi20/presentation/yang). 
* blog post 

---

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


---

### Trace Download 
The full traces are large (2.8 TB in compressed form, 14 TB uncompressed), and can be downloaded from the following places. 

#### Carnegie Mellon University PDL cluster
https://ftp.pdl.cmu.edu/pub/datasets/twemcacheWorkload/open_source

#### SNIA 

#### Storj 
see [storj](storj) for how to access (Good for worldwide access, especially Asia and Europe, but not available after Dec 2020)
  
#### Baidu pan
https://pan.baidu.com/s/1Jm2nAW-UhsjXU6JYoA07LA access code: wcws (Good for Asia access, but UI only has Chinese)


These traces are splitted into smaller files of 1000000000 lines (smaller for SNIA) each and compressed with zstd, so a file with name clusterN.0.zst means this file contains the first 1000000000 requests of cluster N. 

Feel free to contact us if you have problem downloading the traces. 


---

### Choice of traces for different evaluations 
For different evaluation purposes, we recommend the following clusters/workloads 

* **miss ratio related (admission, eviction)**: cluster52, cluster17 (low miss ratio), cluster18 (low miss ratio), cluster24, cluster44, cluster45, cluster29. 


* **write-heavy workloads**: cluster12, cluster15, cluster31, cluster37. 


* **TTL-related**: mix of small and large TTLs: cluster 52, cluster22, cluster25, cluster11; small TTLs only: cluster18, cluster19, cluster6, cluster7. 


---


### More information about each workload is included under `stat/`
We release a computed statistics of each cluster workload under `stat/`, the latest is [here](stat/2020Mar.md). 
This table includes the following fields, each field is the mean value of the metric either from production or from the traces. 

The fields include `production miss ratio`, 
`workload category` (1: storage, 2: computation, 3: transient item), `key size`, `value size`, `request rate`, `mean object frequency`, `one-hit-wonder ratio (%)`, `compulsory miss ratio (%)`, `common TTLs`, `working set size`, `operations`, `Zipf alpha`. 


---

### Misc 
  * Please join our [discussion channel](http://groups.google.com/group/cache-trace) for questions and updates. 
  * We provide a **[trace bibliography](bibliography.bib)** of papers that have used and/or analyzed the traces, and encourage anybody who publishes one to add it to the bibliography by creating an issue or pull request on GitHub. 


---

### Acknowledgement 
  We thank Carnegie Mellon University PDL lab, SNIA and Storj for hosting the traces. 


### License
![Creative Commons CC-BY license](https://i.creativecommons.org/l/by/4.0/88x31.png)
The data and trace documentation are made available under the
[CC-BY](https://creativecommons.org/licenses/by/4.0/) license.
By downloading it or using them, you agree to the terms of this license.


