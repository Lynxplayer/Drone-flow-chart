```mermaid
flowchart TD
  A[Launch App] --> B[Check permissions<br/>(Wi‑Fi/Network)]
  B --> C[Load saved settings<br/>(IP/Port/Rate/Deadzone)]
  C --> D{ESP32 reachable?}
  D -- Yes --> E[Open socket (UDP/TCP)]
  D -- No --> F[Show "Not connected"<br/>Retry button]
  F --> D
  E --> G[UI ready: dual joysticks<br/>Mode‑2 mapping]
  G --> H{User presses ARM?}
  H -- Yes --> I[Send ARM cmd]
  H -- No --> G

  I --> J[Main loop (every 20 ms)]
  J --> K[Read joysticks]
  K --> L[Apply deadzone & scale]
  L --> M[Map Mode‑2 → {thr,yaw,pitch,roll}]
  M --> N[Build packet<br/>{seq,thr,yaw,pitch,roll,aux}]
  N --> O[Send packet over Wi‑Fi]
  O --> P[Receive telemetry?]
  P -- Yes --> Q[Update UI (battery, RSSI,<br/>alt, msgs)]
  P -- No --> R[No update this tick]
  Q --> S[Safety checks<br/>(loss, low batt)]
  R --> S
  S --> T{Disconnected or timeout?}
  T -- Yes --> U[Stop sending, show alert,<br/>Auto‑reconnect]
  T -- No --> J

  G --> V[Emergency STOP button]
  V --> W[Send DISARM/STOP cmd]
  W --> U
