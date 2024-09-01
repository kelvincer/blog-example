# Code
```c
#include <curl/curl.h>

int main()
{
    CURL *curl;
    CURLcode res;

    curl_global_init(CURL_GLOBAL_ALL);

    CURL *hnd = curl_easy_init();

    curl_easy_setopt(hnd, CURLOPT_CUSTOMREQUEST, "POST");
    curl_easy_setopt(hnd, CURLOPT_WRITEDATA, stdout);
    curl_easy_setopt(hnd, CURLOPT_URL, "https://dummy.restapiexample.com/api/v1/create");

    static const char *jsonData = "{\"name\":\"test\",\"salary\":\"123\",\"age\":\"23\"}";

    struct curl_slist *headers = NULL;
    headers = curl_slist_append(headers, "accept: application/json");
    headers = curl_slist_append(headers, "content-type: application/json");
    curl_easy_setopt(hnd, CURLOPT_HTTPHEADER, headers);
    curl_easy_setopt(hnd, CURLOPT_POSTFIELDS, jsonData);

    CURLcode ret = curl_easy_perform(hnd);

    curl_slist_free_all(headers);
    curl_easy_cleanup(hnd);
    curl_global_cleanup();

    return 0;
}
```
# Shell output
```console
MacBook-Pro-de-kelvin:libcurl kelvinc$ leaks --atExit --list  -- ./o
o(64994) MallocStackLogging: could not tag MSL-related memory as no_footprint, so those pages will be included in process footprint - (null)
o(64994) MallocStackLogging: recording malloc and VM allocation stacks using lite mode
{"status":"success","data":{"name":"test","salary":"123","age":"23","id":2556},"message":"Successfully! Record has been added."}Process:         o [64994]
Path:            /Users/USER/*/o
Load Address:    0x10eb11000
Identifier:      o
Version:         0
Code Type:       X86-64
Platform:        macOS
Parent Process:  leaks [64993]

Date/Time:       2024-09-01 16:23:31.353 -0500
Launch Time:     2024-09-01 16:23:29.531 -0500
OS Version:      macOS 14.6.1 (23G93)
Report Version:  7
Analysis Tool:   /Applications/Xcode.app/Contents/Developer/usr/bin/leaks
Analysis Tool Version:  Xcode 15.4 (15F31d)

Physical footprint:         4404K
Physical footprint (peak):  4404K
Idle exit:                  untracked
----

leaks Report Version: 3.0
Process 64994: 3022 nodes malloced for 205 KB
Process 64994: 0 leaks for 0 total leaked bytes.
``` 
