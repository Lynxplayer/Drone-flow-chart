'''mermaid

flowchart TD
    A["Start / Power On" ] --> B[ "Initialize ESP32" 
    <br/> "Set preset intensity threshold" 
    <br/> "Configure Wi‑Fi & email credentials" 
    <br/> "Initialize LCD (I2C 16×2)" 
    <br/> "Set GPIO pins for LEDs" 
    <br/> "Setup ADC for LDR" ]
    
    B --> C[ "Read Potentiometer" 
    <br/> "Set laser driver current" ]
    
    C --> D[ "Turn Laser ON" 
    <br/> "Transmit light through fiber" ]
    
    D --> E[ "Read LDR via ADC" 
    <br/> "Voltage divider measurement" ]
    
    E --> F[ "Convert ADC value to intensity" 
    <br/> "Optional calibration" ]
    
    F --> G{ "Is intensity ≥ preset threshold?" }
    
    G -- Yes (High/Equal) --> H[ "Turn GREEN LED ON" 
    <br/> "Turn RED LED OFF" ]
    
    H --> I[ "Update LCD: LINK OK + value" ]
    
    I --> J[ "Clear alert latch (if set)" ]
    
    J --> K[ "Wait debounce interval (100–200 ms)" ]
    
    K --> E
    
    G -- No (Low/Below) --> L[ "Turn RED LED ON" 
    <br/> "Turn GREEN LED OFF" ]
    
    L --> M[ "Update LCD: ALERT: DROP + value" ]
    
    M --> N{ "Has alert already been sent?" }
    
    N -- No --> O[ "Send Email: Macrobend / Power Drop" 
    <br/> "Include timestamp & readings" ]
    
    O --> P[ "Set alert latch = TRUE" ]
    
    N -- Yes --> Q[ "Skip sending email (prevent spam)" ]
    
    P --> R[ "Optional: log event to NVS/SD" ]
    
    Q --> R
    
    R --> K

