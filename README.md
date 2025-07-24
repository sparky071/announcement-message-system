# MessageQueue

A Roblox client-server system for displaying queued, animated messages with priority support for server announcements.

## 🚀 Features

- Animated UI message display using `TweenService`
- Queues local and remote messages
- Suppresses duplicates (e.g., shows `"[2x]"`, `"[3x]"`)
- Prioritizes remote messages (e.g., global announcements)
- Supports real-time server broadcasts via `MessagingService`

## 🧩 Function

### `handleMessage(message: string, isRemote: boolean)`
**Arguments:**
- `message` (string): The message to display
- `isRemote` (boolean): Whether it came from the server (true = priority)

**Behavior:**
- If the message is already showing, adds a count suffix (e.g., `"[2x]"`)
- Otherwise, queues the message:
  - Remote messages → front of queue
  - Local messages → back of queue
- Displays each message for 5 seconds, with tween-in and tween-out animations

---

## 🖥️ Client Script

### Expects:
- `Message` (`TextLabel`): UI element to display the message
- `SendMessage` (`BindableEvent`): For local message input
- `Message` (`RemoteEvent`): Remote message input (`ReplicatedStorage.Remotes.Message`)

### Behavior:
- Animates message in and out using `TweenService`
- Manages display queue with no overlapping
- Resets after each message and continues queue

---

## 📡 Server Script

### `MessagingService:SubscribeAsync("Announcements")`

**Behavior:**
- Listens for global messages published to `"Announcements"`
- Forwards them to all players using a `RemoteEvent`

### Example:

```lua
local ms = game:GetService("MessagingService")
local messageRemote = -- RemoteEvent reference

ms:SubscribeAsync("Announcements", function(announcement)
  messageRemote:FireAllClients(announcement.Data)
end)
