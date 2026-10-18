# Obsidian Mermaid Diagram Collection

---

# 1. Complex Game Architecture

```mermaid
flowchart TB

    subgraph Presentation["Presentation Layer"]
        UI["UI"]
        HUD["HUD"]
        Menus["Menus"]
    end

    subgraph Gameplay["Gameplay Layer"]
        Player["Player"]
        Combat["Combat"]
        Movement["Movement"]
        Inventory["Inventory"]
    end

    subgraph Core["Core Systems"]
        GameState["Game State"]
        EventBus["Event Bus"]
        Time["Time Controller"]
        Config["Configuration"]
    end

    subgraph Infrastructure["Infrastructure"]
        Save["Save System"]
        Audio["Audio System"]
        Network["Network System"]
        Analytics["Analytics"]
    end

    UI --> Player
    HUD --> Player
    Menus --> GameState

    Player --> Movement
    Player --> Combat
    Player --> Inventory

    Combat --> EventBus
    Movement --> Time
    Inventory --> EventBus

    EventBus --> GameState

    GameState --> Save
    Combat --> Audio
    Player --> Network
    GameState --> Analytics
```

---

# 2. Layered Architecture

```mermaid
flowchart TB

    A["Presentation"]

    B["Application"]

    C["Domain"]

    D["Infrastructure"]

    E[("Database")]

    A --> B
    B --> C
    B --> D
    D --> E
```

---

# 3. Dependency Graph

```mermaid
flowchart LR

    Input["Input System"]

    Player["Player Controller"]

    Movement["Movement"]

    Combat["Combat"]

    Weapon["Weapon System"]

    Damage["Damage System"]

    Health["Health System"]

    Animation["Animation"]

    Audio["Audio"]

    VFX["VFX"]

    Input --> Player

    Player --> Movement
    Player --> Combat

    Combat --> Weapon
    Weapon --> Damage
    Damage --> Health

    Movement --> Animation
    Combat --> Animation

    Combat --> Audio
    Damage --> VFX
```

---

# 4. Complex State Machine

```mermaid
stateDiagram-v2

    [*] --> Loading

    Loading --> MainMenu : Load Complete

    MainMenu --> Lobby : Play

    Lobby --> Connecting : Join Match
    Connecting --> Match : Connected
    Connecting --> Lobby : Connection Failed

    state Match {

        [*] --> Waiting

        Waiting --> Countdown : Players Ready

        Countdown --> Playing : Countdown Complete

        Playing --> Paused : Pause

        Paused --> Playing : Resume

        Playing --> RoundEnd : Round Finished

        RoundEnd --> Playing : Next Round

        RoundEnd --> MatchEnd : Final Round
    }

    MatchEnd --> MainMenu : Exit
```

---

# 5. Player State Machine

```mermaid
stateDiagram-v2

    [*] --> Idle

    Idle --> Moving : Move Input
    Moving --> Idle : Stop Input

    Idle --> Jumping : Jump
    Moving --> Jumping : Jump

    Jumping --> Falling : Vertical Velocity < 0

    Falling --> Idle : Grounded

    Idle --> Attacking : Attack
    Moving --> Attacking : Attack

    Attacking --> Idle : Attack Finished

    Idle --> Dead : Health <= 0
    Moving --> Dead : Health <= 0
    Attacking --> Dead : Health <= 0

    Dead --> Respawning : Respawn
    Respawning --> Idle : Complete
```

---

# 6. Complex Sequence Diagram

```mermaid
sequenceDiagram

    actor Player

    participant UI
    participant GameManager
    participant Matchmaking
    participant Server
    participant Database

    Player->>UI: Press Play

    UI->>GameManager: StartMatch()

    GameManager->>Matchmaking: FindMatch()

    Matchmaking->>Server: Request Match

    Server->>Database: Check Players

    Database-->>Server: Player Data

    Server-->>Matchmaking: Match Found

    Matchmaking-->>GameManager: Match Ready

    GameManager-->>UI: Loading

    UI-->>Player: Loading Screen

    GameManager->>Server: Join Match

    Server-->>GameManager: Connected

    GameManager-->>UI: Start Game

    UI-->>Player: Gameplay
```

---

# 7. Multiplayer Sequence

```mermaid
sequenceDiagram

    actor Client

    participant LocalPlayer
    participant Network
    participant Server
    participant RemotePlayer

    Client->>LocalPlayer: Press Shoot

    LocalPlayer->>Network: Send Shoot RPC

    Network->>Server: Shoot Request

    Server->>Server: Validate Shot

    Server->>Server: Calculate Hit

    Server->>Network: Damage Event

    Network->>RemotePlayer: Apply Damage

    RemotePlayer-->>Client: Hit Feedback
```

---

# 8. Class Diagram

```mermaid
classDiagram

    class Player {
        +string Name
        +float Speed
        +Move()
        +Attack()
        +TakeDamage()
    }

    class Weapon {
        <<abstract>>
        +int Damage
        +float Range
        +Fire()
    }

    class Gun {
        +int MagazineSize
        +Reload()
        +Fire()
    }

    class Sword {
        +float AttackSpeed
        +Swing()
    }

    class Health {
        +float Current
        +float Maximum
        +TakeDamage()
        +Heal()
        +Die()
    }

    class Inventory {
        +AddItem()
        +RemoveItem()
        +HasItem()
    }

    Player --> Weapon
    Player --> Health
    Player --> Inventory

    Weapon <|-- Gun
    Weapon <|-- Sword
```

---

# 9. Larger Class Architecture

```mermaid
classDiagram

    class GameManager {
        +StartGame()
        +EndGame()
        +PauseGame()
    }

    class PlayerController {
        +Move()
        +Look()
        +Jump()
    }

    class CombatSystem {
        +Attack()
        +CalculateDamage()
    }

    class HealthSystem {
        +TakeDamage()
        +Heal()
        +Die()
    }

    class InventorySystem {
        +AddItem()
        +RemoveItem()
    }

    class WeaponSystem {
        +Equip()
        +Unequip()
        +Fire()
    }

    class EventBus {
        +Publish()
        +Subscribe()
        +Unsubscribe()
    }

    GameManager --> EventBus

    PlayerController --> CombatSystem
    PlayerController --> InventorySystem

    CombatSystem --> WeaponSystem
    CombatSystem --> HealthSystem

    WeaponSystem --> EventBus
    HealthSystem --> EventBus
```

---

# 10. Entity Relationship Diagram

```mermaid
erDiagram

    PLAYER {
        int id PK
        string username
        int level
        int experience
    }

    INVENTORY {
        int id PK
        int player_id FK
    }

    ITEM {
        int id PK
        string name
        string type
        int value
    }

    WEAPON {
        int id PK
        string name
        int damage
        float range
    }

    PLAYER ||--|| INVENTORY : owns

    INVENTORY ||--o{ ITEM : contains

    PLAYER ||--o{ WEAPON : owns
```

---

# 11. Database Architecture

```mermaid
erDiagram

    USER ||--o{ SESSION : creates

    USER {
        int id PK
        string username
        string email
    }

    SESSION {
        int id PK
        int user_id FK
        datetime created_at
        datetime expires_at
    }

    SESSION ||--o{ MATCH : joins

    MATCH {
        int id PK
        string status
        datetime started_at
    }

    MATCH ||--o{ PLAYER_MATCH : contains

    PLAYER_MATCH {
        int player_id FK
        int match_id FK
        int score
    }
```

---

# 12. Decision Tree

```mermaid
flowchart TD

    Start(["Start"])

    Start --> CheckHealth{"Health > 0?"}

    CheckHealth -->|No| Dead["Dead"]
    CheckHealth -->|Yes| CheckEnemy{"Enemy Nearby?"}

    CheckEnemy -->|No| Explore["Explore"]

    CheckEnemy -->|Yes| CheckAmmo{"Ammo Available?"}

    CheckAmmo -->|Yes| Attack["Attack"]
    CheckAmmo -->|No| Reload["Reload"]

    Reload --> Attack

    Attack --> CheckHealth

    Explore --> CheckEnemy

    Dead --> Respawn["Respawn"]

    Respawn --> Start
```

---

# 13. Complex Game Loop

```mermaid
flowchart TD

    Start([Game Start])

    Start --> Init["Initialize"]

    Init --> Input["Read Input"]

    Input --> Update["Update Gameplay"]

    Update --> Physics["Physics Simulation"]

    Physics --> Collision["Collision Detection"]

    Collision --> Gameplay["Gameplay Resolution"]

    Gameplay --> Animation["Update Animation"]

    Animation --> Audio["Update Audio"]

    Audio --> Render["Render Frame"]

    Render --> Input
```

---

# 14. Data Flow Diagram

```mermaid
flowchart LR

    Input["Player Input"]
    InputReader["Input Reader"]
    Controller["Controller"]
    Gameplay["Gameplay"]
    Events["Events"]
    UI["UI"]
    Audio["Audio"]
    Network["Network"]

    Input --> InputReader
    InputReader --> Controller
    Controller --> Gameplay

    Gameplay --> Events

    Events --> UI
    Events --> Audio
    Events --> Network
```

---

# 15. Event-Driven Architecture

```mermaid
flowchart TB

    Player["Player"]

    Combat["Combat"]
    Movement["Movement"]
    Inventory["Inventory"]

    EventBus["Event Bus"]

    UI["UI"]
    Audio["Audio"]
    Analytics["Analytics"]
    Achievements["Achievements"]

    Player --> Combat
    Player --> Movement
    Player --> Inventory

    Combat --> EventBus
    Movement --> EventBus
    Inventory --> EventBus

    EventBus --> UI
    EventBus --> Audio
    EventBus --> Analytics
    EventBus --> Achievements
```

---

# 16. Microservice Architecture

```mermaid
flowchart TB

    Client["Client"]

    Gateway["API Gateway"]

    Auth["Auth Service"]
    Player["Player Service"]
    Match["Matchmaking Service"]
    Game["Game Service"]
    Chat["Chat Service"]

    DB1[("Auth DB")]
    DB2[("Player DB")]
    DB3[("Game DB")]

    Client --> Gateway

    Gateway --> Auth
    Gateway --> Player
    Gateway --> Match
    Gateway --> Game
    Gateway --> Chat

    Auth --> DB1
    Player --> DB2
    Game --> DB3

    Match --> Game
```

---

# 17. System Initialization

```mermaid
sequenceDiagram

    participant Boot
    participant Config
    participant Services
    participant Systems
    participant UI
    participant Game

    Boot->>Config: Load Configuration

    Config-->>Boot: Configuration Ready

    Boot->>Services: Initialize Services

    Services-->>Boot: Services Ready

    Boot->>Systems: Initialize Systems

    Systems-->>Boot: Systems Ready

    Boot->>UI: Initialize UI

    UI-->>Boot: UI Ready

    Boot->>Game: Start Game

    Game-->>Boot: Running
```

---

# 18. Object Lifecycle

```mermaid
stateDiagram-v2

    [*] --> Created

    Created --> Initialized

    Initialized --> Active

    Active --> Disabled

    Disabled --> Active

    Active --> Destroyed

    Disabled --> Destroyed

    Destroyed --> [*]
```

---

# 19. CI/CD Pipeline

```mermaid
flowchart LR

    Code["Code"]

    Commit["Git Commit"]

    Build["Build"]

    Tests["Tests"]

    Package["Package"]

    Deploy["Deploy"]

    Production["Production"]

    Code --> Commit
    Commit --> Build
    Build --> Tests

    Tests -->|Pass| Package
    Tests -->|Fail| Fix["Fix"]

    Fix --> Code

    Package --> Deploy
    Deploy --> Production
```

---

# 20. Git Workflow

```mermaid
gitGraph

    commit id: "Initial"

    branch develop

    checkout develop
    commit id: "Feature A"
    commit id: "Feature B"

    branch feature-combat

    checkout feature-combat
    commit id: "Combat"
    commit id: "Combat Tests"

    checkout develop
    merge feature-combat

    commit id: "Integration"

    checkout main
    merge develop

    commit id: "Release"
```

---

# 21. Project Timeline

```mermaid
timeline

    title Game Development Timeline

    2026 : Prototype
         : Core Systems
         : First Playable

    2027 : Vertical Slice
         : Multiplayer
         : Content Production

    2028 : Alpha
         : Testing
         : Optimization

    2029 : Beta
         : Polish
         : Marketing

    2030 : Release
```

---

# 22. Gantt Chart

```mermaid
gantt

    title Game Development

    dateFormat YYYY-MM-DD

    section Design

    Game Design :design, 2026-10-01, 20d

    section Programming

    Core Systems :core, 2026-10-10, 30d

    Gameplay :gameplay, after core, 40d

    Multiplayer :network, after gameplay, 30d

    section Art

    Environment :environment, 2026-10-20, 40d

    Characters :characters, 2026-11-01, 30d

    section Testing

    QA :qa, after network, 20d
```

---

# 23. Mind Map

```mermaid
mindmap

    root((Game))

        Gameplay
            Movement
            Combat
            Inventory
            AI

        Systems
            Input
            Audio
            Save
            Networking

        UI
            HUD
            Menus
            Settings
            Inventory

        Content
            Levels
            Characters
            Weapons
            Enemies
```

---

# 24. Large Mind Map

```mermaid
mindmap

    root((Software Engineering))

        Architecture
            Layers
            Modules
            Dependencies
            Interfaces

        Programming
            Algorithms
            Data Structures
            OOP
            Functional

        Infrastructure
            Networking
            Databases
            Cloud
            CI/CD

        Testing
            Unit
            Integration
            System
            Performance

        Operations
            Logging
            Monitoring
            Deployment
            Backups
```

---

# 25. Quadrant Chart

```mermaid
quadrantChart

    title Technology Priority

    x-axis Low Complexity --> High Complexity

    y-axis Low Value --> High Value

    quadrant-1 Strategic
    quadrant-2 Quick Wins
    quadrant-3 Low Priority
    quadrant-4 Avoid

    AI Tools: [0.8, 0.9]
    Automation: [0.7, 0.8]
    Documentation: [0.2, 0.6]
    Refactoring: [0.5, 0.7]
    Manual Tasks: [0.3, 0.2]
```

---

# 26. XY Chart

```mermaid
xychart-beta

    title "Performance"

    x-axis ["Jan", "Feb", "Mar", "Apr", "May"]

    y-axis "FPS" 0 --> 120

    line [60, 65, 72, 90, 110]

    bar [55, 70, 68, 85, 105]
```

---

# 27. Pie Chart

```mermaid
pie title Development Time

    "Programming" : 40
    "Design" : 20
    "Art" : 20
    "Testing" : 15
    "Documentation" : 5
```

---

# 28. Block Diagram

```mermaid
block-beta

    columns 3

    Input["Input"]
    Controller["Controller"]
    Gameplay["Gameplay"]

    Input --> Controller
    Controller --> Gameplay

    space

    Physics["Physics"]
    Combat["Combat"]
    UI["UI"]

    Gameplay --> Physics
    Gameplay --> Combat
    Gameplay --> UI
```

---

# 29. Complex Block Architecture

```mermaid
block-beta

    columns 4

    block:client
        UI
        Input
    end

    block:game
        Controller
        Gameplay
        Physics
        Combat
    end

    block:services
        Save
        Audio
        Network
    end

    block:data
        Database
    end

    Input --> Controller
    Controller --> Gameplay
    Gameplay --> Physics
    Gameplay --> Combat

    Gameplay --> Save
    Gameplay --> Audio
    Gameplay --> Network

    Save --> Database
```

---

# 30. Sankey Diagram

```mermaid
sankey-beta

    Input,Gameplay,40
    Input,UI,20
    Input,Debug,10

    Gameplay,Combat,20
    Gameplay,Movement,15
    Gameplay,AI,5

    UI,HUD,12
    UI,Menus,8
```

---

# 31. Requirement Flow

```mermaid
flowchart TD

    Requirement["Requirement"]

    Requirement --> Design["System Design"]

    Design --> Prototype["Prototype"]

    Prototype --> Test["Testing"]

    Test -->|Pass| Implementation["Implementation"]

    Test -->|Fail| Redesign["Redesign"]

    Redesign --> Prototype

    Implementation --> Integration["Integration"]

    Integration --> Release["Release"]
```

---

# 32. Debugging Flow

```mermaid
flowchart TD

    Bug["Bug Report"]

    Bug --> Reproduce{"Can Reproduce?"}

    Reproduce -->|No| Logs["Inspect Logs"]
    Reproduce -->|Yes| Isolate["Isolate System"]

    Logs --> Reproduce

    Isolate --> Cause{"Root Cause Found?"}

    Cause -->|No| Instrument["Add Instrumentation"]
    Instrument --> Isolate

    Cause -->|Yes| Fix["Implement Fix"]

    Fix --> Test["Run Tests"]

    Test -->|Pass| Deploy["Deploy"]
    Test -->|Fail| Isolate
```

---

# 33. Multiplayer Architecture

```mermaid
flowchart TB

    subgraph ClientA["Client A"]

        InputA["Input"]
        PlayerA["Player"]
        UIA["UI"]
    end

    subgraph Server["Dedicated Server"]

        Authority["Game Authority"]
        Simulation["Simulation"]
        Validation["Validation"]
    end

    subgraph ClientB["Client B"]

        InputB["Input"]
        PlayerB["Player"]
        UIB["UI"]
    end

    InputA --> PlayerA
    PlayerA --> Authority

    InputB --> PlayerB
    PlayerB --> Authority

    Authority --> Validation
    Validation --> Simulation

    Simulation --> PlayerA
    Simulation --> PlayerB

    Simulation --> UIA
    Simulation --> UIB
```

---

# 34. Save System

```mermaid
flowchart LR

    Gameplay["Gameplay"]

    SaveRequest["Save Request"]

    Serializer["Serializer"]

    File["Save File"]

    Loader["Loader"]

    Deserializer["Deserializer"]

    GameState["Game State"]

    Gameplay --> SaveRequest
    SaveRequest --> Serializer
    Serializer --> File

    File --> Loader
    Loader --> Deserializer
    Deserializer --> GameState
```

---

# 35. Dependency Injection

```mermaid
flowchart TB

    Bootstrap["Bootstrap"]

    Container["DI Container"]

    Input["Input Service"]
    Audio["Audio Service"]
    Save["Save Service"]
    Network["Network Service"]

    Player["Player"]
    UI["UI"]
    GameManager["Game Manager"]

    Bootstrap --> Container

    Container --> Input
    Container --> Audio
    Container --> Save
    Container --> Network

    Input --> Player
    Audio --> Player

    Save --> GameManager
    Network --> GameManager

    Audio --> UI
```

---

# 36. Observer Pattern

```mermaid
flowchart LR

    Subject["Subject"]

    Event["Event"]

    ObserverA["UI"]
    ObserverB["Audio"]
    ObserverC["Analytics"]

    Subject --> Event

    Event --> ObserverA
    Event --> ObserverB
    Event --> ObserverC
```

---

# 37. Object Pool

```mermaid
stateDiagram-v2

    [*] --> Pool

    Pool --> Available

    Available --> Spawned : Get()

    Spawned --> Active

    Active --> Returned : Release()

    Returned --> Available

    Available --> Destroyed : Shutdown()

    Destroyed --> [*]
```

---

# 38. Factory Pattern

```mermaid
flowchart LR

    Request["Create Request"]

    Factory["Factory"]

    Factory --> Player["Player"]
    Factory --> Enemy["Enemy"]
    Factory --> Projectile["Projectile"]

    Request --> Factory
```

---

# 39. ECS-style Architecture

```mermaid
flowchart TB

    Entity["Entity"]

    Components["Components"]

    Systems["Systems"]

    Entity --> Components

    Components --> Transform["Transform"]
    Components --> Velocity["Velocity"]
    Components --> Health["Health"]

    Systems --> Movement["Movement System"]
    Systems --> Combat["Combat System"]
    Systems --> Rendering["Rendering System"]

    Transform --> Movement
    Velocity --> Movement

    Health --> Combat

    Transform --> Rendering
```

---

# 40. Full Game Architecture

```mermaid
flowchart TB

    subgraph Presentation["Presentation"]

        UI["UI"]

        HUD["HUD"]

        Menus["Menus"]

        VFX["VFX"]

        Audio["Audio"]
    end

    subgraph Gameplay["Gameplay"]

        Player["Player"]

        Combat["Combat"]

        Movement["Movement"]

        AI["AI"]

        Inventory["Inventory"]
    end

    subgraph Core["Core"]

        EventBus["Event Bus"]

        GameState["Game State"]

        TimeController["Time Controller"]

        Config["Configuration"]
    end

    subgraph Infrastructure["Infrastructure"]

        Input["Input"]

        Network["Networking"]

        Save["Save"]

        Analytics["Analytics"]
    end

    subgraph External["External"]

        Server["Game Server"]

        Database[("Database")]

        Cloud["Cloud Services"]
    end

    Input --> Player

    UI --> GameState
    Menus --> GameState

    Player --> Movement
    Player --> Combat
    Player --> Inventory

    AI --> Combat

    Combat --> EventBus
    Movement --> EventBus
    Inventory --> EventBus

    EventBus --> UI
    EventBus --> HUD
    EventBus --> VFX
    EventBus --> Audio

    EventBus --> GameState

    GameState --> Save
    GameState --> Analytics

    Player --> Network
    Network --> Server

    Save --> Database
    Analytics --> Cloud
```
1. 