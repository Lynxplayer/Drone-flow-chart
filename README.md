```mermaid
flowchart TD
  A[Launch App]
  B[Check permissions (Network/Wi-Fi)]
  C[Load saved settings (IP / Port / Rate / Deadzone)]
  D{ESP32 reachable?}
  E[Open socket UDP or TCP]
  F["Show 'Not connected' / Retry"]
  G[UI ready: dual joysticks (Mode-2 mapping)]
  H{User presses ARM?}
  I[Send ARM command]
  P[Emergency STOP -> DISARM]
  J[Main loop (20 ms)]
  K[Read joysticks]
  L[Apply deadzone & scale]
  M[Map to {thr, yaw, pitch, roll}]
  N[Send packet to ESP32]
  Q{Disconnected / Timeout?}
  O[Stop sending, alert, auto-reconnect]

  A --> B --> C --> D
  D -- Yes --> E --> G
  D -- No --> F --> D
  G --> H
  H -- Yes --> I --> J
  H -- No --> G
  G --> P --> O --> D
  J --> K --> L --> M --> N --> Q
  Q -- No --> J
  Q -- Yes --> O
