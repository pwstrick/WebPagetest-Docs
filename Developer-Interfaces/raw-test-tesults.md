# 原始测试结果
## 一、页面数据字段
这些是页面级（摘要）结果文件中，从“Raw Page data”链接导出的CSV文件中列。 

Date 
    1. Time 
    *. Event Name  
    4. URL
    5. Load Time (ms)
    6. Time to First Byte (ms)
    7. unused
    8. Bytes Out - 从版本53起，这将总是包含测试的总和，而不考虑测量类型
    9. Bytes In - As of build 53 this will always contain the total for the test, regardless of measurement type
    10. DNS Lookups - 同上
    11. Connections - 同上
    12. Requests - 同上
    13. OK Responses - 同上
    14. Redirects - 同上
    15. Not Modified - 同上
    16. Not Found - 同上
    17. Other Responses - 同上
    18. Error Code
    19. Time to Start Render (ms)
    20. Segments Transmitted
    21. Segments Retransmitted
    22. Packet Loss (out)
    23. Activity Time(ms)
    24. Descriptor
    25. Lab ID
Dialer ID
Connection Type
Cached
Event URL
Pagetest Build
Measurement Type - (DWORD - 1 for web 1.0, 2 for web 2.0)
Experimental (DWORD)
Document Complete Tims (ms)
Event GUID - (matches with Event GUID in object data) - Added in build 42
DOM Element Time (ms) - Added in build 45
Includes Object Data - Flag to indicate if object data is available (1 for yes, 0 for no)
Cache Score - % of static assets that had expires headers to enable browser caching. Added in build 51
Static CDN Score - % of static assets that were served from a cdn. Added in build 51
One CDN Score - 100 - 10 * the number of different CDN's used. Added in build 51
GZIP Score - % of text or js assets that were gzip encoded. Added in build 51
Cookie Score - % of responses that wrote cookies NOT to the aol.com domain. Added in build 51
Keep-Alive Score - % of responses from servers that served more than one asset that used persistent connections. Added in build 51
DOCTYPE Score - % of html or xhtml assets that had a valid DOCTYPE
Minify Score - % of html or js assets that were minified (less that 10% of lines had leading/trailing whitespace or comments). Added in build 51
Combine Score - % of js or css assets that were the only asset of that type served from a given host (more than one of that type from the same host could have been combined). Added in build 51
Bytes Out (Document) - Added in build 53
Bytes In (Document) - Added in build 53
DNS Lookups (Document) - Added in build 53
Connections (Document) - Added in build 53
Requests (Document) - Added in build 53
OK Responses (Document) - Added in build 53
Redirects (Document) - Added in build 53
Not Modified (Document) - Added in build 53
Not Found (Document) - Added in build 53
Other Responses (Document) - Added in build 53
Compression Score - % of image objects that passed the image compression test - Added in build 54
Host - Host name portion of the URL retrieved - Added in build 57
IP Address - IP address of the first request on the page - Added in build 66
ETag Score - % of requests that passed the ETag header check- Added in build 170
Flagged Requests - count of "flagged" requests, internal use only at this point - Added in build 179
Flagged Connections - count of connections to "flagged" networks - Added in build 179
Max Simultaneous Flagged Connections - count of the maximum number of simultaneous "flagged" connections - Added in build 179
Time to Base Page Complete (ms) - Time from the start of the test until the base HTML page has finished downloading (includes DNS lookup, socket connect, redirects, etc) - Added in build 179
Base Page Result - Response code for the base page (200, 404, etc). If redirects are in place this will be the result of the actual base page after the redirects - Added in build 179
Gzip Total Bytes - Total size of applicable resources for Gzip compression - Added in build 179
Gzip Savings - Bytes that could have been saved through Gzip compression - Added in build 179
Minify Total Bytes - Total size of applicable resources for Minification - Added in build 179
Minify Savings - Bytes saved through Minification - Added in build 179
Image Compression Total Bytes - Total size of applicable resources for image compression - Added in build 179
Image Compression Savings - Bytes saved through image optimization - Added in build 179
Base page redirect count - Number of redirects for the base page - Added in build 180
Optimization Checked - 1 if the optimization checks were run and 0 if they were not - Added in build 209
Above-the-Fold Time (ms) - Time until the above-the-fold stabilized (if explicitly requested) - Added in build 260
DOM Element Count - Number of DOM Elements on the document (including sub-documents in frames) - Added in build 274
Page Speed Version - Version of the Page Speed SDK that was used - Added in build 281
Page Title - Title of the resulting document (final title) - Added in build 289
Time to Title - Time from the start of the operation until the title first changed (in ms) - Added in build 289
Load Event Start
Load Event End
DOM Content Ready Start
DOM Content Ready End
Last Visual Change - Time of the last visual change to the page (in ms, only available when video capture is enabled)
Browser Name
Browser Version
Base Page Server Count - Number of IP addresses returned for the DNS lookup of the base page
Server RTT - Estimated Round Trip Time to the base server (taken from the socket connect time of the base page), blank if not available
Base Page CDN - Name of the CDN (if any) used to serve the base HTML page
Adult Site Flag - 0 if it is not likely an adult site and 1 if it is possibly adult content
Fixed Viewport - 1 of the page has a meta viewport defined
Progressive JPEG Score - -1 for N/A, otherwise a percentage of the JPEG bytes that were for progressive JPEGs
First Paint - Browser-reported first paint time (IE-specific right now - window.performance.timing.msFirstPaint)
Peak Memory across browser processes (deprecated)
Peak browser process count (deprecated)
Doc Complete CPU Time (ms)
Fully Loaded CPU Time (ms)
Doc Complete CPU Utilization (%)
Fully Loaded CPU Utilization (%)
Is Site Responsive (deprecated)
Browser Process Count - Measured at the end of a page test
Working set size for the main browser process (KB) - Measured at the end of a page test
Child Private Working Set Size (KB) - Sum of private working sets for all browser processes excluding the main process, Measured at the end of a page test
Time to DOM Interactive - From Navigation Timing
Time to DOM Loading - From Navigation Timing
Base Page TTFB - Time from the request to the response of the base page element
Visually Complete - First time when the visual progress reaches 100%
SpeedIndex - The calculated Speed Index
Certificate Bytes - Total bytes in server-supplied TLS certificates
## 二、请求数据字段
## 三、聚合结果（批量测试）