```mermaid
erDiagram
    users {
        uuid id PK
        varchar_255 email "UNIQUE, NOT NULL"
        varchar_20 phone_number "UNIQUE"
        varchar_255 password_hash
        varchar_50 username "UNIQUE"
        varchar_50 first_name "NOT NULL"
        varchar_50 last_name "NOT NULL"
        date date_of_birth "NOT NULL"
        gender_type gender "NOT NULL"
        orientation_type orientation
        text bio
        varchar_100 occupation
        numeric_9_6 location_lat
        numeric_9_6 location_lon
        boolean is_verified "NOT NULL, DEFAULT FALSE"
        boolean is_active "NOT NULL, DEFAULT TRUE"
        timestamp last_active_at
        integer daily_likes_used "NOT NULL, DEFAULT 0"
        integer daily_likes_limit "NOT NULL, DEFAULT 50"
        text_array selected_interests
        boolean is_private "NOT NULL, DEFAULT FALSE"
        boolean show_age "NOT NULL, DEFAULT TRUE"
        boolean show_location "NOT NULL, DEFAULT TRUE"
        boolean show_last_active "NOT NULL, DEFAULT TRUE"
        timestamp created_at "NOT NULL"
        timestamp updated_at "NOT NULL"
        timestamp deleted_at
    }

    file_uploads {
        uuid id PK
        uuid user_id FK
        file_type file_type "NOT NULL"
        varchar_255 url "NOT NULL"
        varchar_255 thumbnail_url
        file_usage_status status "NOT NULL, DEFAULT 'active'"
        timestamp created_at "NOT NULL"
        timestamp updated_at "NOT NULL"
        timestamp deleted_at
    }

    profile_photos {
        uuid user_id PK, FK
        uuid file_upload_id PK, FK
        integer order_index "NOT NULL, DEFAULT 0"
        boolean is_primary "NOT NULL, DEFAULT FALSE"
        timestamp created_at "NOT NULL"
        timestamp updated_at "NOT NULL"
    }

    swipes {
        uuid id PK
        uuid swiper_user_id FK "NOT NULL"
        uuid swiped_user_id FK "NOT NULL"
        swipe_action_type action "NOT NULL"
        timestamp swiped_at "NOT NULL"
    }

    matches {
        uuid id PK
        uuid user1_id FK "NOT NULL"
        uuid user2_id FK "NOT NULL"
        timestamp matched_at "NOT NULL"
        boolean is_active "NOT NULL, DEFAULT TRUE"
    }

    chat_rooms {
        uuid id PK
        uuid match_id FK "UNIQUE, NOT NULL"
        uuid user1_id FK "NOT NULL"
        uuid user2_id FK "NOT NULL"
        timestamp last_message_at
        timestamp created_at "NOT NULL"
        timestamp updated_at "NOT NULL"
    }

    messages {
        uuid id PK
        uuid chat_room_id FK "NOT NULL"
        uuid sender_id FK "NOT NULL"
        text content "NOT NULL"
        timestamp sent_at "NOT NULL"
        boolean is_read "NOT NULL, DEFAULT FALSE"
    }

    users ||--o{ file_uploads : ""
    users ||--o{ profile_photos : ""
    users ||--o{ swipes : ""
    users ||--o{ matches : ""
    users ||--o{ chat_rooms : ""
    users ||--o{ messages : ""
    
    file_uploads ||--o{ profile_photos : ""
    
    matches ||--|| chat_rooms : ""
    
    chat_rooms ||--o{ messages : ""