<div align="center">

# 💡 BrainStorm Bubble
### Real-Time Collaborative Idea Mapping with a Java Client–Server Architecture

A Java desktop application that lets multiple users **create, drag, and connect idea bubbles** on a shared canvas in real time — powered by a custom TCP client–server architecture, JavaFX GUI, and JSON messaging.

![Java](https://img.shields.io/badge/Java-17+-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![JavaFX](https://img.shields.io/badge/JavaFX-GUI-0078D7?style=for-the-badge&logo=java&logoColor=white)
![Networking](https://img.shields.io/badge/Architecture-Client--Server_TCP-CC0000?style=for-the-badge&logo=buffer&logoColor=white)
![JSON](https://img.shields.io/badge/Messaging-JSON-555555?style=for-the-badge&logo=json&logoColor=white)

</div>

---

## How It Works

1. The **server** launches and listens on port `8080` — maintaining a live `CanvasState` with all bubbles and connections
2. Each **client** connects via TCP socket, receives the current canvas state on join, and renders it in a JavaFX window
3. When a user creates, moves, or deletes a bubble, the client sends a **JSON message** to the server
4. The server updates its state and **broadcasts the change** to all other connected clients instantly
5. Every client's canvas stays in sync — bubbles appear, move, and disappear in real time across all windows

**Actions supported:** 💬 Set main idea · ➕ Add bubble · 🖱️ Drag to reposition · 🗑️ Clear all · 🔄 Auto-sync on join

---

## Screenshots

<table>
  <tr>
    <td align="center" width="33%">
      <img width="320" alt="Client UI - Bubble Canvas" src="https://github.com/user-attachments/assets/e1171589-896f-4a50-8b60-2d8faf01fc17" />
      <br />
      <sub><b>Main brainstorming canvas</b> — interactive bubble interface for creating and organizing ideas.</sub>
    </td>
    <td align="center" width="33%">
      <img width="320" alt="Add Bubble / Input Flow" src="https://github.com/user-attachments/assets/edd41501-9a84-44c8-8b3b-cafe22125213" />
      <br />
      <sub><b>Idea input flow</b> — prompts users to add new bubbles and labels quickly.</sub>
    </td>
    <td align="center" width="33%">
      <img width="320" alt="Connected Bubbles View" src="https://github.com/user-attachments/assets/23e1778b-df83-40f6-bf76-fb71536036a8" />
      <br />
      <sub><b>Connections view</b> — shows linked idea bubbles and their relationships.</sub>
    </td>
  </tr>
</table>

---

## Setup

**Requirements:** Java 17+ · JavaFX SDK · `json-20231013.jar` (included in `lib/`)

**1. Clone the repo**
```bash
git clone https://github.com/aminabk99/Java_BrainstormBubble
cd Java_BrainstormBubble
```

**2. Start the server**
```bash
javac -cp "lib/json-20231013.jar" src/*.java
java -cp "src:lib/json-20231013.jar" BrainstormServerGUI
```

**3. Start one or more clients** (each in a new terminal)
```bash
java -cp "src:lib/json-20231013.jar" BrainstormClientGUI
```

Click **Connect** in the client window to join the server at `localhost:8080`.

---

## Project Structure
```
Java_BrainstormBubble/
├── src/
│   ├── BrainstormServer.java       # TCP server — accepts clients, broadcasts updates
│   ├── BrainstormServerGUI.java    # Server-side live canvas view (read-only)
│   ├── BrainstormClientGUI.java    # Client canvas — create, drag, connect bubbles
│   ├── ClientHandler.java          # Per-client thread — parses and routes messages
│   ├── MessageHandler.java         # Client-side message processor and outbox
│   ├── NetworkClient.java          # TCP socket connection and listener thread
│   ├── CanvasState.java            # Server's shared state — all bubbles and connections
│   ├── Bubble.java                 # Bubble data model with JSON serialization
│   ├── Connection.java             # Connection data model with JSON serialization
│   └── json-20231013.jar           # JSON library (org.json)
└── lib/
└── json_20231013.xml           # Library config

```
---

## Hardest Part
**Keeping all clients in sync without conflicts** — when two users drag bubbles simultaneously, both send update messages to the server at nearly the same time. Ensuring the server processed them in order and broadcast the correct final state to everyone required careful use of `CopyOnWriteArrayList` and synchronized canvas state methods.

## Most Interesting
**The initial state sync on join** — when a new client connects mid-session, the server immediately sends the full current canvas as an `initial_state` message so the new client sees exactly what everyone else sees. Building that catch-up mechanism from scratch made the multiplayer feel seamless.

---

## Future Improvements
- Inline connection drawing between any two bubbles
- User identity labels on each bubble showing who created it
- Persistent canvas saved to a database between sessions
- WebSocket support for browser-based clients

---

<div align="center">
  <sub>Built by <a href="https://github.com/aminabk99">Amina Bilal</a> · <a href="https://linkedin.com/in/amina-bilal-926340382">LinkedIn</a></sub>
</div>
