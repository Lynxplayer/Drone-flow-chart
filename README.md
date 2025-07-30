```mermaid

flowchart TD
  A["Launch App"]
  B["Check permissions (Network/WiFi)"]
  C["Load saved settings (IP / Port / Rate / Deadzone)"]
  D{"ESP32 reachable?"}
  E["Open socket UDP or TCP"]
  F["Show 'Not connected' / Retry"]
  G["UI ready: dual joysticks (Mode 2 mapping)"]
  H{"User presses ARM?"}
  I["Send ARM command"]
  P["Emergency STOP -> DISARM"]
  J["Main loop (20 ms)"]
  K["Read joysticks"]
  L["Apply deadzone and scale"]
  M["Map to {thr, yaw, pitch, roll}"]
  N["Send packet to ESP32"]
  Q{"Disconnected or Timeout?"}
  O["Stop sending, alert, auto reconnect"]

  A --> B
  B --> C
  C --> D
  D -->|Yes| E
  D -->|No| F
  F --> D
  E --> G
  G --> H
  H -->|Yes| I
  H -->|No| G
  I --> J
  J --> K
  K --> L
  L --> M
  M --> N
  N --> Q
  Q -->|No| J
  Q -->|Yes| O
  G --> P
  P --> O
  O --> D

...diagram...
