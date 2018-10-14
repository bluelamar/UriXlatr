# UriXlatr
Translate URL and HTTP method to user defined Action and Resource

This package will translate a given URL and associated HTTP method into a user defined Action and Resource. It requires a user supplied translation map in JSON that is read upon startup. The map is kept in an internal form such that the best match for a URL and HTTP method will return the translation.

Example of the translation map follows. Note that the user defines the string they wish for the **namespace** and the list of translation **matches**.

```
{
  namespace: "my-name-space",
  matches: [
    { 
      uri: "/some/path/",
      resource: "getting.path",
      method: "get",
      action: "read"
    },
    { 
      uri: "/some/path/",
      resource: "updating.path",
      method: "put",
      action: "update"
    },
    {
      uri: "/other/${name}",
      resource: "other.${name}",
      method: get,
      action: "read"
    }
   ]
}
```

Suppose client sends the following HTTP GET request:

http://api.example.com/some/path/

The best match will be the first object in the **matches** list where the **method** and **uri** are exact matches.
The returned translation will be the tuple: "read", "getting.path"
