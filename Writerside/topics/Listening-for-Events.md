# Listening for Events

Aster has a lot of events for a lot of situations. For example, an event is called when a user likes a note. An event
is also fired at every stage of the server starting.

There are two ways you can add an event listener, the first is preferred for Kotlin, and the second is required for Java.

## Kotlin

This method requires Kotlin since it's heavy on Kotlin-specific features. It's preferred however, because the event 
is automatically cast to the correct type.

```kotlin
EventRegistry.addListener<NoteLikeEvent> { event -> // event is the NoteLikeEvent type
	println("${event.user.id} liked note ${event.note.id}")
}
```

We can also add priority to event listeners so they run before the others. If you don't have an explicit reason why to 
use higher priority, don't.

```kotlin
EventRegistry.addListener<NoteLikeEvent>(ListenerPriority.High) { event ->
	println("${event.user.id} liked note ${event.note.id}")
}
```

## Java and Other Languages

This can also be done in Kotlin, and would look like this. 

```kotlin
EventRegistry.addListener(NoteCreateEvent::class) { event ->
	event as NoteCreateEvent
	println(event.note)
}

EventRegistry.addListener(NoteCreateEvent::class, { event ->
	event as NoteCreateEvent
	println(event.note)
}, ListenerPriority.High)
```

TODO: Java