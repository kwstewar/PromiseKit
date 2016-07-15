---
layout: docs
title: Fundamentals
nav: Docs
order: 2
redirect_from:
 - "/then"
 - "/chaining"
 - "/introduction"
 - "/error-handling"
 - "/when"
 - "/finally"
 - "/recovering-from-errors"
---

# The Fundamentals of Promises

A promise is an object that represents an asynchronous task. Pass that object around, and write clean, ordered code; a logical, simple, modular stream of progression from one asynchronous task to another.

`then` is like a completion-handler, but for promises:

```swift
PHPhotoLibrary.requestAuthorization().then {
    //… user said ok!
}
```

Promises separate error handling from your main code path; just like Swift:

```swift
PHPhotoLibrary.requestAuthorization().then {
    //… user said ok!
}.catch { error in
    //… user said no!
}
```

Which is particularly useful because errors jump to the `catch`; just like swift:

```swift
PHPhotoLibrary.requestAuthorization().then {
    
    // returning a promise in a `then` handler waits on that
    // promise before continuing to the next handler
    return CLLocationManager.promise()
    
    // if the above promise fails, execution jumps to the catch below

}.then { location in
    
    // if anything throws in this handler execution also jumps to the `catch`

}.catch { error in
    switch error {
    case is PHPhotoLibrary.Error:
        //…
    case is CLLocation.Error:
        //…
    }
}
```

Promises standardize the interfaces of asynchronicity; the take home is things that are tedious and flakey with completion-handlers are delightful with promises:

```swift
let auth = PHPhotoLibrary.requestAuthorization()
let locater = CLLocationManager.promise()

when(fulfilled: auth, locater).then { auth, location in
   // both tasks suceeded!
}.catch { error in
    // at least one of the tasks failed
}
```

Asynchronous state machines can be nightmares, but less so with promises:

```swift
firstly {
    UIApplication.shared().networkActivityIndicatorVisible = true
    
    //NOTE: we must remember to unset the activity indicator when we are done!
    
}.then {
    when(fulfilled: PHPhotoLibrary.requestAuthorization(), CLLocationManager.promise())
}.then {
    //…
}.always {
    
    //NOTE: oh right, promises make this easy
    
    UIApplication.shared().networkActivityIndicatorVisible = false
}.catch { error in
    //…
}
```

Error handling in Cocoa is hard to do well, but promises make it easy, and they make what is normally tricky edge-case error handling easy too:

```swift
CLLocationManager.promise().recover { error in
    guard error == CLLocationError.NotFound else { throw error }
    return CLLocation.chicago
}.then { location in
    //…
}.catch { error in
    // errors OTHER than CLLocationError.NotFound
}
```

PromiseKit has the concept of “cancellation” baked in. If the user cancels something then typically you don't want to continue a promise chain, but you don't want to show an error message either. So what is cancellation? It's not success and it's not failure, but it should handle more like an error, ie. it should skip all the subsequent `then` handlers. PromiseKit embodies cancellation as a special kind of error:

```swift
firstly {
    UIApplication.shared().networkActivityIndicatorVisible = true
}.then {
    CLLocationManager.promise()
}.then { location in
    let alert = UIAlertView()
    alert.addButton(title: "Proceed!")
    alert.addButton(title: "Proceed, but fastest!")
    alert.cencelButtonIndex = alert.addButton("Cancel")
    return alert.promise()
}.then { dismissedButtonIndex in
    //… cancel wasn't pressed
}.always {
    UIApplication.shared().networkActivityIndicatorVisible = false
}.catch { error in
    //… cancel wasn't pressed
}
```

The above works like you want, if the user presses the cancel button in the alert view the `networkActivityIndicator` is unset but otherwise nothing happens.

If you need to handle cancellation however, you can:

```swift
NSURLSession.shared().promise(url: myUrl).then { data in
    // NSURLSession can be cancelled via the `cancel` method
}.catch(policy: .allErrors) { error in
    if error.isCancelledError {
        // something called “cancel” on the session object
    } else {
        // some other error occurred
    }
}
```

Because `then` is just a function you call on an object, you can call it multiple times:

```swift
let directionsPromise = MKDirections(/*…*/).promise()
directionsPromise.then { print(1) }
directionsPromise.then { print(2) }
```

The closures you pass are executed in order — once the directions have come back from Apple’s servers.
