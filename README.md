analytics.js
============

Every project needs analytics, regardless of which provider you use. **analytics.js** is an abstraction layer for your analytics providers, so you aren't littering your codebase with provider-specific calls. With **analytics.js** you can swap new analytics services in and out in seconds.


## Philosophy

We think every application needs analytics. The more data you collect about how your system is being used, the better your product decisions can be. In the end, your users will benefit.

But having analytics shouldn't mean your tying yourself to a single third-party analytics service. You shouldn't have to muck up your codebase with calls to their methods. And changing analytics services should be a snap.

That's where **analytics.js** comes in. The APIs for most analytics services track the same sorts of metrics, so it's not hard to build an abstraction layer that fits most use cases. And that's what we did! We even use **analytics.js** in [Segment.io](https://segment.io) itself.

The API is dead simple. Once you start using it, you won't want to go back to the nastier of the analytics APIs out there!


## The API

Our goal for the API was to iron out the kinks that crop up in lots of third-party analytics services API's. Keep things simple!  With analytics you record two things: users and their actions.

### Identify
Identify is how you tie a user to their actions. You **identify** your user with the `userId` you recognize them by, which is usually an email. The API looks like this:

```javascript
analytics.identify(userId, traits);
```

+ `userId` is the ID you refer to your user by.
+ `traits` is an _optional_ dictionary of things you know about the user. Things like: `Subscription Plan`, `Friend Count`, `Age`, etc.

An example **identify** call might look like this:

```javascript
analytics.identify('achilles@segment.io', {
    subscriptionPlan : 'Gold',
    friendCount      : 29
});
```

### Track
Track is how you record events your users trigger. Whenever your using does something you want to record, **track** that event. The API looks like this:

```javascript
analytics.track(event, properties);
```

+ `event` is the name of the event.
+ `properties` is an _optional_ dictionary of properties for the event. If the event was `Added to Shopping Cart`, it might have properties like `Price`, `Product Category`, etc.

An example **track** might look like this:

```javascript
analytics.track('Complete Purchase', {
    price          : 40.20,
    shippingMethod : '2-day'
});
```


## How to Use

### Add analytics.js to your web app

Analytics.js is more of a pattern than a library, but feel free to drop
analytics.js into your web application. You can then use the global
window.analytics object in your web application to identify and track.

### Choose your providers

Simply copy the providers you want from the providers/ folder into your version
of analytics.js. Make sure you add your application specific settings to each provider,
such as apiKey.

Also, add the provider implementation to the list of enabled providers on the bottom
of analytics.js:

```javascript

    var analytics = {

        /**
         * Determines whether analytics is enabled in this session
         * @type {Boolean}
         */
        enabled: true,

        //
        // ADD PROVIDERS HERE
        //

        providers: [

            GOOGLE_ANALYTICS,
            SEGMENT_IO,
            KISSMETRICS,
            MIXPANEL,
            INTERCOM_IO

        ]
    };

```


### Add identifies and tracks in your app code

After you determine what traits and actions you want to track, add identify
statements where you have access to the visitor object. Then, add
track statement whenever the visitor performs actions that are important to you.

If applicable, we recommend tracking actions such as:

* Visitor logging in
* Visitor buying something
* Visitor cancelling a plan
* Visitor unsubscribing
* Visitor upgrading a plan
* VIsitor sharing on social service

as well as any actions that show whether a visitor is engaged versus not. If you're
YouTube this would be "watched video", or if you're Amazon, it would be "bought an item".



## Dealing with Provider Inconsistencies

Google Analytics and Segment.io both track page loads automatically, while
Mixpanel does not. Intercom.io only does identifies, etc.

You'll want to make sure that you understand what each provider does to make
sure you have all your bases covered.



## Implementing New Providers

Implementing new providers is fairly painless. Check the files in the providers/
folder for examples, and send us a pull request once you're done.



