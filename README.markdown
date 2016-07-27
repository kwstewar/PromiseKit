![PromiseKit](http://promisekit.org/public/img/logo-500.png)

Modern development is highly asynchronous: isn’t it about time we had tools that made programming asynchronously powerful, easy and delightful?

```swift
firstly {
    UIApplication.shared().networkActivityIndicatorVisible = true
}.then {
    when(fetchImage(), getLocation())
}.then { image, location in
    self.imageView.image = image;
    self.label.text = "Buy your cat a house in \(location)"
}.always {
    UIApplication.shared().networkActivityIndicatorVisible = false
}.catch { error in
    UIAlertView(/*…*/).show()
}
```

PromiseKit is a thoughtful and complete implementation of promises for iOS and macOS with first-class support for **both** Objective-C *and* Swift.

![](https://img.shields.io/cocoapods/v/PromiseKit.svg?label=Current%20Release)  [![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg)](https://github.com/Carthage/Carthage)
[![codebeat](https://codebeat.co/badges/6a2fc7b4-cc8f-4865-a81d-644edd38c662)](https://codebeat.co/projects/github-com-mxcl-promisekit)
[![Build Status](https://travis-ci.org/mxcl/PromiseKit.svg?branch=master)](https://travis-ci.org/mxcl/PromiseKit)

# Getting Set Up

```ruby
#CocoaPods
pod "PromiseKit", "~> 4.0"

#Carthage
github "mxcl/PromiseKit" ~> 4.0
```

Alternatively, clone PromiseKit and drag and drop its `xcodeproj` into your Xcode project.

## Documentation

* We have a gentle but thorough introduction to PromiseKit at [promisekit.org](http://promisekit.org/introduction/).

# PromiseKit vs. Xcode

PromiseKit contains Swift, so we engage in an unending battle with Xcode:

| Xcode | Swift | PromiseKit | Release Notes |
| ----- | ----- | ---------- | ------------- |
|   8   |  3.0  |      4     | [TODO](http://promisekit.org/news/) |
|   8   |  2.3  |      3     |   |
|   7   |  2.2  |      3     |   |
|   6   |  1.2  |      2     | [2015/05](http://localhost:4000/news/2015/05/PromiseKit-2.0-Released/) |
|   *   |  N/A  |      1     |   |

PromiseKit 1 is pure Objective-C and thus works with all Xcodes, it is also your only choice if you need to support iOS 7 or below.

---

We also maintain some branches to aid migrating between Swift versions:

| Xcode | Swift | PromiseKit | Branch |
| ----- | ----- | -----------| ---------------- |
|  7.3  |  2.2  | 2 | swift-2.2-minimal-changes |
|  7.2  |  2.2  | 2 | swift-2.2-minimal-changes |
|  7.1  |  2.1  | 2 | swift-2.0-minimal-changes |
|  7.0  |  2.0  | 2 | swift-2.0-minimal-changes |

We mostly do not backport fixes to these migration-branches.

# Support

PromiseKit is lucky enough to have a large community behind it which is reflected in our [Gitter chat](https://gitter.im/mxcl/PromiseKit). If you’re new to PromiseKit and are stumped or otherwise have a question that doesn't feel like an issue (and isn't answered in our [FAQ](http://promisekit.org/FAQ)) then our Gitter is a great place to go for help. Of course if you're onto something that you believe is broken or could be improved then opening a new issue is still the way to go 👍.

# License

Copyright 2015, Max Howell; <mxcl@me.com>

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
