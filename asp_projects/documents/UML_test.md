### ユースケース図
食事に来た客の振る舞い

---

```uml
@startuml
left to right direction
actor "お客" as fc
rectangle レストラン {
  usecase "食べる" as UC1
  usecase "代金を払う" as UC2
  usecase "飲む" as UC3
  usecase "注文する" as UC4
}
fc --> UC1
fc --> UC2
fc --> UC3
fc --> UC4
@enduml
```
