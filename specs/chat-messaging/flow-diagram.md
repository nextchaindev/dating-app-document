# Chat Messaging Flow Diagram

## Chat Messaging Flow

```mermaid
flowchart TD
    A[User opens chat section] --> B[Display conversation list]
    B --> C[Sort by recent activity]
    C --> D{Select conversation?}
    D -->|Yes| E[Open chat interface]
    D -->|No| F[Wait for user action]

    E --> G[Load message history]
    G --> H[Display messages with timestamps]
    H --> I{User action}
    I -->|Type message| J[Show typing indicator to recipient]
    I -->|Send message| K[Validate message content]
    I -->|Back to list| B

    J --> L[User sends message]
    L --> K
    K --> M{Message valid?}
    M -->|No| N[Show validation error]
    M -->|Yes| O[Send message in real-time]

    O --> P[Show sent indicator - single checkmark]
    P --> Q[Update conversation list]
    Q --> R{Message delivered?}
    R -->|Yes| S[Show delivered indicator - double checkmark]
    R -->|No| T[Show retry option]

    S --> U{Recipient reads message?}
    U -->|Yes| V[Show read indicator - colored checkmarks]
    U -->|No| W[Wait for read status]

    T --> X[Allow message retry]
    X --> O

    F --> Y[Show empty state or recent conversations]
    N --> I
```

## Real-time Message Delivery Flow

```mermaid
flowchart TD
    A[User sends message] --> B[Validate message content]
    B --> C{Content valid?}
    C -->|No| D[Show error message]
    C -->|Yes| E[Check recipient connection]

    E --> F{Recipient online?}
    F -->|Yes| G[Send via WebSocket]
    F -->|No| H[Queue for delivery]

    G --> I[Display message immediately]
    I --> J[Update message status to delivered]
    J --> K[Send delivery confirmation to sender]

    H --> L[Store in message queue]
    L --> M[Send push notification]
    M --> N[Wait for recipient to come online]
    N --> O{Recipient connects?}
    O -->|Yes| P[Deliver queued messages]
    O -->|No| Q[Keep in queue]

    P --> I

    K --> R[Show double checkmark to sender]

    I --> S{Recipient views message?}
    S -->|Yes| T[Mark as read]
    S -->|No| U[Keep as unread]

    T --> V[Send read receipt to sender]
    V --> W[Show colored checkmarks to sender]
```

## Conversation List Management Flow

```mermaid
flowchart TD
    A[User opens chat section] --> B[Load all conversations]
    B --> C[Sort by last message timestamp]
    C --> D[Display conversation previews]
    D --> E[Show match photo, name, last message]
    E --> F{New message received?}
    F -->|Yes| G[Move conversation to top]
    F -->|No| H[Maintain current order]

    G --> I[Show unread indicator]
    I --> J[Update last message preview]
    J --> K[Update timestamp]

    H --> L[Display existing conversations]

    K --> M[Refresh conversation list]
    L --> M
    M --> N{User taps conversation?}
    N -->|Yes| O[Open chat interface]
    N -->|No| P[Continue monitoring for updates]

    O --> Q[Mark conversation as read]
    Q --> R[Remove unread indicator]
    R --> S[Load full message history]

    P --> T[Listen for new messages]
    T --> F

    D --> U{No conversations exist?}
    U -->|Yes| V[Show empty state message]
    U -->|No| E

    V --> W[Encourage user to make more matches]
```

## Message Status Tracking Flow

```mermaid
flowchart TD
    A[Message sent by user] --> B[Set status: SENT]
    B --> C[Display single checkmark]
    C --> D[Attempt delivery to recipient]
    D --> E{Delivery successful?}
    E -->|Yes| F[Set status: DELIVERED]
    E -->|No| G[Set status: FAILED]

    F --> H[Display double checkmark]
    H --> I[Wait for read receipt]
    I --> J{Message read by recipient?}
    J -->|Yes| K[Set status: READ]
    J -->|No| L[Keep status as DELIVERED]

    K --> M[Display colored checkmarks]

    G --> N[Display error indicator]
    N --> O[Show retry button]
    O --> P{User retries?}
    P -->|Yes| Q[Attempt redelivery]
    P -->|No| R[Keep failed status]

    Q --> D

    L --> S[Continue monitoring for read status]
    S --> J
```

## Typing Indicator Flow

```mermaid
flowchart TD
    A[User starts typing] --> B[Detect typing activity]
    B --> C[Send typing indicator to recipient]
    C --> D[Show typing indicator in recipient's chat]
    D --> E[Set typing timeout - 3 seconds]
    E --> F{User continues typing?}
    F -->|Yes| G[Reset timeout]
    F -->|No| H[Stop typing indicator]

    G --> E
    H --> I[Send stop typing signal]
    I --> J[Hide typing indicator from recipient]

    A --> K{User sends message?}
    K -->|Yes| L[Immediately stop typing indicator]
    K -->|No| F

    L --> M[Send message]
    M --> N[Hide typing indicator]
```

## Use Case Diagram

```mermaid
graph LR
    User((User))
    Recipient((Match))
    System[Messaging System]
    RealtimeService[Real-time Service]
    NotificationService[Notification Service]
    MessageQueue[Message Queue]

    User --> |Send message| System
    User --> |View conversations| System
    User --> |Read messages| System
    User --> |Start typing| System

    System --> |Deliver message| RealtimeService
    System --> |Update message status| RealtimeService
    System --> |Send push notification| NotificationService
    System --> |Queue offline messages| MessageQueue
    System --> |Show typing indicator| RealtimeService

    RealtimeService --> |Real-time delivery| Recipient
    MessageQueue --> |Deliver when online| Recipient
```
