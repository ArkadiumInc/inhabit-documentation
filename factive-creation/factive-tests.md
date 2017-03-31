# Inhabit factive tests manual

## Error code types:

* Skip\(first character is `S`\) - this test is not applicable to this factive or this test depends to previous failed test
* Error \(first character is `E`\) 
* Warning \(first character is `W`\)

Factive pass the test suite if there are no one test produce error code of type `Error`

## How to start factive in sandbox

* open [sandbox](http://inhabit-test-fiddler-pages.azurewebsites.net/index-live.html)
* run in browser javascript console `RunWidget(<factive moduleId>, <factive version>, <factive exampleContextualUrl>)`

Notifications:

* **presenter.module** - http\(s\) request **//my\_callback\_handler\_to\_proxy\_success\_module** 
* **presenter.content** - http\(s\) request **//my\_callback\_handler\_to\_proxy\_success\_content**
* **presenter.module.getcontent.error** - http\(s\) request **//my\_callback\_handler\_to\_proxy\_error\_emitted**
  You can see this notifications in the javascript log. 

Analytics events are stored in javascript variable window.scenarioEvents \(it is an array\) :

* module.moduleReady - OnReady
* module.moduleInteractionStart - OnInteractionStart
* module.moduleCycleStart - OnCycleStart
* module.moduleCycleEnd - OnCycleEnd

## How to simulate pagespeed tests

* Go to the [Google PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
* Enter page \(or proxy-page\) url in the input field and press "test"
* See the quality measure. 

# Tests

## Inhabit API Tests

### Test steps

* Open factive in [sandbox](http://inhabit-test-fiddler-pages.azurewebsites.net/index-live.html)
* Wait for any 2 notifications
* Run javascript API tests  
* Get the API test output and parse it

To reproduce third and fourth steps manually, run in browser javascript console function   
`UnitTesting(<moduleId>, <version>)`  
and see test log in javascript variable   
`tape.log`

### API tests

* Module has "moduleName" class property.
* Method with this name is a constructor
* This method constructs an object
* Methods `getContent`, `getTitle`, `getType`, `getThumbnail`, `display` do not throw Exception

### Error codes

* E.1.1 - factive has not been loaded \(API tests has not been started\)
* E.1.2 - Error in API tests \(see the log\)

## Start with default config test

### Test steps

* Open factive in [sandbox](http://inhabit-test-fiddler-pages.azurewebsites.net/index-live.html)
* Grab all network requests in next 30 seconds \(It will be used in the next tests\)
* Read all notifications. True result is one `presenter.module` and one `presenter.content` notification
* Filter from the network requests set  requests from libraries or from sandbox. If you see unknown request url in the next tests logs, please, notify me.

### Error codes

* E.2.1 - notification `presenter.module`  is absent \(factive has not been loaded\)
* E.2.2 - notification `presenter.module`  is present, but notification `presenter.content` is absent\(factive has been loaded, but factive has no content\)
* E.2.3 - more than 2 notifications \(undefined behaviour\)

## Test how an interactive fails on the network problems

### Test steps

if **Start with default config test** failed, skip this test  
In another case, for each grabbed url:

* Open factive in sandbox
* Block request by this url
* Wait for any 2 notifications. 

## Error codes

* E.3.1 - single notification `presenter.module` without notification `presenter.error` \(undefined behaviour\)
* W.3.2 - notification `presenter.module`  and notification `presenter.content` are presents \(may be uncatched error\)
* S.3.3 - **Start with default config test** failed

## Pagespeed test of the images

### Test steps

Filter images requests \(to any hosts\) from grabbed requests set   
For each image:

* Create proxy page `http://inhabit-test-fiddler-pages.azurewebsites.net/imageProxy.aspx?url=(url to image)`
* Test this page by Google pagespeed insights API

### Error codes

* W.4.1 - Quality is less than 90% 
* W.4.2 - Quality test failed 

## List of broken urls in factive package at module repository

### Test steps

Filter requests to files was hosted in modulerepository from grabbed requests set   
For each file url check this file presents in the modulerepository

### Error codes

* E.5.1.\(status code\) - request was failed with the `status code`
* W.5.2 - request throw Exception \(see the message in log\)

## https test

### Test steps

* if **Start with default config test** failed, skip this test
* open factive in [sandbox](http://inhabit-test-fiddler-pages.azurewebsites.net/index-live.html) with  https  protocol
* Grab all network requests in next 30 seconds
* Read all notifications. True result is one `presenter.module` and one `presenter.content` notification
* Compare grabbed set of this test and grabbed set of **Start with default config test**
* If test was failed, put into log request urls from grabbed set of **Start with default config test**, what does not present in grabbed set of this tests, in order of request time. Head of this list may contain hardcoded http protocol. 

### Error codes

* E.6.1 - notification `presenter.module`  is absent \(factive has not been loaded\)
* E.6.2 - notification `presenter.module`  is present, but notification `presenter.content` is absent \(factive has no content\)
* E.6.3 - more than 2 notifications \(undefined behaviour\)
* W.6.4 - request presents in http-grabbed set, but not in https-grabbed set \(this url may contain hardcoded protocol\)
* S.6.5 - **Start with default config test** failed

## Pagespeed test of the scripts

### Test steps

Filter javascript requests \(hosted only in modulerepository\) from grabbed requests set   
For each script:

* Create proxy page `http://inhabit-test-fiddler-pages.azurewebsites.net/ScriptProxy.aspx?url=(url to script)`
* Test this page by Google pagespeed insights API

### Error codes

* W.7.1 - Quality is less than 90% 
* W.7.2 - Quality test failed 

## Reading of the scripts content and search for `http://` pattern

### Test steps

Filter javascript requests \(hosted only in modulerepository\) from grabbed requests set   
For each script:

* download script body
* search "http:/" substring in the body

### Error codes

* W.8.1 - "http:/" substring was found in the body of the script
* W.8.2 - cannot load body of the script \(It is  E.5.1 or W.5.2 too\)

## Default factive use case test

### Test steps

* if **Start with default config test** failed, skip this test
* if default scenario from this factive moduleId not implemented, skip this test
* open factive in [sandbox](http://inhabit-test-fiddler-pages.azurewebsites.net/index-live.html)
* wait for any 2 notifications
* run default scenario 
* check where are no javascript errors in the console \(exclude known sandbox errors\)
* get the analytics  event list 
* check there are events `OnReady`, `onInteractionStart`, `OnCycleStart`, `OnCycleEnd` in this order in the analytics event list

### Error codes

* S.9.1 - **Start with default config test** failed
* S.9.2 - default scenario from this factive moduleId not implemented
* E.9.3 - analytics events list does not match the rules \(see the log\)
* W.9.4 - error in javascript console. \(see the log\)

## Factive speed test
### Test steps
- if **Start with default config test** failed, skip this test
- open factive in [sandbox]
- wait for any 2 notifications
- in the javascript variable `window.timeLog` see the timestamps: before constructor call `beforeCtor` and after event `OnReady` `afterCtor` 
### Error codes
- S.10.1 - **Start with default config test** failed
- E.10.2 - Not both timestamps are presents
- W.10.3 -  Time between timestamps more than 3000

## Font problems test
if **Start with default config test** failed, skip this test
In another case, for each grabbed font url group:
- Open factive in sandbox
- Block request by this urls
- Wait for any 2 notifications. 
### Error codes
- S.11.1 -  **Start with default config test** failed
- S.11.2 - No one font contains in grabbed urls  set
- W.11.3 - notification `presenter.module`  and notification `presenter.content` are presents (may be uncatched error)
- E.11.4 - single notification `presenter.module` without notification `presenter.error` (undefined behaviour)

# Feedback us

## Grabbed requests -&gt; filter

If you see unknown request url in the **Test how to interactive fails on the network problems** logs,  
send it us.  
Patterns in filters now:

* "/ai.0.js"
* "/unpkg.com"
* "/securepubads.g"
* "google"
* "lodash.min.js"
* "cgi.gstatic.com"

**thanks!**

