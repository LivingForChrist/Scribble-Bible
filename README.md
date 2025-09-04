# 📖 Bible Drawing & Notes App – Project Plan

## 1. 🎯 Project Overview

A cross-platform mobile app (Android & iOS) focused on **Bible reading, drawing, highlighting, and note-taking**, optimized for tablets but functional on phones.

**Key Features:**

* Local-first storage (Bible text + user data).
* Drawing & highlighting with multiple pen types (stylus + touch support).
* Offline functionality with optional cloud sync.
* Text-to-Speech (TTS) for Bible verses/chapters.
* Tablet-friendly layout: Bible text on left, notes/drawing on right.

---

## 2. 🛠️ Tech Stack

| Layer               | Recommendation                                                | Notes                                                                                   |
| ------------------- | ------------------------------------------------------------- | --------------------------------------------------------------------------------------- |
| **Framework**       | **Flutter**                                                   | Smooth drawing (CustomPaint, pencil\_field), single codebase, strong tablet UI support. |
| **UI Toolkit**      | Material 3 / Cupertino                                        | Native feel on both platforms.                                                          |
| **Drawing/Canvas**  | `pencil_field`, `custom_paint`                                | Vector strokes, pressure-sensitive, undo/redo.                                          |
| **Storage**         | `sqflite` (SQLite)                                            | Offline-first database for notes/drawings.                                              |
| **Sync (optional)** | Firebase Firestore (with offline cache)                       | Seamless cloud sync.                                                                    |
| **Bible Text**      | Free/public domain (KJV, ASV) from GitHub/SWORD modules       | Download once + cache locally.                                                          |
| **TTS**             | Platform APIs (AVSpeechSynthesizer iOS, TextToSpeech Android) | Works offline, no large data files.                                                     |

---

## 3. 📐 Architecture

### Data Flow

1. **Bible Text** – Downloaded/stored locally in SQLite or JSON files.
2. **Notes/Drawings** – Stored as vector data (paths, metadata) in SQLite.
3. **Sync** – If enabled, Firestore mirrors user notes/drawings.
4. **TTS** – Uses local platform API to read verses aloud.

### Storage Model (simplified)

```plaintext
BibleVerse {
  id: string (book+chapter+verse)
  text: string
}

UserNote {
  id: uuid
  verseId: string
  text: string
  createdAt: datetime
}

UserDrawing {
  id: uuid
  verseId: string
  strokes: vectorPath[]
  createdAt: datetime
}
```

---

## 4. 📲 Features Breakdown

### MVP (Phase 1)

* [ ] Bible text viewer (download + offline).
* [ ] Note-taking panel (plain text).
* [ ] Basic drawing (pen, eraser, highlight).
* [ ] Save notes/drawings locally.

### Phase 2

* [ ] Pressure-sensitive pens (stylus support).
* [ ] Undo/redo + different brush types.
* [ ] Sync to Firebase (optional).
* [ ] Highlight verses directly in text.

### Phase 3

* [ ] TTS integration (play/pause, verse-by-verse).
* [ ] Multiple translations (downloadable).
* [ ] Tablet-optimized split view (Bible left, notes/drawings right).
* [ ] Export notes/drawings (PDF/PNG).

---

## 5. 🎨 UI/UX Design

* **Tablet Layout**:

  * Left: Bible text (scrollable, search).
  * Right: Notes/drawing canvas with toolbar.
* **Phone Layout**:

  * Toggle between text and notes/drawing.
* **Toolbar**: Pen selector, color picker, eraser, undo/redo, save.
* **Dark/Light Mode**: Toggle for better readability.

---

## 6. 🚀 Development Milestones

1. **Setup & Infrastructure** (1–2 weeks)

   * Flutter project setup, Bible text loading, SQLite integration.

2. **MVP Build** (3–4 weeks)

   * Bible viewer, note editor, basic drawing.

3. **Enhanced Drawing & Notes** (3 weeks)

   * Multiple pens, pressure support, undo/redo.

4. **Sync & TTS** (3 weeks)

   * Firebase sync, text-to-speech integration.

5. **Polish & Release** (2 weeks)

   * UI improvements, testing on tablets/phones, app store deployment.

---

## 7. 📂 Future Enhancements

* Handwriting-to-text (iOS PencilKit + Android Ink).
* Verse tagging + cross-references.
* Audio Bible integration.
* Cloud export/import of all notes.
* Collaborative Bible study mode (shared notes).

---


# 📂 Flutter Project Structure

```plaintext
/lib
 ├── main.dart                # App entry point
 ├── app.dart                 # Root widget (theme, routes)
 │
 ├── core/                    # Core app logic
 │   ├── constants/            # App-wide constants (colors, fonts, configs)
 │   ├── utils/                # Helper functions (formatting, logging, etc.)
 │   └── theme/                # Theme files (light/dark mode, styles)
 │
 ├── models/                  # Data models (POJOs)
 │   ├── bible_verse.dart
 │   ├── user_note.dart
 │   └── user_drawing.dart
 │
 ├── services/                # App services (data access, APIs)
 │   ├── bible_service.dart    # Load/download Bible text
 │   ├── storage_service.dart  # Local SQLite storage
 │   ├── sync_service.dart     # Firebase sync (optional)
 │   ├── tts_service.dart      # Text-to-Speech wrapper
 │   └── drawing_service.dart  # Handle vector path storage
 │
 ├── providers/               # State management (Riverpod/Provider/Bloc)
 │   ├── bible_provider.dart
 │   ├── note_provider.dart
 │   └── drawing_provider.dart
 │
 ├── screens/                 # App screens
 │   ├── home_screen.dart      # Main entry (split layout for tablets)
 │   ├── bible_screen.dart     # Bible text reader
 │   ├── notes_screen.dart     # Notes editor
 │   ├── drawing_screen.dart   # Drawing canvas
 │   └── settings_screen.dart  # App settings (theme, sync, etc.)
 │
 ├── widgets/                 # Reusable UI components
 │   ├── verse_tile.dart
 │   ├── drawing_toolbar.dart
 │   ├── note_card.dart
 │   └── custom_button.dart
 │
 └── data/                    # Preloaded data (optional)
     └── bible/                # Local Bible JSON/SQLite (if bundled)
```

---

## 🛠️ Notes on Usage

* **`models/`** → defines BibleVerse, UserNote, UserDrawing.
* **`services/`** → all “backend” logic (Bible download, SQLite CRUD, Firebase sync).
* **`providers/`** → state management (Riverpod recommended for scalability).
* **`screens/`** → full-screen widgets, each handling one major feature.
* **`widgets/`** → small, reusable UI parts (verse list items, toolbars).
* **`core/`** → keep constants, themes, helpers centralized.

---

⚡ This structure ensures:

* Bible reading, notes, and drawing features are **separated but connected**.
* Easy future expansion (new screens/services won’t clutter main code).
* Clean dependency flow: UI → Providers → Services → Models → Storage/API.
