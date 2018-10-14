# UrixLatr
Translate URL and HTTP method to user defined Action and Resource

This package will translate a given URL and associated HTTP method into a user defined Action and Resource.
The translation map is JSON and is defined as follows:

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
      uri: "/other/${name}",
      resource: "other.${name}",
      method: get,
      action: "read"
    }
   ]
}
```
