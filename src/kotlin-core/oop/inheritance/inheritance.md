#### [Основы наследования](#Основы-наследования-1)
#### [Переопределение методов](#Переопределение-методов-1)
#### [Переопределение свойств](#Переопределение-свойств-1)
#### [Вызов методов и свойств базового класса](#Вызов-методов-и-свойств-базового-класса-1)
#### [Абстрактные классы](#Абстрактные-классы-1)
#### [Интерфейсы и наследование](#Интерфейсы-и-наследование-1)
#### [Практическое применение наследования](#Практическое-применение-наследования-1)

---
# Наследование в Kotlin

Наследование — это одна из ключевых концепций объектно-ориентированного программирования (ООП), которая позволяет создавать новые классы на основе существующих. В Kotlin наследование используется для повторного использования кода, расширения функциональности и создания иерархий классов. В этом разделе мы рассмотрим, как работает наследование в Kotlin, его синтаксис, особенности и практическое применение.

---

## Основы наследования

### Базовый класс и производный класс
В Kotlin все классы по умолчанию являются **финальными** (не могут быть унаследованы). Чтобы разрешить наследование, класс должен быть помечен ключевым словом `open`.

```kotlin
open class PaymentMethod(val name: String) {
    fun processPayment(amount: Double) {
        println("Обработка платежа $amount с помощью $name.")
    }
}
```

- Класс `PaymentMethod` является базовым классом. Он помечен как `open`, чтобы его можно было наследовать.

### Создание производного класса
Производный класс наследует свойства и методы базового класса. Для этого используется ключевое слово `:` после имени производного класса.

```kotlin
class CreditCard(name: String) : PaymentMethod(name) {
    fun authorize() {
        println("Авторизация кредитной карты $name.")
    }
}
```

- Класс `CreditCard` наследует класс `PaymentMethod` и добавляет новый метод `authorize()`.

### Использование наследования
Теперь мы можем создавать объекты производного класса и использовать как унаследованные, так и новые методы.

```kotlin
val creditCard = CreditCard("Visa")
creditCard.processPayment(100.0)  // Унаследованный метод
creditCard.authorize()            // Новый метод
```

---

## Переопределение методов

### Переопределение методов базового класса
Чтобы переопределить метод базового класса, он должен быть помечен как `open`. В производном классе метод переопределяется с помощью ключевого слова `override`.

```kotlin
open class PaymentMethod(val name: String) {
    open fun processPayment(amount: Double) {
        println("Обработка платежа $amount с помощью $name.")
    }
}

class PayPal(name: String) : PaymentMethod(name) {
    override fun processPayment(amount: Double) {
        println("Обработка платежа $amount через PayPal ($name).")
    }
}
```

- Метод `processPayment()` переопределен в классе `PayPal`.

### Запрет на дальнейшее переопределение
Если вы хотите запретить дальнейшее переопределение метода в производных классах, используйте ключевое слово `final`.

```kotlin
open class PaymentMethod(val name: String) {
    open fun processPayment(amount: Double) {
        println("Обработка платежа $amount с помощью $name.")
    }
}

class PayPal(name: String) : PaymentMethod(name) {
    final override fun processPayment(amount: Double) {
        println("Обработка платежа $amount через PayPal ($name).")
    }
}
```

- Метод `processPayment()` в классе `PayPal` теперь нельзя переопределить в дальнейших производных классах.

---

## Переопределение свойств

### Переопределение свойств базового класса
Свойства также можно переопределять в производных классах. Для этого свойство в базовом классе должно быть помечено как `open`, а в производном классе — как `override`.

```kotlin
open class PaymentMethod {
    open val fee: Double = 0.0
}

class CreditCard : PaymentMethod() {
    override val fee: Double = 1.5  // Комиссия для кредитной карты
}
```

- Свойство `fee` переопределено в классе `CreditCard`.

### Использование переопределенных свойств
Переопределенные свойства можно использовать так же, как и обычные.

```kotlin
val creditCard = CreditCard()
println("Комиссия: ${creditCard.fee}%")  // Output: Комиссия: 1.5%
```

---

## Вызов методов и свойств базового класса

### Ключевое слово `super`
Ключевое слово `super` используется для доступа к методам и свойствам базового класса из производного класса.

```kotlin
open class PaymentMethod(val name: String) {
    open fun processPayment(amount: Double) {
        println("Обработка платежа $amount с помощью $name.")
    }
}

class PayPal(name: String) : PaymentMethod(name) {
    override fun processPayment(amount: Double) {
        super.processPayment(amount)  // Вызов метода базового класса
        println("Дополнительная обработка через PayPal.")
    }
}
```

- Метод `processPayment()` в классе `PayPal` сначала вызывает метод базового класса, а затем добавляет свою логику.

---

## Абстрактные классы

### Определение абстрактного класса
Абстрактный класс — это класс, который не может быть инстанцирован (создан как объект). Он может содержать абстрактные методы (без реализации), которые должны быть реализованы в производных классах.

```kotlin
abstract class PaymentGateway {
    abstract fun processPayment(amount: Double)  // Абстрактный метод
}

class Stripe : PaymentGateway() {
    override fun processPayment(amount: Double) {
        println("Обработка платежа $amount через Stripe.")
    }
}
```

- Класс `PaymentGateway` является абстрактным, а метод `processPayment()` реализован в классе `Stripe`.

### Использование абстрактных классов
Абстрактные классы полезны для создания общих интерфейсов и обеспечения обязательной реализации определенных методов.

```kotlin
val stripe = Stripe()
stripe.processPayment(200.0)  // Output: Обработка платежа 200.0 через Stripe.
```

---

## Интерфейсы и наследование

### Множественное наследование через интерфейсы
Kotlin не поддерживает множественное наследование классов, но позволяет реализовывать несколько интерфейсов. Интерфейсы могут содержать методы с реализацией по умолчанию.

```kotlin
interface Refundable {
    fun refund(amount: Double) {
        println("Возврат $amount.")
    }
}

interface Loggable {
    fun log(message: String) {
        println("Лог: $message")
    }
}

class PayPalPayment : Refundable, Loggable
```

- Класс `PayPalPayment` реализует интерфейсы `Refundable` и `Loggable`.

### Использование интерфейсов
Теперь объект класса `PayPalPayment` может использовать методы из обоих интерфейсов.

```kotlin
val payment = PayPalPayment()
payment.refund(50.0)  // Output: Возврат 50.0.
payment.log("Платеж возвращен.")  // Output: Лог: Платеж возвращен.
```

---

## Практическое применение наследования

### Пример: Иерархия платежных систем
Рассмотрим пример иерархии классов для платежных систем.

```kotlin
open class PaymentMethod(val name: String) {
    open fun processPayment(amount: Double) {
        println("Обработка платежа $amount с помощью $name.")
    }
}

class CreditCard(name: String) : PaymentMethod(name) {
    override fun processPayment(amount: Double) {
        println("Обработка платежа $amount через кредитную карту $name.")
    }
}

class PayPal(name: String) : PaymentMethod(name) {
    override fun processPayment(amount: Double) {
        println("Обработка платежа $amount через PayPal ($name).")
    }
}
```

- Классы `CreditCard` и `PayPal` наследуют класс `PaymentMethod` и переопределяют метод `processPayment()`.

### Использование иерархии
Теперь мы можем создавать объекты разных типов и вызывать их методы.

```kotlin
val creditCard = CreditCard("Visa")
val payPal = PayPal("example@example.com")

creditCard.processPayment(100.0)  // Output: Обработка платежа 100.0 через кредитную карту Visa.
payPal.processPayment(200.0)      // Output: Обработка платежа 200.0 через PayPal (example@example.com).
```


