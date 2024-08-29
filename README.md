# blog-example
blog

# Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <curl/curl.h>
#include <json-c/json.h>

// Callback function to write received data into a string buffer
size_t WriteCallback(void *contents, size_t size, size_t nmemb, void *userp)
{
    size_t totalSize = size * nmemb;
    strncat(userp, contents, totalSize);
    return totalSize;
}

int main(void)
{
    CURL *curl;
    CURLcode res;
    json_object *jobj;

    const char *url = "https://reqres.in/api/users";
    // const char *jsonData = "{\"name\":\"John Doe\",\"job\":\"Software Developer\"}";
    char buffer[1000] = {0}; // Buffer to store the response data

    const char *question = "Mum, clouds hide alien spaceships don't they ?";
    const char *answer = "Of course not! (\"sigh\")";

    const char *name = "Pili Mili";
    const char *job = "Manager";

    int i;
    struct
    {
        int flag;
        const char *flag_str;
    } json_flags[] = {
        {JSON_C_TO_STRING_PLAIN, "JSON_C_TO_STRING_PLAIN"},
        {JSON_C_TO_STRING_SPACED, "JSON_C_TO_STRING_SPACED"},
        {JSON_C_TO_STRING_PRETTY, "JSON_C_TO_STRING_PRETTY"},
        {JSON_C_TO_STRING_NOZERO, "JSON_C_TO_STRING_NOZERO"},
        {JSON_C_TO_STRING_SPACED | JSON_C_TO_STRING_PRETTY, "JSON_C_TO_STRING_SPACED | JSON_C_TO_STRING_PRETTY"},
        {-1, NULL}};
    jobj = json_object_new_object();
    json_object_object_add(jobj, "question", json_object_new_string(question));
    json_object_object_add(jobj, "answer", json_object_new_string(answer));

    for (i = 0; json_flags[i].flag_str != NULL; i++)
    {
        printf("\nFlag %s:\n---\n", json_flags[i].flag_str);
        printf("%s\n---\n", json_object_to_json_string_ext(jobj, json_flags[i].flag));
    }

    // json_object_put(jobj);

    json_object *postData = json_object_new_object();
    json_object_object_add(postData, "name", json_object_new_string(name));
    json_object_object_add(postData, "job", json_object_new_string(job));

    const char *jsonData = json_object_to_json_string_ext(postData, JSON_C_TO_STRING_PLAIN);

    curl_global_init(CURL_GLOBAL_ALL); // Initialize libcurl
    curl = curl_easy_init();           // Initialize a CURL session

    if (curl)
    {
        curl_easy_setopt(curl, CURLOPT_URL, url); // Set the URL for the POST request

        // Specify that this is a POST request
        curl_easy_setopt(curl, CURLOPT_POST, 1L);

        // Set the data to be sent in the POST request
        curl_easy_setopt(curl, CURLOPT_POSTFIELDS, jsonData);

        // Set the Content-Type to application/json
        struct curl_slist *headers = NULL;
        headers = curl_slist_append(headers, "Content-Type: application/json");
        curl_easy_setopt(curl, CURLOPT_HTTPHEADER, headers);

        // Set the callback function to capture the response data
        curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, WriteCallback);
        // Pass the buffer to the callback function
        curl_easy_setopt(curl, CURLOPT_WRITEDATA, buffer);

        // Perform the request, res will get the return code
        res = curl_easy_perform(curl);

        // Check for errors
        if (res != CURLE_OK)
        {
            fprintf(stderr, "curl_easy_perform() failed: %s\n", curl_easy_strerror(res));
        }
        else
        {
            printf("POST request sent successfully.\n");
            // Print the response data
            printf("Response data:\n%s\n", buffer); // This line prints the return data
        }

        // Clean up
        curl_slist_free_all(headers);
        curl_easy_cleanup(curl);

        json_object_put(jobj);
        json_object_put(postData);
    }

    curl_global_cleanup(); // Clean up globally

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
