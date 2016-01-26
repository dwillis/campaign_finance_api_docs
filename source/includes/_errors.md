# Errors

<aside class="notice">This error section is stored in a separate file in `includes/_errors.md`. Slate allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside>

The ProPublica Campaign Finance API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is improperly configured
401 | Unauthorized -- Your API key is wrong or missing
403 | Forbidden -- Your request is not allowed
404 | Not Found -- Your request worked, but did not find a response
405 | Method Not Allowed -- You tried to access a response with an invalid method
406 | Not Acceptable -- You requested a format that isn't json or xml
429 | Too Many Requests -- You've made too many requests.
500 | Internal Server Error -- We had a problem with our server. Try again later, and let us know what you requested at apihelp@propublica.org.
503 | Service Unavailable -- We're temporarily offline for maintenance or because of another issue. Please try again later.
