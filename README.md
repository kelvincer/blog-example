# blog-example
blog

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
root@6ed89fd004e8:/usr/src/VSCode/libcurl# valgrind --tool=memcheck --leak-check=yes ./o
==617== Memcheck, a memory error detector
==617== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==617== Using Valgrind-3.19.0 and LibVEX; rerun with -h for copyright info
==617== Command: ./o
==617==
POST request sent successfully.
Response data:
{"name":"John Doe","job":"Software Developer","id":"649","createdAt":"2024-08-29T22:41:59.381Z"}
==617==
==617== HEAP SUMMARY:
==617==     in use at exit: 0 bytes in 0 blocks
==617==   total heap usage: 143,331 allocs, 143,331 frees, 6,378,789 bytes allocated
==617==
==617== All heap blocks were freed -- no leaks are possible
==617==
==617== For lists of detected and suppressed errors, rerun with: -s
==617== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
``` 
