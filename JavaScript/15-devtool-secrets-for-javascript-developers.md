[15-devtool-secrets-for-javascript-developers](https://blog.asayer.io/15-devtool-secrets-for-javascript-developers)

### 1. Use incognito mode
Incognito or private mode uses a separate user profile and does not retain data such as cookies, localStorage, 
or cached files between browser restarts. Each session starts in a clean state so it can be ideal for testing login systems, 
first-load performance, and Progressive Web Apps (PWA).

### [2. Auto-start DevTools](https://blog.asayer.io/15-devtool-secrets-for-javascript-developers#2-auto-start-devtools)


### 3. Use the command palette
!!!This will only work if an instance of Chrome is not already running. 

### 4. Find unused JavaScript
Chrome’s Coverage panel allows you to quickly locate JavaScript (and CSS) code that has — and has not — been used. To start, 
open Coverage from the More tools sub-menu in the DevTools menu.

### 5. Locate DOM-changing code
Determine which function is responsible for updating a specific HTML DOM element when an event occurs. 

### 6. Throttle network speed
This can help identify the cause of performance bottlenecks

### 7. Filter network requests
[Chrome DevTools: How to Filter Network Requests](https://www.freecodecamp.org/news/chrome-devtools-network-tab-tricks/) </br>
The DevTools Network panel offers several filters including a JS button to only show JavaScript requests

* filter cached requests with is:cached
  
* filter incomplete requests with is:running
  
* identify large requests by entering larger-than: `<S>`, where `<S>` is a size in bytes (1000000), kilobytes (1000k), or megabytes (1M), or
  
* identify third-party resources by entering -domain:<yourdomain>. Your domain can use wildcard characters such as *.

### 8. Blackbox scripts and lines
It’s usually futile trying to debug problems in libraries (React, Vue.js, jQuery, etc.) or third-party scripts (analytics, social media widgets, chat bots, etc.) which you cannot easily change.
Alternatively, click the settings cog icon or press F1 to access the Settings, then switch to the Ignore List tab. 

### [9. Use logpoints](https://umaar.com/dev-tips/186-logpoint/)
You can use the new Logpoint feature to quickly inject a console.log message into your JavaScript code with any variables you like

### 10. Use conditional breakpoints
It halts a script at that point during execution so you can step through code to inspect variables, the call stack, etc.
Breakpoints are not always practical, e.g. if an error occurs during the last iteration of a loop which runs 1,000 times. 
A conditional breakpoint however, can be set so it will only trigger when a certain condition is met, e.g. i > 999. 

### 11. Halt infinite loops
To stop an infinite loop in Chrome DevTools, open the Sources panel and click the debugging pause icon to stop the script. 
Hold down the same icon and choose the square stop icon to discontinue script processing.

### 12. Re-run Ajax requests
DevTools shows lots of information but it’s sometimes practical to re-run an Ajax call and analyse the results in another tool.

### 13. Enable local file overrides
Chrome allows any HTTP request to use a local file on your device rather than fetch it over the network. This could allow you to:
* edit scripts or styles on a live site without requiring build tools
* develop a site offline which normally requests essential files from a third-party, or domain
* temporarily replace an unnecessary script such as analytics. </br>

It will also be present in the Overrides tab and the localfiles directory. 
The file can be edited within Chrome or using any code editor — the updated version is used whenever the page is loaded again.

### 14. Manage client-side storage
Web pages can store data on the client using a variety of techniques. 
The Application panel in Chrome DevTools (or Storage panel in Firefox) allows you to add, examine, modify, and delete values held in cookies, cache storage, localStorage, sessionStorage, IndexedDB, and Web SQL (where supported).

### 15. Emulate mobile hardware
Smart phones and tablets often include hardware such as Global Positioning Systems (GPS), gyroscopes, and accelerometers.
Chrome can emulate device hardware in DevTools — choose Sensors from the More tools menu
