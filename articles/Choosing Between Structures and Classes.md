# <i><span style="color: #C0C0C0">Choosing Between Structures and Classes</span></i>    
# 구조체와 클래스 중에 선택하기

<i><span style="color: #C0C0C0">Decide how to store data and model behavior.
</span></i>    
어떻게 데이터 저장하는지 및 어떻게 동작을 모델링하는지 결정합니다.

---

## <i><span style="color: #C0C0C0">Overview</span></i>
## 개요

<i><span style="color: #C0C0C0">Structures and classes are good choices for storing data and modeling behavior in your apps, but their similarities can make it difficult to choose one over the other.</span></i>    
구조체와 클래스는 앱에 데이터를 저장하고 동작을 모델링하는데 좋은 선택이지만, 유사한 부분들 때문에 둘 중 하나를 선택하는 게 어려울 수 있습니다.

<i><span style="color: #C0C0C0">Consider the following recommendations to help choose which option makes sense when adding a new data type to your app.
</span></i>    
앱에 새 데이터 타입을 추가할 때, 적합한 옵션을 선택하는 데 도움이 되는 아래의 추천을 고려하세요.
<i><span style="color: #C0C0C0">
* Use structures by default.</span></i>   
기본적으로 구조체를 사용하세요. 
<i><span style="color: #C0C0C0">
* Use classes when you need Objective-C interoperability.</span></i>    
Objective-C 코드와 상호작용을 호환할 때 클래스를 사용하세요. (*interoperability 상호운용성*)
<i><span style="color: #C0C0C0">
* Use classes when you need to control the identity of the data you're modeling.</span></i>    
모델링 중인 데이터의 독자성[¹](#1)을 제어해야 할 때 클래스를 사용하세요.
<i><span style="color: #C0C0C0">
* Use structures along with protocols to adopt behavior by sharing implementations.</span></i>    
구현을 공유함으로써 동작을 채택하는 프로토콜과 함께 구조체를 사용하기.


<br>

## <i><span style="color: #C0C0C0">Choose Structures by Default</span></i>
## 기본적으로 구조체 선택하기

<i><span style="color: #C0C0C0">Use structures to represent common kinds of data. Structures in Swift include many features that are limited to classes in other languages: They can include stored properties, computed properties, and methods. Moreover, Swift structures can adopt protocols to gain behavior through default implementations. The Swift standard library and Foundation use structures for types you use frequently, such as numbers, strings, arrays, and dictionaries.</span></i>    
구조체를 사용해서 공통 데이터 종류를 나타냅니다. 스위프트의 구조체는 다른 프로그래밍 언어에서 클래스로 제한되는 많은 기능을 포함합니다: 저장 프로퍼티, 계산 프로퍼티 및 메서드가 포함될 수 있습니다. 또한, 스위프트 구조체는 기본  구현을 통해 동작을 얻기 위해 프로토콜을 채택할 수 있습니다. 스위프트 표준 라이브러리 및 `Foundation`은 숫자, 문자열, 배열 및 딕셔너리와 같이 자주 사용하는 타입에 대해 구조체를 사용합니다.

<i><span style="color: #C0C0C0">Using structures makes it easier to reason about a portion of your code without needing to consider the whole state of your app. Because structures are value types—unlike classes—local changes to a structure aren't visible to the rest of your app unless you intentionally communicate those changes as part of the flow of your app. As a result, you can look at a section of code and be more confident that changes to instances in that section will be made explicitly, rather than being made invisibly from a tangentially related function call.</span></i>    
구조체를 사용하면 앱의 전체 상태를 고려할 필요 없이 코드 일부에 대해 추론하기가 더 쉬워집니다. 구조체는 클래스와 달리 값 타입입니다. 그러므로 앱 흐름의 일부로 지역[²](#2) 변경 사항을 의도적으로 전달하지 않는 한 나머지 앱에서는 알 수 없습니다. 따라서, 섹션의 인스턴스가 별로 관계 없는 함수 호출을 통해 보이지 않는 것보다는, 그 코드의 섹션을 보고 명시적으로 변경될 것이라고 좀 더 확신할 수 있습니다.

<br>

## <i><span style="color: #C0C0C0">Use Classes When You Need Objective-C Interoperability</span></i>
## Objective-C 코드와 상호작용을 호환할 때 클래스 사용하기 

<i><span style="color: #C0C0C0">If you use an Objective-C API that needs to process your data, or you need to fit your data model into an existing class hierarchy defined in an Objective-C framework, you might need to use classes and class inheritance to model your data. For example, many Objective-C frameworks expose classes that you are expected to subclass.</span></i>    
데이터를 처리해야 하는 Objective-C API를 사용하거나, Objective-C 프레임워크에 정의된 기존 클래스 계층에 데이터 모델을 맞춰야 하는 경우에,
데이터를 모델링하려면 클래스 및 클래스 상속 사용을 필요로 할 수 있습니다. 예를 들어, 많은 Objective-C 프레임워크는 하위 클래스로 예상되는 클래스를 표시합니다.

<br>

## <i><span style="color: #C0C0C0">Use Classes When You Need to Control Identity</span></i>
## 독자성[¹](#1)을 제어해야 할 때 클래스 사용하기

<i><span style="color: #C0C0C0">Classes in Swift come with a built-in notion of identity because they're reference types. This means that when two different class instances have the same value for each of their stored properties, they're still considered to be different by the identity operator (===). It also means that when you share a class instance across your app, changes you make to that instance are visible to every part of your code that holds a reference to that instance. Use classes when you need your instances to have this kind of identity. Common use cases are file handles, network connections, and shared hardware intermediaries like [CBCentralManager](https://developer.apple.com/documentation/corebluetooth/cbcentralmanager).</span></i>    
스위프트의 클래스는 참조 타입이기 때문에 독자성[¹](#1)에 대한 기본 개념이 함께 있습니다. 이 말은, 두 개의 서로 다른 클래스 인스턴스가 각각의 저장 프로퍼티에 대해 같은 값을 가질 때, identity 연산자(===)는 여전히 서로 다른 것으로 여겨집니다. 또한 앱에서 클래스 인스턴스를 공유할 때, 해당 인스턴스에 대한 변경 사항은 해당 인스턴스가 참조된 코드의 모든 부분에 보입니다. 이런 독자성[¹](#1)을 갖기 위해 인스턴스가 필요할 때 클래스를 사용하세요. 일반적인 사용 사례는 파일 다루기 *(File handles)*, 네트워크 연결 *(Network connections)*, [CBCentralManager](https://developer.apple.com/documentation/corebluetooth/cbcentralmanager)같은 공유 하드웨어 중개 *(Shared hardware intermediaries)* 입니다.

<i><span style="color: #C0C0C0">For example, if you have a type that represents a local database connection, the code that manages access to that database needs full control over the state of the database as viewed from your app. It's appropriate to use a class in this case, but be sure to limit which parts of your app get access to the shared database object.</span></i>    
예를 들어 지역[²](#2) 데이터베이스 연결을 나타내는 타입이 있는 경우, 해당 데이터베이스에 대한 액세스를 관리하는 코드는 앱에서 보여지는 데이터베이스 상태를 완전히 제거해야 합니다. 이런 경우 클래스를 사용하는 것이 적절하지만, 앱에서 공유 데이터베이스 개체에 액세스할 수 있는 부분을 확실히 제한해야 합니다.

> **Important**   
> <i><span style="color: #C0C0C0">
Treat identity with care. Sharing class instances pervasively throughout an app makes logic errors more likely. You might not anticipate the consequences of changing a heavily shared instance, so it's more work to write such code correctly.</span></i>    
> 독자성[¹](#1)을 조심해서 다뤄야 합니다. 앱 전체에 걸쳐 클래스 인스턴스를 구석구석 공유하는 건 논리 오류가 발생할 가능성이 커집니다. 공유가 많은 인스턴스를 변경할 때 결과가 예상되지 않을 수 있으므로, 이런 코드를 올바르게 작성하는 것이 더 중요합니다.

<br>

## <i><span style="color: #C0C0C0">Use Structures When You Don't Control Identity</span></i>
## 독자성[¹](#1)를 제어하지 않을 때 구조체를 기본으로 선택하기

<i><span style="color: #C0C0C0">Use structures when you're modeling data that contains information about an entity with an identity that you don't control.</span></i>    
제어하지 않는 독자성[¹](#1)을 가진 엔티티에 대한 정보가 들어있는 데이터를 모델링할 때 구조체를 사용하세요. 

<i><span style="color: #C0C0C0">In an app that consults a remote database, for example, an instance's identity may be fully owned by an external entity and communicated by an identifier. If the consistency of an app's models is stored on a server, you can model records as structures with identifiers. In the example below, jsonResponse contains an encoded PenPalRecord instance from a server:</span></i>    
예를 들어, 원격 데이터베이스를 참조하는 앱에서, 인스턴스의 독자성[¹](#1)은 외부 엔티티에 의해 완전히 소유되고 식별자에 의해 전달될 수 있습니다. 앱 모델의 일관성이 서버에 저장된 경우, 레코드[³](#3)를 식별자가 있는 구조체로 모델링할 수 있습니다. 아래 예제에서 `jsonResponse`는 서버에서 인코딩된 `PenPalRecord` 인스턴스를 포함합니다.

```swift
struct PenPalRecord {
    let myID: Int
    var myNickname: String
    var recommendedPenPalID: Int
}

var myRecord = try JSONDecoder().decode(PenPalRecord.self, from: jsonResponse)
```

<i><span style="color: #C0C0C0">Local changes to model types like PenPalRecord are useful. For example, an app might recommend multiple different penpals in response to user feedback. Because the PenPalRecord structure doesn't control the identity of the underlying database records, there's no risk that the changes made to local PenPalRecord instances accidentally change values in the database.</span></i>    
`PenPalRecord`와 같은 모델 타입에 대한 지역[²](#2) 변경은 유용합니다. 예를 들어, 앱은 사용자 피드백에 응답해서 여러 개의 다른 `penpal`을 추천할 수 있습니다. `PenPalRecord` 구조체는 근본 데이터베이스 레코드[³](#3)의 독자성[¹](#1)을 제어하지 않기 때문에, 지역[²](#2) `PenPalRecord` 인스턴스의 변경 사항이 데이터베이스의 값을 실수로 변경할 위험은 없습니다.

<i><span style="color: #C0C0C0">If another part of the app changes myNickname and submits a change request back to the server, the most recently rejected penpal recommendation won't be mistakenly picked up by the change. Because the myID property is declared as a constant, it can't change locally. As a result, requests to the database won't accidentally change the wrong record.</span></i>    
앱의 다른 부분이 `myNickname`을 변경하고 변경 요청을 서버로 제출하면, 가장 최근에 거부된 `penpal` 추천은 변경 사항으로 인해서 잘못 선택되지 않을 겁니다. `myID` 프로퍼티는 상수로 선언되기 때문에 지역적으로 변할 수 없습니다. 따라서 데이터베이스에 대한 요청이 뜻하지 않게 잘못된 레코드[³](#3)를 변경하지 않습니다.

<br>

## <i><span style="color: #C0C0C0">Use Structures and Protocols to Model Inheritance and Share Behavior</span></i>
## 상속을 모델링하고 동작을 공유하기 위해 구조체와 프로토콜을 사용하기

<i><span style="color: #C0C0C0">Structures and classes both support a form of inheritance. Structures and protocols can only adopt protocols; they can't inherit from classes. However, the kinds of inheritance hierarchies you can build with class inheritance can be also modeled using protocol inheritance and structures.</span></i>    
구조체와 클래스 모두 상속을 지원합니다. 구조체와 프로토콜은 프로토콜만 채택할 수 있습니다; 구조체는 클래스로부터 상속을 할 수 없습니다. 그러나, 클래스 상속으로 구축할 수 있는 상속 계층의 종류는 프로토콜 상속이나 구조체를 사용해서도 모델링 할 수 있습니다.

<i><span style="color: #C0C0C0">If you're building an inheritance relationship from scratch, prefer protocol inheritance. Protocols permit classes, structures, and enumerations to participate in inheritance, while class inheritance is only compatible with other classes. When you're choosing how to model your data, try building the hierarchy of data types using protocol inheritance first, then adopt those protocols in your structures.</span></i>    
처음부터 상속 관계를 구축하는 경우 프로토콜 상속을 선호합니다. 프로토콜은 클래스, 구조체 및 열거형을 상속에 참여할 수 있는데, 반면에 클래스 상속은 다른 클래스만 호환합니다. 데이터를 모델링하는 방법을 선택할 때는, 먼저 프로토콜 상속을 사용해서 데이터 타입의 계층을 구축한 다음 해당 프로토콜을 구조체에 채택합니다.

---
###### 1 
identity 독자성 
###### 2
local 지역 
###### 3
record, 레코드 [wiki](https://ko.wikipedia.org/wiki/로우_(데이터베이스))