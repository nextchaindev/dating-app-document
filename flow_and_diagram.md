# Dating Platform Flows and Use Case Diagrams

## User Flow Diagrams

### User Signup Flow

```mermaid
flowchart TD
    A[User visits signup page] --> B[Fill email, password, confirm password]
    B --> C[Real-time validation]
    C --> D{Validation passed?}
    D -->|No| E[Show error messages] --> B
    D -->|Yes| F[Submit registration]

    F --> G{Email exists?}
    G -->|Yes| H[Show duplicate email error] --> B
    G -->|No| I[Create account]

    I --> J[Send confirmation email]
    J --> K[Profile setup wizard]
    K --> L[Enter name, age, gender, gender preference, bio, location]
    L --> M[Upload profile photos]
    M --> N[Redirect to main app]
```

### User Login Flow

```mermaid
flowchart TD
    A[User visits login page] --> B[Enter email and password]
    B --> C[Submit login]
    C --> D{Credentials valid?}
    D -->|No| E[Log failed attempt]
    E --> F[Show invalid credentials] --> B

    D -->|Yes| G[Generate JWT token]
    G --> H[Load user gender preferences]
    H --> I[Set session with preferences]
    I --> J[Log login event]
    J --> K[Redirect to discovery page]
    K --> L[Show matches based on gender preferences]
```

### Profile Management Flow

```mermaid
flowchart TD
    A[User accesses profile settings] --> B[Display current profile info]
    B --> C{What to update?}
    C -->|Basic Info| D[Edit name, age, bio, location]
    C -->|Photos| E[Manage photo gallery]
    C -->|Gender Preferences| F[Update gender and preferences]
    C -->|Privacy| G[Adjust visibility settings]

    D --> H[Validate and save changes]
    E --> I[Upload/delete/reorder photos]
    F --> J[Save preferences and refresh matches]
    G --> K[Apply privacy settings]

    H --> L[Show success confirmation]
    I --> L
    J --> L
    K --> L
    L --> M[Update profile timestamp]
```

### Match Discovery Flow

```mermaid
flowchart TD
    A[User opens discovery page] --> B[Load gender preferences]
    B --> C[Filter potential matches by gender preference]
    C --> D[Calculate matches by age range]
    D --> E{Matches available?}
    E -->|No| F[Show expand search message]
    E -->|Yes| G[Display match card]

    G --> H{User action}
    H -->|Swipe Right/Like| I[Record positive interest]
    H -->|Swipe Left/Pass| J[Record negative interest]
    H -->|Tap for Details| K[Show full profile]

    I --> L{Mutual like?}
    L -->|Yes| M[Create match and notify both users]
    L -->|No| N[Show next match]

    J --> N
    K --> O[View all photos and bio]
    O --> P{User action in detail view}
    P -->|Like| I
    P -->|Pass| J
    P -->|Close| G

    M --> Q[Add to matches list]
    N --> G
```

### Chat Messaging Flow

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
    K --> M[Send message in real-time]
    M --> N[Show sent indicator]
    N --> O[Update conversation list]
    O --> P{Message delivered?}
    P -->|Yes| Q[Show delivered indicator]
    P -->|No| R[Show retry option]

    Q --> S{Recipient reads message?}
    S -->|Yes| T[Show read indicator]
    S -->|No| U[Wait for read status]
```

## Use Case Diagrams

### User Authentication Use Cases

```mermaid
graph LR
    User((User))
    System[Authentication System]
    EmailService[Email Service]

    User --> |Register with email| System
    User --> |Login with credentials| System
    User --> |Reset password| System
    User --> |Logout| System

    System --> |Send confirmation| EmailService
    System --> |Send reset link| EmailService
```

### Profile Management Use Cases

```mermaid
graph LR
    User((User))
    System[Profile System]
    FileStorage[File Storage]
    MatchingEngine[Matching Engine]

    User --> |Update basic info| System
    User --> |Upload photos| System
    User --> |Update gender preferences| System
    User --> |Manage privacy settings| System

    System --> |Store photos| FileStorage
    System --> |Update matching criteria| MatchingEngine
    System --> |Validate profile data| System
```

### Match Discovery Use Cases

```mermaid
graph LR
    User((User))
    System[Discovery System]
    MatchingEngine[Matching Engine]
    NotificationService[Notification Service]

    User --> |View potential matches| System
    User --> |Like profile| System
    User --> |Pass on profile| System
    User --> |View detailed profile| System
    User --> |Apply filters| System

    System --> |Calculate compatibility| MatchingEngine
    System --> |Create mutual match| MatchingEngine
    System --> |Send match notification| NotificationService
```

### Chat Messaging Use Cases

```mermaid
graph LR
    User((User))
    Recipient((Match))
    System[Messaging System]
    RealtimeService[Real-time Service]
    NotificationService[Notification Service]

    User --> |Send message| System
    User --> |View conversations| System
    User --> |Read messages| System

    System --> |Deliver message| RealtimeService
    System --> |Update message status| RealtimeService
    System --> |Send push notification| NotificationService

    RealtimeService --> |Real-time delivery| Recipient
```

## Key User Personas

### Primary Actors

1. **User**: Individual looking for romantic connections through the dating platform
2. **Match**: Another user who appears in discovery or has mutual interest
3. **System**: Automated processes for matching, validation, and notifications

### Secondary Actors

1. **Email Service**: External service for sending verification and notifications

2. **File Storage**: Service for storing uploaded profile photos
3. **Notification Service**: System for sending match alerts and messages
4. **Real-time Service**: WebSocket service for instant messaging

## Security Considerations

### Authentication Security

- Password hashing using bcrypt with salt
- JWT tokens with 1-hour expiration and refresh tokens
- Failed login attempt logging for security monitoring
- Secure session management with gender preference data

### Data Protection

- HTTPS encryption for all endpoints
- Secure file upload validation for profile photos
- Personal data encryption at rest
- Audit logging for all authentication and matching events

### Privacy Protection

- Gender preference data stored securely
- Profile visibility controls
- User blocking and privacy settings
- Secure message delivery between matched users

### Content Moderation

- Photo upload validation and content filtering
- Inappropriate content flagging system
- User reporting mechanisms
- Automated content moderation for safety
