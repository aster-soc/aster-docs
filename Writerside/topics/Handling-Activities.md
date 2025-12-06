# Handling Activities

Aster makes it easy to add your own activity handler, but be aware that each activity type can only support one
handler set to it.

Throughout this page a hypothetical "Poke" activity will be used. In JSON, it'd look something like this:

```json
{
  "@context": [],
  "id": "https://example.com/activities/something",
  "type": "Poke",
  "actor": "https://example.com/users/someone",
  "object": "https://other.example.com/users/someoneElse"
}
```

Activity handlers inherit the ApInboxHandler interface and must implement handling logic. They must also be
registered with the InboxHandlerRegistry.

```kotlin
class ApPokeActivity : ApInboxHandler {
	private val logger = LoggerFactory.getLogger(ApAcceptHandler::class.java)

	override suspend fun handle(job: InboxQueueEntity) {
		// Handle activity
	}
}

// Somewhere in your enable hook...
InboxHandlerRegistry.register<ApPokeActivity>("Poke")
```

## Writing handling logic

Activities need to be decoded into a class to be properly used in inbox handling logic. Make sure
you create a class that represents the activity. [Read more about creating activity classes.](Creating-Activities.md)

```kotlin
override suspend fun handle(job: InboxQueueEntity) {
	val activity = jsonConfig.decodeFromString<ApPokeActivity>(String(job.content.bytes))
}
```