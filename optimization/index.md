# How to optimize performance in Web front-end

### 1. Set image size properly
-  Use image CDN
- Utilize srcset, sizes attribute of img component: standard could be either w (width) or x (resolution)
- Utilize next/Image

### 2. Code splitting
- Split code by routes
- Split code by modules

### 3. Find bottleneck code
- Utilize performance tab of devtools and find task with long runtime.

### 4. Reduce bundle size
- Use bundler features (eg. uglify)
- Use bundle analysis tool (eg. webpack-bundle-analyzer)
- Enable text compression (set Content-Encoding in response header from the server)
(1) gzip: Use deflate internally + extra (higher compression)
(2) deflate

### 5. Preconnect to required origins
- Pre-setup tasks for connecting to important 3rd party origin which could take a lot of time (DNS search, TCP hand-shaking, ..)
- add <link rel="preconnect" href=`${link}` /> 
- For several types of resources (eg. font), crossorigin attribute is required (<link rel="preconnect" href=~ crossorigin >)
- Caution
(1) Preconnect consumes quite a bit of CPU especially with credential communication
(2) Abusing preconnect would cause bandwidth contention. For the less important ones, you could use <link rel="dns-prefetch href=~> to only save time for DNS lookup
(3) If the browser is going to be closed after a short term, preload is ineffective (average standard about this is 10 seconds for browers)

- Practical use cases
(1) When origin is known but the exact target is unknown (eg. versioned file, image cdn with queries)
(2) Streaming Data : Data which is not necessarily required at the very initialization (normally have to wait till scripts are fully loaded), but minimizing the time consumed for connections setup is required