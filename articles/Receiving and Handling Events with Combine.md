# <i><span style="color: #C0C0C0">Receiving and Handling Events with Combine</span></i>    
# 컴바인을 활용해 이벤트를 받고, 다루어 봅시다.

<i><span style="color: #C0C0C0">Customize and receive events from asynchronous sources.</span></i>    
비동기 소스로부터 이벤트를 받고 조작해봅시다.

---

## <i><span style="color: #C0C0C0">Overview </span></i>
## 개요

<i><span style="color: #C0C0C0">The Combine framework provides a declarative approach for how your app processes events. Rather than potentially implementing multiple delegate callbacks or completion handler closures, you can create a single processing chain for a given event source. Each part of the chain is a Combine operator that performs a distinct action on the elements received from the previous step. </span></i>    
Combine 프레임워크는 앱이 이벤트를 처리하는 방식에 대한 선언적 접근방식을 제공합니다. 당신은 잠재적으로 여러 딜리게이트 콜백이나, 컴플리션 핸들러를 구현하는 대신, 지정된 이벤트 소스에 대한 단일 처리 체인을 만들 수 있습니다. 체인의 각 부분은 이전 단계에서 받은 요소에 대해 고유한 작업을 진행하는 Combine의 연산자입니다.

<i><span style="color: #C0C0C0"> Consider an app that needs to filter a table or collection view based on the contents of a text field. In AppKit, each keystroke in the text field produces a [`Notification`](https://developer.apple.com/documentation/foundation/notification) that you can subscribe to with Combine. After receiving the notification, you can use operators to change the content and timing of event delivery, and use the final result to update your app’s user interface. </span></i>    

텍스트 필드의 내용을 바탕으로 테이블이나 컬렉션 뷰를 필터링해야하는 앱을 고려해봅시다. AppKit 에서, 텍스트 필드의 각 키에 대한 입력은 컴바인을 통해 구동할 수 있는 [`Notification`](https://developer.apple.com/documentation/foundation/notification)을 만들어냅니다. Notification을 받은 이후에, 당신은 연산자를 사용하여 전달된 이벤트의 내용과 시간을 변경하거나, 최종 결과물을 토대로 앱의 UI를 업데이트할 수 있습니다. 

<br>

## <i><span style="color: #C0C0C0">Connext a Publisher to a Subscriber.</span></i>
## Publisher(게시자) 와 Subscriber(구독자) 를 연결해봅시다.

<i><span style="color: #C0C0C0">To receive the text field’s notifications with Combine, access the default instance of [`NotificationCenter`](https://developer.apple.com/documentation/foundation/notificationcenter) and call its [`publisher(for:object:)`](https://developer.apple.com/documentation/foundation/notificationcenter/3329353-publisher) method. This call takes the notification name and source object that you want notifications from, and returns a publisher that produces notification elements.</span></i>    
Combine으로 텍스트필드의 알림을 전달받기 위해선,[`NotificationCenter`](https://developer.apple.com/documentation/foundation/notificationcenter) 의 default 인스턴스에 접근하여, [`publisher(for:object:)`](https://developer.apple.com/documentation/foundation/notificationcenter/3329353-publisher) 메서드를 호출해야합니다. 이 호출은 알림을 받을 이름과 소스 객체를 사용하고, 알림 요소를 생성해내는 publisher 를 반환합니다.

```swift
let pub = NotificationCenter.default    
		.publisher(for: NSControl.textDidChangeNotification, object: filterField)
```

<i><span style="color: #C0C0C0">You use a [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) to receive elements from the publisher. The subscriber defines an associated type, [`Input`](https://developer.apple.com/documentation/combine/subscriber/input), to declare the type that it receives. The publisher also defines a type, [`Output`](https://developer.apple.com/documentation/combine/publisher/output), to declare what it produces. The publisher and subscriber both define a type, [`Failure`](https://developer.apple.com/documentation/combine/publisher/failure), to indicate the kind of error they produce or receive. To connect a subscriber to a producer, the [`Output`](https://developer.apple.com/documentation/combine/publisher/output) must match the [`Input`](https://developer.apple.com/documentation/combine/subscriber/input), and the [`Failure`](https://developer.apple.com/documentation/combine/publisher/failure) types must also match.</span></i>    
[`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) 를 활용하여 publisher 로부터 요소들을 전달받을 수 있습니다. Subscriber 는  [`Input`](https://developer.apple.com/documentation/combine/subscriber/input) 이라는 전달받을 연관 타입을 정의합니다. Publisher  역시 [`Output`](https://developer.apple.com/documentation/combine/publisher/output)이라는 만들어낼 연관 타입을 정의합니다. Publisher 와 subscriber 는 모두 생성하거나, 수신할 때 발생하는 문제 나타내는 [`Failure`](https://developer.apple.com/documentation/combine/publisher/failure)라는 타입을 정의합니다. Subscriber 와 publisher를 서로 매칭하기 위해서는, [`Output`](https://developer.apple.com/documentation/combine/publisher/output)과 [`Input`](https://developer.apple.com/documentation/combine/subscriber/input) 타입 그리고 각자의 [`Failure`](https://developer.apple.com/documentation/combine/publisher/failure) 타입이 서로 같아야합니다.

<i><span style="color: #C0C0C0">Combine provides two built-in subscribers, which automatically match the output and failure types of their attached publisher:</span></i>    
Combine은 output 과 failure 타입을 자동으로 매칭하는 두 개의 Subscriber 를 기본적으로 제공합니다. 

- <i><span style="color: #C0C0C0">[`sink(receiveCompletion:receiveValue:)`](https://developer.apple.com/documentation/combine/publisher/sink(receivecompletion:receivevalue:)) takes two closures. The first closure executes when it receives [`Subscribers.Completion`](https://developer.apple.com/documentation/combine/subscribers/completion), which is an enumeration that indicates whether the publisher finished normally or failed with an error. The second closure executes when it receives an element from the publisher.</span></i>   
    [`sink(receiveCompletion:receiveValue:)`](https://developer.apple.com/documentation/combine/publisher/sink(receivecompletion:receivevalue:)) 는 두 개의 클로저를 갖습니다. 첫 번째 클로저는 [`Subscribers.Completion`](https://developer.apple.com/documentation/combine/subscribers/completion) 을 수신할 때 실행되며, 이는 Publisher 가 정상적으로 작업을 마무리 지었는지, 혹은 에러와 함께 실패했는지를 드러내는 열거형입니다. 두 번째 클로저는 Publisher 로 부터 요소를 수신할 때 실행됩니다.
- <i><span style="color: #C0C0C0">[`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) immediately assigns every element it receives to a property of a given object, using a key path to indicate the property.</span></i>   
    [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) 은 주어진 객체의 프로퍼티에 전달받은 모든 요소를 즉각적으로 할당합니다. 이 때, 키패스를 사용해 프로퍼티를 가리킵니다.

<i><span style="color: #C0C0C0">For example, you can use the sink subscriber to log when the publisher completes, and each time it receives an element:</span></i>   
예를 들어, publisher 가 작업을 마쳤을 때, 그리고 요소를 수신받는 각각의 시점에 sink subscriber 를 사용할 수 있습니다.

```swift
let sub = NotificationCenter.default    
		.publisher(for: NSControl.textDidChangeNotification, object: filterField)    
		.sink(receiveCompletion: { print ($0) },          
					receiveValue: { print ($0) })
```

<i><span style="color: #C0C0C0">Both the `sink(receiveCompletion:receiveValue:)` and [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) subscribers request an unlimited number of elements from their publishers. To control the rate at which you receive elements, create your own subscriber by implementing the [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) protocol.</span></i>   
`sink(receiveCompletion:receiveValue:)` 와 [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) subscriber는 둘 다 publisher로 부터 요소들을 제약없이 전달받습니다. 전달받을 요소들을 제어하기 위해선, 직접 [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) 프로토콜을 채택해 자신만의 subscriber를 만들 수 있습니다.

<br>

## <i><span style="color: #C0C0C0">Change the Output Type with Operators</span></i>
## Operator를 활용해 Output 타입을 변경해봅시다.

<i><span style="color: #C0C0C0">The sink subscriber in the previous section performs all its work in the `receiveValue` closure. This could be burdensome if it needs to perform a lot of custom work with received elements or maintain state between invocations. The advantage of Combine comes from combining operators to customize event delivery.</span></i>    
이전 섹션에서 다루었던 `sink` subscriber는 `receiveValue`클로저에서 모든 작업을 수행합니다.  만약 전달받은 요소를 바탕으로 많은 사용자 정의 작업을 수행해야하거나, 호출들 사이에서 상태를 유지해야하는 상황이라면, 이는 부담이 될 수도 있습니다.

<i><span style="color: #C0C0C0">For example, [`NotificationCenter.Publisher.Output`](https://developer.apple.com/documentation/foundation/notificationcenter/publisher/output) isn’t a convenient type to receive in the callback if all you need is the text field’s string value. Since a publisher’s output is essentially a sequence of elements over time, Combine offers sequence-modifying operators like [`map(_:)`](https://developer.apple.com/documentation/combine/publisher/map(_:)-99evh), [`flatMap(maxPublishers:_:)`](https://developer.apple.com/documentation/combine/publisher/flatmap(maxpublishers:_:)-3k7z5), and [`reduce(_:_:)`](https://developer.apple.com/documentation/combine/publisher/reduce(_:_:)). The behavior of these operators is similar to their equivalents in the Swift standard library.</span></i>    
예를들어, [`NotificationCenter.Publisher.Output`](https://developer.apple.com/documentation/foundation/notificationcenter/publisher/output) 과 같은 타입은 텍스트 필드의 문자열 값만을 필요로 하는 경우, 콜백을 통해 전달받기에 편리하한 타입이 아닙니다. Publisher의 output 은 결국 시간에 따른 일련의 요소이므로, Combine 은 [`map(_:)`](https://developer.apple.com/documentation/combine/publisher/map(_:)-99evh), [`flatMap(maxPublishers:_:)`](https://developer.apple.com/documentation/combine/publisher/flatmap(maxpublishers:_:)-3k7z5),  [`reduce(_:_:)`](https://developer.apple.com/documentation/combine/publisher/reduce(_:_:))와 같이 순서를 조정하는 연산자를 제공합니다. 이러한 연산자들의 동작은 Swift standard library 의 그것과 비슷합니다. 

<i><span style="color: #C0C0C0">To change the output type of the publisher, you add a [`map(_:)`](https://developer.apple.com/documentation/combine/publisher/map(_:)-99evh)operator whose closure returns a different type. In this case, you can get the notification’s object as an [`NSTextField`](https://developer.apple.com/documentation/appkit/nstextfield), and then get the field’s [`stringValue`](https://developer.apple.com/documentation/appkit/nscontrol/1428950-stringvalue).</span></i>    
Publisher 의 Output 타입을 변경하기 위해서는, 다른 타입을 반환하는 클로저를 갖는 [`map(_:)`](https://developer.apple.com/documentation/combine/publisher/map(_:)-99evh) 연산자를 추가해야합니다. 텍스트 필드의 경우, Notification의 객체를 [`NSTextField`](https://developer.apple.com/documentation/appkit/nstextfield) 타입으로 가져온 후, [`stringValue`](https://developer.apple.com/documentation/appkit/nscontrol/1428950-stringvalue) 를 꺼내올 수 있습니다.

```swift
let sub = NotificationCenter.default    
    .publisher(for: NSControl.textDidChangeNotification, object: filterField)    
    .map( { ($0.object as! NSTextField).stringValue } )    
    .sink(receiveCompletion: { print ($0) },          
    			receiveValue: { print ($0) })
```

<i><span style="color: #C0C0C0">After the publisher chain produces the type you want, replace `sink(receiveCompletion:receiveValue:)` with [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)). The following example takes the strings it receives from the publisher chain and assigns them to the `filterString` of a custom view model object:</span></i>    
Publisher 체인은 우리가 원하는 타입을 만들어낸 이후에, `sink(receiveCompletion:receiveValue:)` 를 [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) 로 대체할 수 있습니다. 아래의 예시는 Publisher 체인으로부터 문자열을 받고, 이를 view model 객체의 `filterString` 에 할당하고 있습니다. 

```swift
let sub = NotificationCenter.default    
		.publisher(for: NSControl.textDidChangeNotification, object: filterField)    
		.map( { ($0.object as! NSTextField).stringValue } )    
		.assign(to: \MyViewModel.filterString, on: myViewModel)
```

<br>

## <i><span style="color: #C0C0C0">Customize Publishers with Operators</span></i>
## 연산자를 활용해 Publisher를 사용자정의 해봅시다.

<i><span style="color: #C0C0C0">You can extend the [`Publisher`](https://developer.apple.com/documentation/combine/publisher) instance with an operator that performs actions that you’d otherwise need to code manually. Here are three ways you could use operators to improve this event-processing chain:</span></i>    
기존이라면 우리가 수동으로 코딩해야하는 작업들을, [`Publisher`](https://developer.apple.com/documentation/combine/publisher) 인스턴스에 연산자를 활용해 확장할 수 있습니다.

- <i><span style="color: #C0C0C0">Rather than updating the view model with any string typed into the text field, you could use the [`filter(_:)`](https://developer.apple.com/documentation/combine/publisher/filter(_:)) operator to ignore input under a certain length or to reject non-alphanumeric characters.</span></i>    
    텍스트 필드에 입력된 문자열로 view model을 업데이트하는 대신, [`filter(_:)`](https://developer.apple.com/documentation/combine/publisher/filter(_:)) 연산자를 활용해 특정 길이 미만의 입력값을 무시하거나, 영문/숫자가 아닌 문자를 거부할 수 있습니다.
- <i><span style="color: #C0C0C0">If the filtering operation is expensive — for example, if it’s querying a large database — you might want to wait for the user to stop typing. For this, the [`debounce(for:scheduler:options:)`](https://developer.apple.com/documentation/combine/publisher/debounce(for:scheduler:options:)) operator lets you set a minimum period of time that must elapse before a publisher emits an event. The [`RunLoop`](https://developer.apple.com/documentation/foundation/runloop) class provides conveniences for specifying the time delay in seconds or milliseconds.</span></i>    
    변별 작업이 오래 걸리는 경우 (예를 들어, 대규모의 데이터베이스를 선별하는 경우) 사용자가 타이핑을 멈출 때까지 잠시 기다리기를 원하기도 합니다. [`debounce(for:scheduler:options:)`](https://developer.apple.com/documentation/combine/publisher/debounce(for:scheduler:options:)) 연산자를 활용하면, Publisher가 이벤트를 발생시키기 전에 경과해야하는 최고 시간을 설정할 수 있습니다.  [`RunLoop`](https://developer.apple.com/documentation/foundation/runloop) 클래스는 초단위 혹은 밀리초단위의 지연 시간을 쉽게 지정할 수 있도록 돕습니다.
- <i><span style="color: #C0C0C0">If the results update the UI, you can deliver callbacks to the main thread by calling the [`receive(on:options:)`](https://developer.apple.com/documentation/combine/publisher/receive(on:options:)) method. By specifying the [`Scheduler`](https://developer.apple.com/documentation/combine/scheduler) instance provided by the [`RunLoop`](https://developer.apple.com/documentation/foundation/runloop) class as the first parameter, you tell Combine to call your subscriber on the main run loop.</span></i>   
    만약 결과물이 UI를 업데이트해야한다면, [`receive(on:options:)`](https://developer.apple.com/documentation/combine/publisher/receive(on:options:)) 메서드를 호출해 콜백함수를 메인 스레드로 옮길 수 있습니다. [`RunLoop`](https://developer.apple.com/documentation/foundation/runloop) 클래스에서 제공하는  [`Scheduler`](https://developer.apple.com/documentation/combine/scheduler) 인스턴스를 첫번째 매개변수로 지정함으로써, subscriber를 메인 런루프에서 호출하도록 지시할 수 있습니다.

<i><span style="color: #C0C0C0">The resulting publisher declaration follows:</span></i>    
선언된 Publisher는 다음과 같습니다.

```swift
let sub = NotificationCenter.default    
		.publisher(for: NSControl.textDidChangeNotification, object: filterField)    
		.map( { ($0.object as! NSTextField).stringValue } )    
		.filter( { $0.unicodeScalars.allSatisfy({CharacterSet.alphanumerics.contains($0)}) } )    
		.debounce(for: .milliseconds(500), scheduler: RunLoop.main)    
		.receive(on: RunLoop.main)    
		.assign(to:\MyViewModel.filterString, on: myViewModel)
```

<br>

## <i><span style="color: #C0C0C0">Cancel Publishing when Desired</span></i>
## 원하는 시점에 발행을 취소해봅시다.

<i><span style="color: #C0C0C0">A publisher continues to emit elements until it completes normally or fails. If you no longer want to subscribe to the publisher, you can cancel the subscription. The subscriber types created by `sink(receiveCompletion:receiveValue:)` and [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) both implement the [`Cancellable`](https://developer.apple.com/documentation/combine/cancellable) protocol, which provides a [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) method:</span></i>    
Publisher 는 작업이 완료되거나, 실패할때까지 요소들을 계속적으로 내보냅니다. 만약, 더이상 Publisher를 구독하고 싶지 않다면, 구독을 취소할 수 있습니다. Subscriber 타입들은 [`Cancellable`](https://developer.apple.com/documentation/combine/cancellable) 프로토콜을 구현하는  `sink(receiveCompletion:receiveValue:)` 나 [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:))  메서드에 의해 생성됩니다. Cancellable 프로토콜은  [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) 메서드를 제공하고 있습니다.

```swift
sub?.cancel()
```

<i><span style="color: #C0C0C0">If you create a custom [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber), the publisher sends a [`Subscription`](https://developer.apple.com/documentation/combine/subscription) object when you first subscribe to it. Store this subscription, and then call its [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) method when you want to cancel publishing. When you create a custom subscriber, you should implement the [`Cancellable`](https://developer.apple.com/documentation/combine/cancellable) protocol, and have your [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) implementation forward the call to the stored subscription.</span></i>    
만약 당신이 [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) 를 직접 생성한다면, Publisher는 자신이 처음 구독되는 시점에 [`Subscription`](https://developer.apple.com/documentation/combine/subscription) 객체를 전달합니다. 전달된 `Subscription` 객체를 저장한 후, 발행을 취소하고 싶을 때 [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) 메서드를 호출해봅시다. 사용자 정의 Subscriber를 직접 생성할 때, 우리는 반드시 [`Cancellable`](https://developer.apple.com/documentation/combine/cancellable) 프로토콜을 채택해 구현해야합니다. 그리고 구현한 [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) 메서드가 저장된 subscription으로 호출을 전달하도록 해야합니다.
