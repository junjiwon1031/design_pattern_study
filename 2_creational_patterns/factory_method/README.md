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
> 코드가 이미 존재하는 class에 이리저리 엮여있는 상태여서, 새로운 class를 추가하는 것은 쉽지 않다...


와! 신난다! 하지만 코드를 살펴볼까요? 현재 우리의 대부분의 코드들은 `Truck` class에 엮여있는 상태입니다. `Ship` class를 추가하려면 우리의 모든 codebase를 바꿔야 합니다.
게다가 나중에 다른 운송 수단이 우리 앱에 추가될 때도 똑같이 전체적인 코드를 바꿔야 합니다.

결과적으로 운송 객체의 class에 따라 조건부로 동작을 실행하는 아주 끔찍한 코드가 되어버릴 것입니다.

## Solution

> ![solution1](../../assets/factory_method/solution1.png)
>
> subclass들은 factory method에 의해 return 될 객체들의 class를 바꿀 수 있습니다. (`Truck` vs `Ship`)

Factory Method 패턴은 직접적인 개체 생성 호출(`new` operator를 사용하는) 들을 *factory*라 불리는 method로 대체하는 패턴입니다.
대체한다곤 했지만 결과적으로 `new`를 통해서 객체를 생성하는 건 같습니다. 단지 *factory method*를 통해서 호출될 뿐입니다. Factory method에 의해 반환된 객체는 종종 product라고 불립니다.

처음 보면 '아니 그냥 constructor를 call 하는 위치만 바꾼거 아닌가?'와 같은 생각이 들 수도 있습니다. 하지만 factory method를 subclass에서 override 할 수 있게 되었고, 메서드에서 생성되는 product class 를 변경할 수 있게 되었습니다.

물론 작은 제한 사항도 발생합니다. 첫째로, subclass들이 공통된 base class 나 interface를 가진 경우에만 여러 유형의 product를 반환할 수 있습니다. 그리고 base class의 factory method는 이 interface로 선언된 return type을 반드시 가지고 있어야 합니다.

> ![solution2](../../assets/factory_method/solution2-en.png)
>
> 모든 product들은 같은 interface를 따라가야 한다.

예를 들어 `Truck`과 `Ship` class들은 `deliver`이라 불리는 method가 정의된 `Transport` interface를 구현해야 합니다. 트럭은 땅에서 화물을 운송하고 배는 바다에서 화물을 움직이기에 각각의 class는 다르게 interface를 구현합니다. `RoadLogistics`의 factory method는 `truck` 객체를 반환하고 `SeaLogistics`의 factory method는 배를 `Ship` 객체를 반환합니다.

> ![solution3](../../assets/factory_method/solution3-en.png)
>
> 모든 product class들이 동일한 interface로 구현하는 한, 객체들을 client code로 손상 없이 전달할 수 있게 된다.

Factory method를 이용한 코드 (종종 client code라 부르는)들은 여러 subclass들이 반환하는 실제 product들이 어떠한 차이가 있는지 확인하지 않습니다. client들은 모든 product들을 추상적인 `Transport`로 처리할 뿐이죠. client는 모든 `Transport` 객체들이 `deliver`를 가지고 있기로 한 것을 알고 있지만 `deliver`가 어떻게 작동하는지는 별로 중요하지 않습니다.

## Structure

![structure](../../assets/factory_method/structure.png)

1. TBI
2. TBI
3. TBI

## Pseudocode

> ![example](../../assets/factory_method/example.png)

TBI

```
// The creator class declares the factory method that must
// return an object of a product class. The creator's subclasses
// usually provide the implementation of this method.
class Dialog is
    // The creator may also provide some default implementation
    // of the factory method.
    abstract method createButton():Button

    // Note that, despite its name, the creator's primary
    // responsibility isn't creating products. It usually
    // contains some core business logic that relies on product
    // objects returned by the factory method. Subclasses can
    // indirectly change that business logic by overriding the
    // factory method and returning a different type of product
    // from it.
    method render() is
        // Call the factory method to create a product object.
        Button okButton = createButton()
        // Now use the product.
        okButton.onClick(closeDialog)
        okButton.render()


// Concrete creators override the factory method to change the
// resulting product's type.
class WindowsDialog extends Dialog is
    method createButton():Button is
        return new WindowsButton()

class WebDialog extends Dialog is
    method createButton():Button is
        return new HTMLButton()


// The product interface declares the operations that all
// concrete products must implement.
interface Button is
    method render()
    method onClick(f)

// Concrete products provide various implementations of the
// product interface.
class WindowsButton implements Button is
    method render(a, b) is
        // Render a button in Windows style.
    method onClick(f) is
        // Bind a native OS click event.

class HTMLButton implements Button is
    method render(a, b) is
        // Return an HTML representation of a button.
    method onClick(f) is
        // Bind a web browser click event.


class Application is
    field dialog: Dialog

    // The application picks a creator's type depending on the
    // current configuration or environment settings.
    method initialize() is
        config = readApplicationConfigFile()

        if (config.OS == "Windows") then
            dialog = new WindowsDialog()
        else if (config.OS == "Web") then
            dialog = new WebDialog()
        else
            throw new Exception("Error! Unknown operating system.")

    // The client code works with an instance of a concrete
    // creator, albeit through its base interface. As long as
    // the client keeps working with the creator via the base
    // interface, you can pass it any creator's subclass.
    method main() is
        this.initialize()
        dialog.render()
```

## Applicability
TBI

## How to Implement
TBI

## Pros and Cons
TBI

## Relations with Other Patterns
TBI
