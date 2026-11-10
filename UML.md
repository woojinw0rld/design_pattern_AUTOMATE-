# 소프트웨어요구분석및디자인

- 해당 과목의 팀프로젝트로 특정 프로젝트의 클래스 구조를 분석
- 분석한 클래스 구조로 디자인패턴을 클래스 다이어그램으로 그리는 팀프로젝트.
- 해당 프로젝트의 주제는 스타듀벨리의 모드인 Automate모드로 선정함.

---

## UML


### 최상위 클래스 분석

```mermaid
classDiagram 
    %% 최상위 클래스.
    class IAutomatable { 
        <<interface>>
        +GameLocation Location
        +Rectangle TileArea
    }
    class IMachine{
        <<interface>>
        +GetState()
        +GetOutput()
        +SetInput(IStorage input)
    }
    class IContainer {
        <<interface>>
        string TypeId 
        <!-- string Name  -->
        ModDataDictionary ModData 
        bool IsJunimoChest 
        bool IsLocked 
        object InventoryReferenceId 
        IInventory Inventory D
        +GetFilled()
        +GetCapacity()
        +Store(ITrackedStack stack)
    }
    MachineGroup "1" o-- "*" IMachine : Machine
    MachineGroup "1" o-- "*" IContainer : Containers
    IAutomatable <|-- IMachine
    IAutomatable <|-- IContainer
```

---

### Adapter 패턴

```mermaid
classDiagram
    direction TB

    class IAutomatable {
        <<interface>>
        +GameLocation Location
        +Rectangle TileArea
    }

    class IMachine {
        <<interface>>
        +GetState() MachineState
        +GetOutput() ITrackedStack
        +SetInput(IStorage input) bool
    }

    class FishPondMachine {
        +GetState() MachineState
        +GetOutput() ITrackedStack
        +SetInput(IStorage input) bool
    }
    class JunimoHutBehavior  {

    }

    class FishPond {
        <<game class>>
    }

    class MachineGroup

    IAutomatable <|-- IMachine
    IMachine <|.. FishPondMachine : BaseMachine 경유
    FishPondMachine --> FishPond : Machine
    MachineGroup "1" o-- "*" IMachine : Machines
    IMachine <|.. JunimoHutBehavior : BaseMachine 경유

    note for IMachine "Target"
    note for FishPondMachine "Adapter"
    note for FishPond "Adaptee(게임 클래스)"
    note for MachineGroup "Client"
```


---


### Composite 패턴
```mermaid
classDiagram
    direction TB

    class ITrackedStack {
        <<interface>>
        +int Count
        +Reduce(int count)
    }

    class TrackedItem {
        +int Count
        +Reduce(int count)
    }

    class TrackedItemCollection {
        -List~ITrackedStack~ Stacks
        +int Count
        +Reduce(int count)
    }

    class Consumable {
        +ITrackedStack Consumables
        +Reduce()
    }

    ITrackedStack <|.. TrackedItem
    ITrackedStack <|.. TrackedItemCollection
    TrackedItemCollection "1" o-- "*" ITrackedStack : Stacks
    Consumable --> ITrackedStack

    note for ITrackedStack "Component"
    note for TrackedItem "Leaf"
    note for TrackedItemCollection "Composite"
    note for Consumable "Client"
```


