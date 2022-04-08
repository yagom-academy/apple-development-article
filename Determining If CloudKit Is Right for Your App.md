# <i><span style="color: #C0C0C0">Determining If CloudKit Is Right for Your App</span></i>
# CloudKit이 당신의 앱에 적합한지 결정하세요.

<i><span style="color: #C0C0C0">Explore various ways of storing your app’s data in iCloud.</span></i>   
iCloud에 앱의 데이터를 저장하는 다양한 방법을 살펴봅시다.

---

## <i><span style="color: #C0C0C0">Overview</span></i>
## 개요

<i><span style="color: #C0C0C0">There are several ways you can store app and user data in iCloud. This article describes the various options and their suitability for your app. Each option has trade-offs in complexity, access to iCloud features, and control over how your data persists.</span></i>   
iCloud에 앱과 사용자의 정보를 저장할 수 있는 다양한 방법이 있습니다. 이 아티클은 당신의 앱에 대한 다양한 옵션들과 적합성에 대해서 설명합니다. 각 옵션에는 복잡성, iCloud 기능 엑세스, 데이터 지속 방식 제어 등의 trade-off가 있습니다.

<i><span style="color: #C0C0C0">Consider the following approaches before you choose CloudKit.</span></i>    
CloudKit을 선택하기 전에 다음 방법들을 고려하세요.

<br>

## <i><span style="color: #C0C0C0">Store Data as Files</span></i> 
## 파일로 데이터 저장하기

<i><span style="color: #C0C0C0">If your app stores data as files and you want those files to sync across devices, you can use iCloud document storage. This storage option gives you granular control over which files to sync. Choosing to store your data as files affords the least control over how your data persists to iCloud, but is relatively simple.</span></i>    
만약 앱이 데이터를 파일로 저장하거나 파일들을 기기 간에 동기화시키고 싶다면, iCloud document 저장소를 사용할 수 있습니다. 이 저장소 옵션을 사용하면 동기화할 파일을 세밀하게 제어할 수 있습니다. 데이터를 파일로 저장하는 것을 선택하면 데이터가 iCloud에 유지되는 방식을 가장 적게 제어할 수 있지만 비교적으로 간단합니다.

<i><span style="color: #C0C0C0">To use iCloud document storage, use [url(forUbiquityContainerIdentifier:)](https://developer.apple.com/documentation/foundation/filemanager/1411653-url) in [FileManager](https://developer.apple.com/documentation/foundation/filemanager). For more information, see [Designing for Documents in iCloud](https://developer.apple.com/library/archive/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForDocumentsIniCloud.html#//apple_ref/doc/uid/TP40012094-CH2).</span></i>    
iCloud document 저장소를 사용하려면, [FileManager](https://developer.apple.com/documentation/foundation/filemanager)의 [url(forUbiquityContainerIdentifier:)](https://developer.apple.com/documentation/foundation/filemanager/1411653-url)를 사용하세요. 정보를 더 얻고 싶다면 [Designing for Documents in iCloud](https://developer.apple.com/library/archive/documentation/General/Conceptual/iCloudDesignGuide/Chapters/DesigningForDocumentsIniCloud.html#//apple_ref/doc/uid/TP40012094-CH2)를 참고하세요.

<i><span style="color: #C0C0C0">If your files reside on a single device, you can save them using the nonubiquity methods of [FileManager](https://developer.apple.com/documentation/foundation/filemanager). When a user enables iCloud Backup, a snapshot of these files uploads to iCloud. The user can then restore a device using that snapshot. Note that your app’s data contributes to the overall device backup size. A large total backup size can lead to lengthy restore times. See [iCloud Backup](https://developer.apple.com/library/archive/releasenotes/General/WhatsNewIniOS/Articles/iOS5.html#//apple_ref/doc/uid/TP30915195-SW36).</span></i>    
만약 파일이 하나의 기기에 있다면, [FileManager](https://developer.apple.com/documentation/foundation/filemanager)의 nonubiquity 메서드를 사용하여 저장할 수 있습니다. 사용자가 iCloud 백업을 이용할 수 있을 때, 이 파일들의 스냅샷이 iCloud에 업로드 됩니다. 사용자는 이 스냅샷을 가지고 기기에 다시 저장할 수 있습니다. 앱의 데이터는 전체 기기의 백업 크기에 영향을 미칩니다. 백업할 데이터가 많아지면 복원하는데에 시간이 오래걸립니다. [iCloud Backup](https://developer.apple.com/library/archive/releasenotes/General/WhatsNewIniOS/Articles/iOS5.html#//apple_ref/doc/uid/TP30915195-SW36)을 참고하세요.

<br>

## <i><span style="color: #C0C0C0">Store Unstructured Data That Syncs Across Devices</span></i>
## 기기 간에 동기화되는 구조화되지 않은 데이터 저장하기

<i><span style="color: #C0C0C0">If you store a small number of key-value pairs that sync across the user’s devices, you can use iCloud key-value storage. This approach is simple but doesn’t provide control over how data persists, and it limits the kind of data you can store.</span></i>    
만약 사용자의 기기 간에 동기화시킬 적은 수의 키-값들을 저장한다면, iCloud key-value 저장소를 사용할 수 있습니다. 간단한 방법이지만 데이터가 유지되는 방법에 대한 제어를 제공하지는 않고, 저장할 수 있는 데이터 종류에 제한이 있습니다.

<i><span style="color: #C0C0C0">You can store up to 1,024 pairs of string keys to scalar values. The value types supported are Bool, numeric types, String, Date, Data, Array, and Dictionary. You might use this solution to store a user’s preferences or game state as property lists, for example. For more information, see [NSUbiquitousKeyValueStore](https://developer.apple.com/documentation/foundation/nsubiquitouskeyvaluestore).</span></i>    
1024개까지 문자열 키와 스칼라 값의 짝을 저장할 수 있습니다. 값 타입으로는 Bool, 숫자, String, Date, Data, Array, Dictionary 타입들을 지원합니다. 아마 사용자의 설정이나 프로퍼티 리스트로 저장된 게임의 상태 등을 저장하는데에 사용하게 될 것입니다. 정보를 더 얻고 싶다면 [NSUbiquitousKeyValueStore](https://developer.apple.com/documentation/foundation/nsubiquitouskeyvaluestore)를 참고하세요.

<br>

## <i><span style="color: #C0C0C0">Store Objects That Sync Across Devices</span></i>
## 기기 간에 동기화되는 객체 저장하기

<i><span style="color: #C0C0C0">If you store your data as objects and relationships, consider using Core Data with CloudKit, or using CloudKit directly. Using CloudKit directly is more complex, but it gives you full control over the data you store, including the design and management of your container’s schema. Use this option if you want direct access to iCloud storage features, such as private data sharing, or if you want to manage local data caching. With a local cache of data, your app can reduce the number of calls to iCloud and can access cached data offline.</span></i>     
만약 객체와 관계로 데이터를 저장할 경우, CloudKit과 Core Data를 함께 사용하거나, CloudKit을 직접적으로 사용할 수 있습니다. CloudKit을 직접적으로 사용하는 것은 복잡하지만 데이터를 저장하는데에 있어서 디자인이나 컨테이너의 스키마 관리 등 모든 권한을 얻을 수 있습니다. 개인 정보를 공유하거나 로컬 데이터 캐싱을 관리하고 싶을 때 iCloud 저장소 기능에 직접 접근하고 싶다면 이 옵션을 사용하세요. 로컬 데이터 캐시를 사용하면 iCloud 호출 수를 줄이고 오프라인에서 캐시된 데이터에 액세스할 수 있습니다.

<i><span style="color: #C0C0C0">If you only sync private data and you don’t need granular control over which data syncs, you can use Core Data with CloudKit. Core Data with CloudKit stores a local replica of a CloudKit private database and syncs it across all of the user’s devices. This storage option doesn’t support all CloudKit features—for example, it doesn’t support live queries and shared private data—but can be simpler than using CloudKit directly. Core Data with CloudKit manages the schema for your iCloud container, so you don’t have to.</span></i>   
개인 데이터만 동기화하고 데이터 도익화를 세밀하게 제어할 필요가 없는 경우 CloudKit과 함께 Core Data를 사용할 수 있습니다. CloudKit을 사용한 Core Data는 개인 데이터 베이스의 로컬 복제본을 저장하고 사용자의 모든 기기에 동기화합니다. 이 저장소 옵션은 실시간 쿼리나 개인 데이터 공유 등 CloudKit의 모든 기능을 제공하지는 않지만 CloudKit을 직접적으로 사용하는 것보다 간단할 수 있습니다. CloudKit을 사용한 Core Data는 iCloud 컨테이너의 스키마를 관리하므로 사용자가 관리할 필요가 없습니다.

<i><span style="color: #C0C0C0">To get started with CloudKit, see [Enabling CloudKit in Your App](https://developer.apple.com/documentation/cloudkit/enabling_cloudkit_in_your_app). For more information on Core Data with CloudKit, see [Setting Up Core Data with CloudKit](https://developer.apple.com/documentation/coredata/mirroring_a_core_data_store_with_cloudkit/setting_up_core_data_with_cloudkit).</span></i>    
CloudKit을 시작하려면 [Enabling CloudKit in Your App](https://developer.apple.com/documentation/cloudkit/enabling_cloudkit_in_your_app)를 확인하세요. CloudKit을 이용한 Core Data에 대해서 더 정보를 얻고 싶다면 [Setting Up Core Data with CloudKit](https://developer.apple.com/documentation/coredata/mirroring_a_core_data_store_with_cloudkit/setting_up_core_data_with_cloudkit)를 확인해주세요.


