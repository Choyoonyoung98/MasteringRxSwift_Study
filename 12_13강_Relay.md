# Relay

* Relay는 Subject를 래핑한 것이다. 따라서 래핑한 Subject의 성격을 그대로 띈다. 
* Relay는 Subject와 달리 Completed 이벤트와 Error 이벤트를 전달받지도 않고 전달하지도 않는다.
* 따라서 **종료되지 않는다**는 특징이 있다. 구독자가 dispose 되기 전까지 계속 이벤트를 처리한다. 
* 그래서 주로 끝나지 않는 UI Event(RxCocoa)에 이용된다. Relay는 RxSwift 가 아닌 **RxCocoa**를 통해 제공된다.

```swift
let bag = DisposeBag()

let publishRelay = PublishRelay<Int>()
publishRelay.subscribe { print("1: \($0)") }
publishRelay.accept(1)

// 실행 결과 
// 1: next(1)
```
=> relay는 onNext가 아닌 **accept 메소드**를 통해 Next 이벤트를 전달한다.

```swift
let behaviorRelay = BehaviorRelay<Int>(value: 1)
behaviorRelay.accept(2)

behaviorRelay.subscribe { print("2: \($0)") }
  .disposed(by: bag)

behaviorRelay.accept(3)
print(behaviorRelay.value) // 저장된 value값을 리턴한다. 

// 실행 결과 
// 1: next(1)
// 2: next(2)
// 2: next(3)
// 3
```

```
let replayRelay = ReplayRelay<Int>.create(bufferSize: 3)

(1 ... 10).forEach { replayRelay.accept($0) }

replayRelay.subscribe { print("3: \($0)") }
  .disposed(by: bag)
  
//실행 결과
// 3: next(8)
// 3: next(9)
// 3: next(10)
```
