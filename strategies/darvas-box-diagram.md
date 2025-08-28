# Darvas Box Strategy - Low-Level Flow Diagram

This diagram represents the state machine logic of the Darvas Box strategy implemented in `strategies/darvas-box.pine`.

## State Flow Diagram

```mermaid
graph TD
    A[IDLE] --> B{New Relative High?}
    B -->|Yes| C[SEEKING_CEILING_CONF]
    B -->|No| A
    
    C --> D{Ceiling Confirmed?}
    D -->|No| E{Higher High?}
    E -->|Yes| F[Update Potential Ceiling]
    E -->|No| G[Increment Confirmation Days]
    F --> C
    G --> D
    
    D -->|Yes| H[CEILING_CONFIRMED]
    
    H --> I{Floor Confirmed?}
    I -->|No| J[Track Running Floor]
    J --> K{Higher High than Ceiling?}
    K -->|Yes| L[Reset to SEEKING_CEILING_CONF]
    K -->|No| I
    
    I -->|Yes| M{Box Quality Valid?}
    M -->|No| N[Reset Floor Count]
    N --> I
    M -->|Yes| O[BOX_FULLY_CONFIRMED]
    
    O --> P[Create Visual Box]
    P --> Q[Set Current Box Variables]
    Q --> R{Higher High than Ceiling?}
    R -->|Yes| S[Reset to SEEKING_CEILING_CONF]
    R -->|No| T[Monitor for Breakouts]
    
    T --> U{Breakout Type?}
    U -->|CEILING_BREAK| V[Execute Long Entry]
    U -->|FLOOR_BREAK| W[Update Box Color to Red]
    U -->|NO_BREAK| X[Extend Box Right Edge]
    
    V --> Y[Set Entry Variables]
    Y --> Z[Set Initial Stop Loss]
    Z --> AA[Track Position]
    
    AA --> BB{Position Active?}
    BB -->|Yes| CC[Calculate Trailing Stop]
    CC --> DD[Execute Stop Loss Order]
    DD --> EE{Bear Market?}
    EE -->|Yes| FF[Close All Positions]
    EE -->|No| BB
    
    BB -->|No| GG{Bull Market & Filters OK?}
    GG -->|Yes| B
    GG -->|No| A
    
    W --> HH[Mark Box as Broken]
    X --> II{Stop Violation?}
    II -->|Yes| JJ[Update Box Color to Yellow]
    II -->|No| X
    
    HH --> GG
    JJ --> GG
    
    L --> C
    S --> C
    
    style A fill:#e1f5fe
    style O fill:#c8e6c9
    style V fill:#ffeb3b
    style W fill:#ffcdd2
    style FF fill:#f8bbd9
```

## State Descriptions

### Main States

1. **IDLE**: Initial state, waiting for new relative highs
2. **SEEKING_CEILING_CONF**: Confirming the ceiling of a potential Darvas box
3. **CEILING_CONFIRMED**: Ceiling confirmed, seeking floor confirmation
4. **BOX_FULLY_CONFIRMED**: Box fully formed, monitoring for breakouts

### Transition Conditions

- **New Relative High**: Current price exceeds the maximum of the last N periods
- **Ceiling Confirmed**: N consecutive bars without new highs
- **Floor Confirmed**: N consecutive bars without new lows
- **Box Quality Valid**: Box meets minimum width and height criteria
- **Breakout Detection**: Price breaks above or below box boundaries

### Breakout Types

- **CEILING_BREAK**: Price breaks above ceiling → Long entry
- **FLOOR_BREAK**: Price breaks below floor → Box invalidated
- **NO_BREAK**: Price remains within box → Continue monitoring

### Risk Management

- **Initial Stop Loss**: Set at the floor of the entry box
- **Trailing Stop**: Updated when new boxes form at higher levels
- **Bear Market Exit**: Forced exit when market conditions deteriorate

### Quality Filters

- **Market Trend Filter**: Only trades in bull markets
- **Annual High Filter**: Only forms boxes near annual highs
- **Volume Filter**: Requires elevated volume on breakouts
- **Box Quality**: Minimum width and height requirements

## Diagram Colors

- **Light Blue**: Initial state (IDLE)
- **Green**: Full confirmation state (BOX_FULLY_CONFIRMED)
- **Yellow**: Entry execution (Execute Long Entry)
- **Red**: Floor breakouts (Floor Break)
- **Pink**: Forced exits (Bear Market Exit)