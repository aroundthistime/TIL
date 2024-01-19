# How to optimize performance in Web front-end

### 1. Set image size properly
-  Use image CDN
- Utilize srcset, sizes attribute of img component: standard could be either w (width) or x (resolution)
- Utilize next/Image

### 2. Code splitting
- Splitting the code could be done by
    (1) routes (for React)
    (2) modules
    (3) components
- Code splitting creates delay when the lazy-loaded code actually becomes required. The delay could be minimized with 'preloading' the code. Preloading the code could be done by calling import statement with the same path. The result of the import doesn't have to be stored or directly utilized. Just calling the import and pre-calling the request would do the job.
- There is no absolute answer for 'when to preload' the splitted code. This should be decided by the detailed status of the application. Several examples could be:
    (1) When component has mounted (call import inside empty dependency useEffect)
    (2) When user puts mouse over element which would show lazy loaded component when clicked.

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
- For several types of resources (eg. font), crossorigin attribute is required (<link rel="preconnect" href="~" crossorigin >)
- Caution
    (1) Preconnect consumes quite a bit of CPU especially with credential communication
    (2) Abusing preconnect would cause bandwidth contention. For the less important ones, you could use <link rel="dns-prefetch" href="~"> to only save time for DNS lookup
    (3) If the browser is going to be closed after a short term, preload is ineffective (average standard about this is 10 seconds for browers)

- Practical use cases
    (1) When origin is known but the exact target is unknown (eg. versioned file, image cdn with queries)
    (2) Streaming Data : Data which is not necessarily required at the very initialization (normally have to wait till scripts are fully loaded), but minimizing the time consumed for connections setup is required

### 6. CSS Animation
- To check whether animation is being played smoothly, measure performance from devtools while setting CPU config to slow down.
- Jank: Stuttering or choppiness issue in UI, normally caused by executing heavy task on the main thread which blocks rendering. Can be noticeable if browser rendering is done under 60 FPS.
- GPU-accelerated CSS properties: Using the following properties of CSS would separate the element into a separated layer. The style updates will be delegated from CPU to GPU, which would update style faster without triggering reflow or repaint. (but having to many layers could trigger overhead as well)
    (1) transform (eg. use transform: scaleX() instead of changing width)
    (2) opacity