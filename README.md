# UriXlatr
Translate URL and HTTP method to user defined Action and Resource

This package will translate a given URL and associated HTTP method into a user defined Action and Resource. It requires a user supplied translation map in JSON that is read upon startup. The map is kept in an internal form such that the best match for a URL and HTTP method will return the translation. Use of the **namespace** in the returned **resource** is configurable.

Example of the translation map follows. Note that the user defines the string they wish for the **namespace** and the list of translation **matches**.

```
{
  namespace: "final-frontier",
  matches: [
    { 
      "uri": "/paths/",
      "resource": "getting.paths",
      "method": "get",
      "action": "read"
    },
    { 
      "uri": "/paths/",
      "resource": "updating.paths",
      "method": "put",
      "action": "update"
    },
    {
      "uri": "/planet/path",
      "resource": "planet.${name}",
      "method": "get",
      "action": "retrieve"
    }
   ]
}
```

## Suppose the client sends the following HTTP GET request:

http://api.finalfrontier.com/paths/

The best match will be the first object in the **matches** list where the **method** and **uri** are exact matches.
The returned translation will be the tuple: **{ "read", "getting.paths" }**

If configured to use the **namespace**, the returned tuple may be something like: **{ "read", "final-frontier:getting.paths" }**

Notice the 2nd object in the list, although matches the **uri**, does not match the **method**, therefore that object is ignored.

## Suppose the client sends the following HTTP GET request:

http://api.finalfrontier.com/planet/path?name=jupiter

The best match will be the 3rd object in the **matches** list where the **method** and **uri** are exact matches. 

The query parameter is **name** which matches the variable in the **resource**. Therefore we have a full match and the returned translation tuple will be: **{ "retrieve", "planet.jupiter" }**

If configured to use the **namespace**, the returned tuple may be something like: **{ "retrieve", "final-frontier:planet.jupiter" }**

## Example showing variable in **uri** and **resource**:

Suppose we have this map object:

```
{
  "uri": "/planet/path/${name}",
  "resource": "planet.${name}",
  "method": "post",
  "action": "emplace"
}
```

Then a **POST** request is sent to :

http://api.finalfrontier.com/planet/path/earth

In this case, the **uri** will match positionally with variable **name** in the **uri**. This matched variable will then be used in the **resource** string.
Therefore we have a match and the returned translation tuple will be: **{ "emplace", "planet.earth" }**

## Example showing multiple variables in **uri** and **resource**:

Suppose we have this map object:

```
{
  "uri": "/planet/${name}/moon/${moon}",
  "resource": "moon.${moon).planet.${name}",
  "method": "post",
  "action": "setorbit"
}
```

Then a **POST** request is sent to :

http://api.finalfrontier.com/planet/earth/moon/luna

In this case, the **uri** will match positionally with variable **name** and **moon** in the **uri**. The matched variables will then be used in the **resource** string. Position doesnt matter, just use of same name.
Therefore we have a match and the returned translation tuple will be: **{ "setorbit", "moon.luna.planet.earth" }**
