# Creating Activities

Activities are the basis for communication in ActivityPub, and they need class representations
in Aster.

They must be a serializable data class that inherits ApObjectWithContext. This will automatically append
`@context` which is needed for federation.

If you have a property that can be an ID or an object, ApIdOrObject can be either. Since it's nested inside
an activity, it requires `@Serializable(with = NestedApObjectSerializer::class)` to prevent having `@context`
twice when being translated to JSON.

```kotlin
@Serializable
data class ApCreateActivity(
	val id: String,
	val type: ApType.Activity = ApType.Activity.Create,

	val actor: String? = null,
	@Serializable(with = NestedApObjectSerializer::class)
	val `object`: ApIdOrObject,

	val to: List<String>,
	val cc: List<String>
) : ApObjectWithContext()
```

## Activity Type

You can just set type to be a string, or you can choose to extend `ApType.Activity`.

## Using ApIdOrObject

When you go to set the value of `object`, you'll need to wrap your value in a helper function.

```kotlin
// Setting to an object
ApCreateActivity(
	`object` = ApIdOrObject.createObject { ApNote.fromEntity(note) }
)

// Setting to a JsonArray
ApCreateActivity(
	`object` = ApIdOrObject.createArray { JsonArray() }
)

// Setting to an ID
ApCreateActivity(
	`object` = ApIdOrObject.Id("https://example.com/some/id")
)
```