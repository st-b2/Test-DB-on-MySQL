# 🗄️ Test MySQL Database Dump

This repository contains a **test database dump** created while learning SQL and MySQL. The database mimics the structure of a typical **messenger** or **social application** with users, channels, groups, messages, reactions, and more.

---

## 📚 Table of Contents

- [📖 Database Description](#-database-description)
- [📂 Database Structure](#-database-structure)
  - [👤 Users](#-users)
  - [📢 Channels](#-channels)
  - [💬 Groups](#-groups)
  - [📨 Messages](#-messages)
  - [📎 Media Attachments](#-media-attachments)
  - [❤️ Reactions](#️-reactions)
  - [⚙️ Settings](#️-settings)
  - [📋 Stories](#-stories)
- [📊 Database Schema](#-database-schema)

---

## 📖 Database Description

This database was created as part of a MySQL course and includes the core entities needed for a modern messenger or social app:

- **Users** — registration, profiles, settings.
- **Channels** — public/private channels for broadcasting.
- **Groups** — chats with multiple participants.
- **Messages** — text messages with media attachments.
- **Reactions** — likes and other reactions to messages.
- **Settings** — user preferences (theme, language, notifications).

---

## 📂 Database Structure

### 👤 Users (`users`)

Stores information about registered users.

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique user identifier |
| `firstname` | VARCHAR(50) | User's first name |
| `lastname` | VARCHAR(50) | User's last name |
| `email` | VARCHAR(100) | Email (unique) |
| `password_hash` | VARCHAR(255) | Password hash |
| `phone` | VARCHAR(20) | Phone number |
| `birthday` | DATE | Date of birth |
| `created_at` | DATETIME | Registration date |
| `is_premium_account` | BOOLEAN | Premium account flag |

---

### 📢 Channels (`channels`)

Public or private channels for content broadcasting.

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique channel identifier |
| `title` | VARCHAR(100) | Channel name |
| `owner_user_id` | INT (FK) | Channel creator |
| `is_private` | BOOLEAN | Privacy flag |
| `invite_link` | VARCHAR(255) | Invitation link |
| `created_at` | DATETIME | Creation date |

---

### 💬 Groups (`groups`)

Chats with multiple participants.

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique group identifier |
| `title` | VARCHAR(100) | Group name |
| `created_at` | DATETIME | Creation date |

---

### 👥 Group Members (`group_members`)

Many-to-many relationship between users and groups.

| Field | Type | Description |
| :--- | :--- | :--- |
| `group_id` | INT (FK) | Group ID |
| `user_id` | INT (FK) | User ID |
| `created_at` | DATETIME | Join date |

---

### 📨 Messages

#### Channels — `channel_messages`

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique message ID |
| `channel_id` | INT (FK) | Channel ID |
| `sender_id` | INT (FK) | Sender |
| `body` | TEXT | Message text |
| `media_type` | VARCHAR(20) | Attachment type (image, video, etc.) |
| `filename` | VARCHAR(255) | File name |
| `views_count` | INT | View count |
| `code` | TEXT | Code (if applicable) |
| `created_at` | DATETIME | Sent date |

#### Groups — `group_messages`

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique message ID |
| `group_id` | INT (FK) | Group ID |
| `sender_id` | INT (FK) | Sender |
| `body` | TEXT | Message text |
| `media_type` | VARCHAR(20) | Attachment type |
| `filename` | VARCHAR(255) | File name |
| `reply_to_id` | INT (FK) | Reply to another message |
| `created_at` | DATETIME | Sent date |

#### Private — `private_messages`

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique message ID |
| `sender_id` | INT (FK) | Sender |
| `receiver_id` | INT (FK) | Receiver |
| `body` | TEXT | Message text |
| `media_type` | VARCHAR(20) | Attachment type |
| `filename` | VARCHAR(255) | File name |
| `reply_to_id` | INT (FK) | Reply to another message |
| `is_read` | BOOLEAN | Read/Unread status |
| `created_at` | DATETIME | Sent date |

---

### ❤️ Reactions

#### Channel Message Reactions — `channel_message_reactions`

| Field | Type | Description |
| :--- | :--- | :--- |
| `message_id` | INT (FK) | Message ID |
| `user_id` | INT (FK) | User |
| `reaction_id` | INT (FK) | Reaction ID |
| `created_at` | DATETIME | Reaction date |

#### Group Message Reactions — `group_message_reactions`

| Field | Type | Description |
| :--- | :--- | :--- |
| `message_id` | INT (FK) | Message ID |
| `user_id` | INT (FK) | User |
| `reaction_id` | INT (FK) | Reaction ID |
| `created_at` | DATETIME | Reaction date |

#### Private Message Reactions — `private_message_reactions`

| Field | Type | Description |
| :--- | :--- | :--- |
| `message_id` | INT (FK) | Message ID |
| `user_id` | INT (FK) | User |
| `reaction_id` | INT (FK) | Reaction ID |
| `created_at` | DATETIME | Reaction date |

---

### ⚙️ Settings (`settings`)

Stores user preferences.

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique ID |
| `user_id` | INT (FK) | User |
| `app_language` | VARCHAR(10) | Interface language |
| `color_scheme` | VARCHAR(20) | Color scheme |
| `is_night_mode_enabled` | BOOLEAN | Night mode |
| `notifications_and_sounds` | JSON | Notification settings |
| `status` | VARCHAR(50) | User status |
| `status_text` | TEXT | Status text |
| `icon` | VARCHAR(50) | Status icon |
| `created_at` | DATETIME | Creation date |
| `updated_at` | DATETIME | Last update date |

---

### 📎 Media Attachments (`media`)

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique ID |
| `filename` | VARCHAR(255) | File name |
| `media_type` | VARCHAR(20) | Type (image, video, audio) |
| `created_at` | DATETIME | Upload date |

---

### 📋 Stories (`stories` and `stories_likes`)

#### Stories (`stories`)

| Field | Type | Description |
| :--- | :--- | :--- |
| `id` | INT (PK) | Unique ID |
| `user_id` | INT (FK) | Author |
| `caption` | TEXT | Description |
| `filename` | VARCHAR(255) | Story file |
| `views_count` | INT | View count |
| `created_at` | DATETIME | Publication date |

#### Story Likes (`stories_likes`)

| Field | Type | Description |
| :--- | :--- | :--- |
| `story_id` | INT (FK) | Story ID |
| `user_id` | INT (FK) | User |
| `created_at` | DATETIME | Like date |

---

## 📊 Database Schema

![Database Schema](https://github.com/user-attachments/assets/43766bfe-f9ef-4c04-8f83-75fe8889d8de)

---
