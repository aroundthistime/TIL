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


### 2. Web Accessibility
- *Web accessibility* means that websites, tools, and technologies are designed and developed so that *people with disabilities can use properly them*. (the disability could be in any kinds: auditory, cognitive, speech, visual, ..)
- Web accessibility is not only targeting people with disabilities but also who are not:
    - people from *different types of devices* (eg. smart watches, smart TV, smart glasses, ..)
    - people with *temporal disabilities* (eg. broken arm, lost glasses)
    - people with *situational limitations* (eg. sunlight being too bright to check the screen, too loud to hear the audio)
    - people using *slow network*.
- To say that *Web Accessibility* is being respected, people should be able to:
    - perceive, understand, navigate, and interact with the Web
    - contribute to the web
- Methods to achieve Web Accesibility from Front-End:
    - Use `Semantic Markups`
        - Use `Semantic Tags`: tags defining meaning and the purpose of the content (eg. `main`, `nav`, `section`)
        - Even if it's not a `Semantic Tag`, select tags while respecting HTML5 specs (eg. use `a` for links, `button` for click events)
        - improves *Web Accessibility* (eg. properly using `Semantic Markups` gives more context to *screen readers* or any other assistive technologies)
        - improves *SEO*
        - improves *readability* with consistent structure design
    
    - Provide `alt` texts
        - For adding visual contents (eg. `img`, `video`)
        - Helps screen reader users get context about the visual content.
        - `<figure />`: tag used to render additional contents (eg. image, charts) to help understanding of nearby contents. The content of this tag should be optional (it's OK to be omitted but just to help if it exists) and could be used with `<figcaption />` which adds *caption* to add more information inside the `<figure />`.
    
    - Add `ARIA` attributes (Accessible Rich Internet Applications)
        - `ARIA` attributes are used to express *roles* and *attributes* to better define the web content.
        - It is important to prioritize *native HTML specs* over `ARIA` attributes. `ARIA` attribute should be the last option which will be used when native HTML specs aren't enough to express the information.

    - Set proper designs (eg. font size, color contrast of content and the background)
    - Tools to check *Web Accessibility*: WAVE, AXE, Lighthouse, NVDA, ..