本专题主要解决 Swift 5.5 增加的 Concurrency 功能的学习，首要问题就是要明白[Swift Concurrency 是什么](https://github.com/KwaiAppTeam/SwiftPamphletApp/issues/63)。

推荐从官方文档学起，对接口使用能有些概念，接着从提案清单去了解，为什么提出这个功能？解决了什么问题？最终采取的方案为什么是这样做？

最后，可以看看别人对这个功能的理解，以及在实际场景下的实践经验。

## 1. 官方资料

**Concurrency 章节文档：**

- [官方英文文档](https://docs.swift.org/swift-book/LanguageGuide/Concurrency.html)

- [中文翻译文档](https://swiftgg.gitbook.io/swift/swift-jiao-cheng/28_concurrency)

**WWDC 2021 视频:**

- [Meet async/await in Swift](https://developer.apple.com/videos/play/wwdc2021/10132/) (WWDC21)
- [Explore structured concurrency in Swift](https://developer.apple.com/videos/play/wwdc2021/10134/) (WWDC21)
- [Swift concurrency: Update a sample app](https://developer.apple.com/videos/play/wwdc2021/10194/) (WWDC21)
- [Protect mutable state with Swift actors](https://developer.apple.com/videos/play/wwdc2021/10133/) (WWDC21)
- [Swift concurrency: Behind the scenes](https://developer.apple.com/videos/play/wwdc2021/10254/) (WWDC21)

**所有相关提案清单如下：**

- [SE-0296: Async/await](https://github.com/apple/swift-evolution/blob/main/proposals/0296-async-await.md) [【译】SE-0296 Async/await](https://kemchenj.github.io/2021-03-06/)
- [SE-0317: async let](https://github.com/apple/swift-evolution/blob/main/proposals/0317-async-let.md)
- [SE-0300: Continuations for interfacing async tasks with synchronous code](https://github.com/apple/swift-evolution/blob/main/proposals/0300-continuation.md) [【译】SE-0300 Continuation – 执行同步代码的异步任务接口](https://kemchenj.github.io/2021-03-31/)
- [SE-0302: Sendable and @Sendable closures](https://github.com/apple/swift-evolution/blob/main/proposals/0302-concurrent-value-and-concurrent-closures.md)
- [SE-0298: Async/Await: Sequences](https://github.com/apple/swift-evolution/blob/main/proposals/0298-asyncsequence.md) [【译】SE-0298 Async/Await 序列](https://kemchenj.github.io/2021-03-10/)
- [SE-0304: Structured concurrency](https://github.com/apple/swift-evolution/blob/main/proposals/0304-structured-concurrency.md)
- [SE-0306: Actors](https://github.com/apple/swift-evolution/blob/main/proposals/0306-actors.md) [【译】SE-0306 Actors](https://kemchenj.github.io/2021-04-25/)
- [SE-0313: Improved control over actor isolation](https://github.com/apple/swift-evolution/blob/main/proposals/0313-actor-isolation-control.md)
- [SE-0297: Concurrency Interoperability with Objective-C](https://github.com/apple/swift-evolution/blob/main/proposals/0297-concurrency-objc.md) [【译】SE-0297 Concurrency 与 Objective-C 的交互](https://kemchenj.github.io/2021-03-07/)
- [SE-0314: AsyncStream and AsyncThrowingStream](https://github.com/apple/swift-evolution/blob/main/proposals/0314-async-stream.md)
- [SE-0316: Global actors](https://github.com/apple/swift-evolution/blob/main/proposals/0316-global-actors.md)
- [SE-0310: Effectful read-only properties](https://github.com/apple/swift-evolution/blob/main/proposals/0310-effectful-readonly-properties.md)
- [SE-0311: Task Local Values](https://github.com/apple/swift-evolution/blob/main/proposals/0311-task-locals.md)
- [Custom Executors](https://forums.swift.org/t/support-custom-executors-in-swift-concurrency/44425)

## 2. 闲话 Swift 协程

- [闲话 Swift 协程（0）：前言](https://www.bennyhuo.com/2021/10/11/swift-coroutines-README/)
- [闲话 Swift 协程（1）：Swift 协程长什么样？](https://www.bennyhuo.com/2021/10/11/swift-coroutines-01-intro/)
- [闲话 Swift 协程（2）：将回调改写成 async 函数](https://www.bennyhuo.com/2021/10/13/swift-coroutines-02-wrap-callback/)
- [闲话 Swift 协程（3）：在程序当中调用异步函数](https://www.bennyhuo.com/2022/01/21/swift-coroutines-03-call-async-func/)
- [闲话 Swift 协程（4）：TaskGroup 与结构化并发](https://www.bennyhuo.com/2022/01/22/swift-coroutines-04-structured-concurrency/)
- [闲话 Swift 协程（5）：Task 的取消](https://www.bennyhuo.com/2022/01/28/swift-coroutines-05-cancellation/)
- [闲话 Swift 协程（6）：Actor 和属性隔离](https://www.bennyhuo.com/2022/02/12/swift-coroutines-06-actor/)
- [闲话 Swift 协程（7）：GlobalActor 和异步函数的调度](https://www.bennyhuo.com/2022/02/12/swift-coroutines-07-globalactor/)
- [闲话 Swift 协程（8）：TaskLocal](https://www.bennyhuo.com/2022/02/12/swift-coroutines-08-tasklocal/)
- [闲话 Swift 协程（9）：异步函数与其他语言的互调用](https://www.bennyhuo.com/2022/02/16/swift-coroutines-09-interop/)

## 3. [Functional core Imperative shell in Swift. Unidirectional Flow](https://swiftwithmajid.com/2022/03/16/functional-core-imperative-shell-in-swift-unidirectional-flow)

函数式的核心，命令式的壳，单项数据流。

单项数据流可以看作者的这个系列:

1. [Redux-like state container in SwiftUI. Basics](https://swiftwithmajid.com/2019/09/18/redux-like-state-container-in-swiftui/)
2. [Redux-like state container in SwiftUI. Best practices](https://swiftwithmajid.com/2019/09/25/redux-like-state-container-in-swiftui-part2/)
3. [Redux-like state container in SwiftUI. Container Views.](https://swiftwithmajid.com/2019/10/02/redux-like-state-container-in-swiftui-part3/)
4. [Redux-like state container in SwiftUI. Connectors.](https://swiftwithmajid.com/2021/02/03/redux-like-state-container-in-swiftui-part4/)
5. [Redux-like state container in SwiftUI. Swift concurrency model.](https://swiftwithmajid.com/2022/02/17/redux-like-state-container-in-swiftui-part5/)

## 4. [Asynchronous programming with SwiftUI and Combine](https://peterfriese.dev/posts/combine-vs-async)

有了 Combine，还有必要存在 async/await 吗？

## 5. [Async HTTP API clients in Swift](https://theswiftdev.com/async-http-api-clients-in-swift/)

异步 HTTP 请求封装的实践

## 6. [Web API Client in Swift](https://kean.blog/post/new-api-client)

有了 Async/Await 和 Actors 的特性，现在在 Swift 中设计自定义 Web API 相比以往可以通过少量的抽象，使代码易于理解和扩展，文中进行了一些实践演示应用。

## 7. Modern Swift Concurrency from Andy Ibanez

作者 Andy Ibanez，来自玻利维亚，10+年 iOS 开发。

本系列文章是作者对 iOS 现代并发的理解。

1. [Modern Concurrency in Swift: Introduction](https://www.andyibanez.com/posts/modern-concurrency-in-swift-introduction/)
2. [Understanding async/await in Swift](https://www.andyibanez.com/posts/understanding-async-await-in-swift/)
3. [Converting closure-based code into async/await in Swift](https://www.andyibanez.com/posts/converting-closure-based-code-into-async-await-in-swift/)
4. [Structured Concurrency in Swift: Using async let](https://www.andyibanez.com/posts/structured-concurrency-in-swift-using-async-let/)
5. [Structured Concurrency With Group Tasks in Swift](https://www.andyibanez.com/posts/structured-concurrency-with-group-tasks-in-swift/)
6. [Introduction to Unstructured Concurrency in Swift](https://www.andyibanez.com/posts/introduction-to-unstructured-concurrency-in-swift/)
7. [Unstructured Concurrency With Detached Tasks in Swift](https://www.andyibanez.com/posts/unstructured-concurrency-with-detached-tasks-in-swift/)
8. [Understanding Actors in the New Concurrency Model in Swift](https://www.andyibanez.com/posts/understanding-actors-in-the-new-concurrency-model-in-swift/)
9. [@MainActor and Global Actors in Swift](https://www.andyibanez.com/posts/mainactor-and-global-actors-in-swift/)
10. [Sharing Data Across Tasks with the @TaskLocal property wrapper in the new Swift Concurrency Model](https://www.andyibanez.com/posts/modern-swift-concurrency-summary-cheatsheet-thanks/posts/sharing-data-across-tasks-tasklocal-new-swift-concurrency-model)
11. [Using AsyncSequence in Swift](https://www.andyibanez.com/posts/using-asyncsequence-in-swift/)
12. [Modern Swift Concurrency Summary, Cheatsheet, and Thanks](https://www.andyibanez.com/posts/modern-swift-concurrency-summary-cheatsheet-thanks/)

## 8. [Sundell 关于 Swift Concurrency 的文章合集](https://www.swiftbysundell.com/discover/concurrency/)

作者 [Sundell](https://www.swiftbysundell.com/) 是一个非常勤快的 Swift 开发人员，经常在个人网站上分享关于 Swift 开发的方方面面，如果了解 Swift 的更多隐藏东西，可以关注他的网站。

## 9. Hacking With Swift 教程

- [Swift Concurrency by Example](https://www.hackingwithswift.com/quick-start/concurrency/)

## 10. [Concurrency by Tutorials](https://www.raywenderlich.com/books/concurrency-by-tutorials)

本电子书内容主要包括传统的并发模式，GCD、Operation 等，可以复习一遍来对照 Modern Concurrency 的用法。

## 11. [Modern Concurrency in Swift](https://www.raywenderlich.com/28971664-announcing-modern-concurrency-in-swift-first-edition)

Learn everything you need to create safe, performant and predictable asynchronous apps by leveraging the new modern concurrency features introduced in Swift 5.5, such as async/await, tasks, actors and more.

## 12. [Async / Await in Swift](https://jayeshkawli.ghost.io/async-await-in-swift/)

## 13. 『 Swift 新并发框架 』系列文章

- [Swift 新并发框架之 async/await](https://juejin.cn/post/7076733264798416926)
- [Swift 新并发框架之 actor](https://juejin.cn/post/7076738494869012494)
- [Swift 新并发框架之 Sendable](https://juejin.cn/post/7076741945820872717/)
- [Swift 新并发框架之 Task](https://juejin.cn/post/7084640887250092062/)

## 14. [喵神的 Swift 并发初步](https://onevcat.com/2021/07/swift-concurrency/)
