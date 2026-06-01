erDiagram
  users {
    uuid id PK
    varchar email UK
    varchar password_hash
    varchar first_name
    varchar last_name
    text avatar
    text cover_photo
    text bio
    timestamp created_at
    timestamp updated_at
  }

  refresh_tokens {
    uuid id PK
    uuid user_id FK
    text token
    timestamp expires_at
    timestamp created_at
  }

  posts {
    uuid id PK
    uuid author_id FK
    text content
    varchar privacy "public|friends|private"
    int like_count
    int comment_count
    timestamp created_at
    timestamp updated_at
  }

  post_media {
    uuid id PK
    uuid post_id FK
    text media_url
    varchar media_type "image|video"
    int order_index
  }

  comments {
    uuid id PK
    uuid post_id FK
    uuid author_id FK
    uuid parent_id FK "null = comment gốc"
    text content
    timestamp created_at
  }

  post_likes {
    uuid id PK
    uuid post_id FK
    uuid user_id FK
    timestamp created_at
  }

  friend_requests {
    uuid id PK
    uuid sender_id FK
    uuid receiver_id FK
    varchar status "pending|accepted|rejected"
    timestamp created_at
    timestamp updated_at
  }

  friendships {
    uuid id PK
    uuid user_id FK
    uuid friend_id FK
    timestamp created_at
  }

  notifications {
    uuid id PK
    uuid user_id FK "người nhận"
    uuid sender_id FK
    varchar type "LIKE|COMMENT|FRIEND_REQUEST|FRIEND_ACCEPT"
    text content
    uuid reference_id
    varchar reference_type
    boolean is_read
    timestamp created_at
  }

  conversations {
    uuid id PK
    timestamp created_at
    timestamp updated_at
  }

  conversation_participants {
    uuid conversation_id FK
    uuid user_id FK
    string note "Composite PK: (conversation_id, user_id)"
  }

  messages {
    uuid id PK
    uuid conversation_id FK
    uuid sender_id FK
    text content
    text media_url
    varchar type "text|image|video"
    boolean is_read
    timestamp created_at
  }

  %% Relationships
  users ||--o{ refresh_tokens : "has"
  users ||--o{ posts : "creates"
  users ||--o{ comments : "writes"
  users ||--o{ post_likes : "likes"
  users ||--o{ friend_requests : "sends"
  users ||--o{ friend_requests : "receives"
  users ||--o{ friendships : "has"
  users ||--o{ notifications : "receives"
  users ||--o{ conversation_participants : "participates"
  users ||--o{ messages : "sends"
  
  posts ||--o{ post_media : "has"
  posts ||--o{ comments : "has"
  posts ||--o{ post_likes : "has"
  
  conversations ||--o{ conversation_participants : "includes"
  conversations ||--o{ messages : "contains"
