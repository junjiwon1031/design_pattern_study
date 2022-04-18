# Factory Method

## intent
**Factory method**는 creational design pattern의 하나로, superclass에 객체를 만들기 위한 interface를 두고 객체를 만들 subclass에서 객체의 타입을 변경할 수 있도록 허용하는 패턴입니다.

![포토샵이 없어 수정을 못하겠어!](../../assets/factory_method/factory-method-en.png)

## Problem
우리가 물류 (logistic) 관리용 어플리케이션을 만들었다고 생각해봅시다.
이 앱의 첫번째 버전에선 트럭으로만 운송을 할 수 있었고, 때문에 대부분의 코드는 `Truck` class 안에 있을 겁니다.

시간이 지나, 우리의 앱은 크게 성공하였습니다.
날마다 앱에 해양 물류를 추가해 달라는 해양 운송 회사들의 수 많은 요청을 받게 되었죠.

> ![성공해버린 나](../../assets/factory_method/problem1-en.png)
>
> 이미 코드들이 존재하는 class에 이리 저리 엮여 있으면 새로운 클래스를 추가하기 쉽지 않다.


와! 신난다! 하지만 코드를 살펴볼까요? 현재 우리의 대부분의 코드들은 `Truck` class에 엮여있는 상태입니다. `Ship` class를 추가하려면 우리의 모든 codebase를 바꿔야 합니다.
게다가 나중에 다른 운송 수단이 우리 앱에 추가될 때도 똑같이 전체적인 코드를 바꿔야 합니다.

결과적으로 운송 객체의 class에 따라 조건부로 동작을 실행하는 아주 끔찍한 코드가 되어버릴 것입니다.

## Solution
TBI

