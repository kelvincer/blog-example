# Code
```c
#include <curl/curl.h>

int main()
{
    const char *jsonString = "{\"name\":\"test\",\"salary\":\"123\",\"age\":\"23\"}";

    curl_global_init(CURL_GLOBAL_ALL);
    CURL *curl = curl_easy_init();

    if (curl)
    {
        curl_easy_setopt(curl, CURLOPT_CUSTOMREQUEST, "POST");
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, stdout);
        curl_easy_setopt(curl, CURLOPT_URL, "https://dummy.restapiexample.com/api/v1/create");

        struct curl_slist *headers = NULL;
        headers = curl_slist_append(headers, "accept: application/json");
        headers = curl_slist_append(headers, "content-type: application/json");
        curl_easy_setopt(curl, CURLOPT_HTTPHEADER, headers);
        curl_easy_setopt(curl, CURLOPT_POSTFIELDS, jsonString);

        CURLcode ret = curl_easy_perform(curl);

        curl_slist_free_all(headers);
        curl_easy_cleanup(curl);
    }

    curl_global_cleanup();

    return 0;
}
```
# Shell output
```console
MacBook-Pro-de-kelvin:libcurl kelvinc$ leaks --atExit --list  -- ./o
o(72615) MallocStackLogging: could not tag MSL-related memory as no_footprint, so those pages will be included in process footprint - (null)
o(72615) MallocStackLogging: recording malloc and VM allocation stacks using lite mode
{"status":"success","data":{"name":"test","salary":"123","age":"23","id":763},"message":"Successfully! Record has been added."}
Process:         o [72615]
Path:            /Users/USER/*/o
Load Address:    0x10fb30000
Identifier:      o
Version:         0
Code Type:       X86-64
Platform:        macOS
Parent Process:  leaks [72614]

Date/Time:       2024-09-01 17:07:45.438 -0500
Launch Time:     2024-09-01 17:07:43.972 -0500
OS Version:      macOS 14.6.1 (23G93)
Report Version:  7
Analysis Tool:   /Applications/Xcode.app/Contents/Developer/usr/bin/leaks
Analysis Tool Version:  Xcode 15.4 (15F31d)

Physical footprint:         4408K
Physical footprint (peak):  4408K
Idle exit:                  untracked
----

leaks Report Version: 3.0
Process 72615: 3022 nodes malloced for 205 KB
Process 72615: 0 leaks for 0 total leaked bytes.
``` 
