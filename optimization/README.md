# How to optimize performance in Web front-end

### 1. Code splitting
- Splitting the code could be done by
    - routes (for React)
    - modules
    - components
- Code splitting creates delay when the lazy-loaded code actually becomes required. The delay could be minimized with 'preloading' the code. Preloading the code could be done by calling import statement with the same path. The result of the import doesn't have to be stored or directly utilized. Just calling the import and pre-calling the request would do the job.
- There is no absolute answer for 'when to preload' the splitted code. This should be decided by the detailed status of the application. Several examples could be:
    - When component has mounted (call import inside empty dependency useEffect)
    - When user puts mouse over element which would show lazy loaded component when clicked.

### 2. Find bottleneck code
- Utilize performance tab of devtools and find task with long runtime.

### 3. Reduce bundle size
- Use bundler features (eg. uglify)
- Use bundle analysis tool (eg. webpack-bundle-analyzer)
- Enable text compression (set Content-Encoding in response header from the server)
    - gzip: Use deflate internally + extra (higher compression)
    - deflate
- Remove unused code from the bundle
    - Check Coverage tab inside devtools to see the percentage of code actually being used.
        - Coverage tab shows the percentage of code used at least once till the moment. The percentage would start low and increase as user starts interacting with the page.
        - Coverage tab could be hidden by default (click more tools button)
    - PurgeCSS
        - Remove unused css codes (considered as "unused" if the css code doesn't include any keyword)
        - The keyword could be extracted from element tags, content, classnames, etc.
        - The rule for extracting the keywords could be customized.

### 4. Preconnect to required origins
- Pre-setup tasks for connecting to important 3rd party origin which could take a lot of time (DNS search, TCP hand-shaking, ..)
- add <link rel="preconnect" href=`${link}` /> 
- For several types of resources (eg. font), crossorigin attribute is required (<link rel="preconnect" href="~" crossorigin >)
- Caution
    - Preconnect consumes quite a bit of CPU especially with credential communication
    - Abusing preconnect would cause bandwidth contention. For the less important ones, you could use <link rel="dns-prefetch" href="~"> to only save time for DNS lookup
    - If the browser is going to be closed after a short term, preload is ineffective (average standard about this is 10 seconds for browers)

- Practical use cases
    - When origin is known but the exact target is unknown (eg. versioned file, image cdn with queries)
    - Streaming Data : Data which is not necessarily required at the very initialization (normally have to wait till scripts are fully loaded), but minimizing the time consumed for connections setup is required

### 5. CSS Animation
- To check whether animation is being played smoothly, measure performance from devtools while setting CPU config to slow down.
- Jank: Stuttering or choppiness issue in UI, normally caused by executing heavy task on the main thread which blocks rendering. Can be noticeable if browser rendering is done under 60 FPS.
- GPU-accelerated CSS properties: Using the following properties of CSS would separate the element into a separated layer. The style updates will be delegated from CPU to GPU, which would update style faster without triggering reflow or repaint. (but having to many layers could trigger overhead as well)
    - transform (eg. use transform: scaleX() instead of changing width)
    - opacity

### 6. Image
- Lazy loading image could help if there are many image items in the page which do not need to be exposed to the user immediately.
    - Implement custom logic based on intersection observer
    - Utilize next/image (provide lazy loading by default)
    - img element supports 'loading' attribute with the same way as next/image
- Preloading the image could be useful if the loading process of the image asset could affect the layout in a bad way
    - Fetch image in advance (eg. new Image().src = '~')
    - Set loading attribute of next/image 'eager' (default is 'lazy' should for lazy loading)
- Set image size properly
    - Use image CDN
    - Utilize srcset, sizes attribute of img component: standard could be either w (width) or x (resolution)
    - Utilize next/Image
    - Useful tools
        - Squoosh (provided by Google)
- Use next-gen formats (like webp, but must check browser compatibility, in that case utilize picture + source - same with videos)
    - For <videos />, source requires 'src'
    - For <pictures />, source requires 'srcSet' (<img /> still requires src)
    - <picture>
        <source srcSet="~.webp"/>
        <source ~ />
        <img src="~.jpg" /> // Universal format as an insurance
        </picture>

### 7. Font
- Browsers have different default behaviors with applying fonts:
    - FOUT (Flash of Unstyled Text): Show text in default font and then apply the actual font once it's loaded (eg. Microsoft Edge)
    - FOIT (Flash of Invisible Text): Only show text once font is downloaded and applied (eg. Chrome, Firefox, Safari) - But in some cases like Chrome, if font isn't loaded under certain amount of time, it just shows text with default font and later replaces it.
    - The mechanism could be controlled based on `font-display` CSS property inside `@font-face`:
        - auto: follow default behavior of the browser
        - block: FOIT (show text with default font after 3s)
        - swap: FOUT
        - fallback: FOIT (show text with default font after 0.1s, replace if font is loaded under 3s. Otherwise, only cache the font without replacing)
        - optional: FOIT (show text with default font after 0.1s, decided whether to apply the font based on the network status)

    - The detailed option could be decided by the content:
        - Important texts (eg. title): use FOUT
        - Unnoticeable texts: use FOIT (changes in the layout would trigger user's attention unnecessarily)
- Font file size: WOFF2 < WOFF < TTF/OTF < EOT (but some formats may have compabitility issues)
- Just like image or video source, you could give multiple options for font source (order lighter ones first)
    - eg. `src: url('font.woff') format('woff'), url('font.ttf) format('truetype);`