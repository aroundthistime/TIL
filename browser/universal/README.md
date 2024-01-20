# Universal information about Web Browsers

### 1. Cache
- Cache can be stored in 2 places of Web Browser:
    - Memory Cache: Stored inside Memory (RAM)
    - Disk Cache: Stored inside the disk
    - The user cannot decide whether to save or read cache from. (Browser decides based on file size, request frequency, ..)
    - Type of the used cache can be checked with Network tab of devtools.

- Server validates whether the cache can be used or not based on E-tag:
    - If the latest E-tag in the server is same as the E-tag inside the cached resource, server returns 304 status acode.
    - If two E-tags are different, server returns latest resource with updated E-tag.

- Cache control should be decided by the characteristic of the resource.
    - HTML: no-cache to keep the latest state (If HTML is not up-to-date, all other resources like Stylesheet, Scripts would possibily out-of-date as well)
    - JS, CSS, static assets: Long cache time (because the files are normally hashed during build time)