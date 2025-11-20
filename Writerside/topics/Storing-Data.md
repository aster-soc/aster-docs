# Storing Data

Aster has a key-val store specifically for plugins to use. This service, unlike others, does not require
having Exposed as a dependency since it has methods that only take strings rather than queries.

<note>
The key-val service stores values in the database, so treat these like any query when thinking about performance.
</note>

## Using KeyValService

```kotlin
// Override a value, or create it new
KeyValService.set("myplugin-counter", "0")

repeat(10) {
	val counter = KeyValService.get("myplugin-counter") as Int
    KeyValService.set("myplugin-counter", counter++)
}

// Delete the key and value
KeyValService.delete("myplugin-counter")
```