# Events

- [Basic usage](#basic-usage)
- [Subscribing to events](#events-subscribing)
    - [Where to register listeners](#event-registration)
    - [Subscribe using priority](#subscribing-priority)
    - [Halting events](#subscribing-halting)
    - [Wildcard listeners](#wildcard-listeners)
- [Firing events](#events-firing)
    - [Passing arguments by reference](#event-pass-by-reference)
    - [Queued events](#queued-events)
- [Using classes as listeners](#using-classes-as-listeners)
    - [Subscribe to individual methods](#event-class-method)
    - [Subscribe to entire class](#event-class-subscribe)
- [Event emitter trait](#event-emitter-trait)

<a name="basic-usage"></a>
## Basic usage

The `Event` class provides a simple observer implementation, allowing you to subscribe and listen for events in your application. For example, you may listen for when a user signs in and update their last login date.

```php
Event::listen('auth.login', function ($user) {
    $user->last_login = new DateTime;
    $user->save();
});
```

This is event made available with the `Event::fire` method which is called as part of the user sign in logic, thereby making the logic extensible.

```php
Event::fire('auth.login', [$user]);
```

<a name="events-subscribing"></a>
## Subscribing to events

The `Event::listen` method is primarily used to subscribe to events and can be done from anywhere within your application code. The first argument is the event name.

```php
Event::listen('acme.blog.myevent', ...);
```

The second argument can be a closure that specifies what should happen when the event is fired. The closure can accept optional some arguments, provided by [the firing event](#events-firing).

```php
Event::listen('acme.blog.myevent', function ($arg1, $arg2) {
    // Do something
});
```

You may also pass a reference to any callable object or a [dedicated event class](#using-classes-as-listeners) and this will be used instead.

```php
Event::listen('auth.login', [$this, 'LoginHandler']);
```

> **NOTE**: The callable method can choose to specify all, some or none of the arguments. Either way the event will not throw any errors unless it specifies too many.

<a name="event-registration"></a>
### Where to register listeners

The most common place is the `boot` method of a [Plugin registration file](../plugin/registration#registration-methods).

```php
class Plugin extends PluginBase
{
    [...]

    public function boot()
    {
        Event::listen(...);
    }
}
```

Alternatively, plugins can supply a file named **init.php** in the plugin directory that you can use to place event registration logic. For example:

```php
<?php

Event::listen(...);
```

Since none of these approaches is inherently "correct", choose an approach you feel comfortable with based on the size of your application.

<a name="subscribing-priority"></a>
### Subscribe using priority

You may also specify a priority as the third argument when subscribing to events. Listeners with higher priority will be run first, while listeners that have the same priority will be run in order of subscription.

```php
// Run first
Event::listen('auth.login', function () { ... }, 10);

// Run second
Event::listen('auth.login', function () { ... }, 5);
```

> **NOTE**: Wildcard listeners do not support priority.

<a name="subscribing-halting"></a>
### Halting events

Sometimes you may wish to stop the propagation of an event to other listeners. You may do so by returning `false` from your listener:

```php
Event::listen('auth.login', function ($event) {
    // Handle the event

    return false;
});
```

<a name="wildcard-listeners"></a>
### Wildcard listeners

When registering an event listener, you may use asterisks to specify wildcard listeners. Wildcard listeners will receive the event name fired first, followed by the parameters passed through to the event as an array.

The following listener will handle all events that begin with `foo.`.

```php
Event::listen('foo.*', function ($event, $params) {
    // Handle the event...
});
```

`$event` provides the event name that was fired.
`$params` is the event payload. This will be an array of arguments passed from the fired event.

Given the following event:
```php
Event::fire('foo.event', function ($arg1, $arg2) {
   // ...
}
```

When using a wildcard listener, this would map to the following:
```php
Event::listen('foo.*', function ($event, $params) {
    $arg1 = $params[0];
    $arg2 = $params[1];
}
```

<a name="events-firing"></a>
## Firing events

You may use the `Event::fire` method anywhere in your code to make the logic extensible. This means other developers, or even your own internal code, can "hook" to this point of code and inject specific logic. The first argument of should be the event name.

```php
Event::fire('myevent')
```

It is always a good idea to prefix event names with your plugin namespace code, this will prevent collisions with other plugins.

```php
Event::fire('acme.blog.myevent');
```

The second argument is an array of values that will be passed as arguments to [the event listener](#events-subscribing) subscribing to it.

```php
Event::fire('acme.blog.myevent', [$arg1, $arg2]);
```

The third argument specifies whether the event should be a [halting event](#subscribing-halting), meaning it should halt if a "non null" value is returned. This argument is set to false by default.

```php
Event::fire('acme.blog.myevent', [...], true);
```

If the event is halting, the first value returned with be captured.

```php
// Single result, event halted
$result = Event::fire('acme.blog.myevent', [...], true);
```

Otherwise it returns a collection of all the responses from all the events in the form of an array.

```php
// Multiple results, all events fired
$results = Event::fire('acme.blog.myevent', [...]);
```

<a name="event-pass-by-reference"></a>
## Passing arguments by reference

When processing or filtering over a value passed to an event, you may prefix the variable with `&` to pass it by reference. This allows multiple listeners to manipulate the result and pass it to the next one.

```php
Event::fire('cms.processContent', [&$content]);
```

When listening for the event, the argument also needs to be declared with the `&` symbol in the closure definition. In the example below, the `$content` variable will have "AB" appended to the result.

```php
Event::listen('cms.processContent', function (&$content) {
    $content = $content . 'A';
});

Event::listen('cms.processContent', function (&$content) {
    $content = $content . 'B';
});
```

<a name="queued-events"></a>
### Queued events

Firing events can be deferred in [conjunction with queues](../services/queues). Use the `Event::queue` method to "queue" the event for firing but not fire it immediately.

```php
Event::queue('foo', [$user]);
```

You may use the `Event::flush` method to flush all queued events.

```php
Event::flush('foo');
```

<a name="using-classes-as-listeners"></a>
## Using classes as listeners

In some cases, you may wish to use a class to handle an event rather than a Closure. Class event listeners will be resolved out of the [Application IoC container](../services/application), providing you with the full power of dependency injection on your listeners.

<a name="event-class-method"></a>
### Subscribe to individual methods

The event class can be registered with the `Event::listen` method like any other, passing the class name as a string.

```php
Event::listen('auth.login', 'LoginHandler');
```

By default, the `handle` method on the `LoginHandler` class will be called:

```php
class LoginHandler
{
    public function handle($data)
    {
        // ...
    }
}
```

If you do not wish to use the default `handle` method, you may specify the method name that should be subscribed.

```php
Event::listen('auth.login', 'LoginHandler@onLogin');
```

<a name="event-class-subscribe"></a>
### Subscribe to entire class

Event subscribers are classes that may subscribe to multiple events from within the class itself. Subscribers should define a `subscribe` method, which will be passed an event dispatcher instance.

```php
class UserEventHandler
{
    /**
     * Handle user login events.
     */
    public function userLogin($event)
    {
        // ...
    }

    /**
     * Handle user logout events.
     */
    public function userLogout($event)
    {
        // ...
    }

    /**
     * Register the listeners for the subscriber.
     *
     * @param  Illuminate\Events\Dispatcher  $events
     * @return array
     */
    public function subscribe($events)
    {
        $events->listen('auth.login', 'UserEventHandler@userLogin');

        $events->listen('auth.logout', 'UserEventHandler@userLogout');
    }
}
```

Once the subscriber has been defined, it may be registered with the `Event::subscribe` method.

```php
Event::subscribe(new UserEventHandler);
```

You may also use the [Application IoC container](../services/application) to resolve your subscriber. To do so, simply pass the name of your subscriber to the `subscribe` method.

```php
Event::subscribe('UserEventHandler');
```

<a name="event-emitter-trait"></a>
## Event emitter trait

Sometimes you want to bind events to a single instance of an object. You may use an alternative event system by implementing the `Winter\Storm\Support\Traits\Emitter` trait inside your class.

```php
class UserManager
{
    use \Winter\Storm\Support\Traits\Emitter;
}
```

This trait provides a method to listen for events with `bindEvent`.

```php
$manager = new UserManager;
$manager->bindEvent('user.beforeRegister', function ($user) {
    // Check if the $user is a spammer
});
```

> **NOTE**: Wildcard listeners are currently not supported for this trait.

The `fireEvent` method is used to fire events.

```php
$manager = new UserManager;
$manager->fireEvent('user.beforeRegister', [$user]);
```

These events will only occur on the local object as opposed to globally.

