# Darvas Box Strategy - High-Level Trading Method

This diagram represents the high-level trading methodology of the Darvas Box strategy, following Nicolas Darvas' original approach for identifying and trading growth stocks.

## High-Level Trading Flow

```mermaid
graph TD
    subgraph Phase 1: Candidate Filtering
        A[Start] --> B{Stock in 'Future Industry'?};
        B -- No --> Z[Discard];
        B -- Yes --> C{Growing Earnings?};
        C -- No --> Z;
    end

    subgraph Phase 2: Technical Monitoring
        C -- Yes --> D[Monitor and Define 'Box'];
        D --> E{Box Top at 52-Week High?};
        E -- No --> D;
    end

    subgraph Phase 3: Entry Trigger
        E -- Yes --> F{Trigger: Price > Box Top AND Significant Volume?};
        F -- No --> D;
        F -- Yes --> G[ACTION: Execute Buy Order];
        G --> H[ACTION: Set Initial Stop-Loss Below Box Bottom];
    end

    subgraph "Phase 4 & 5: Management and Exit"
        H --> I{Open Position: Monitor Price};
        I --> J{Price Hits Stop-Loss?};
        J -- Yes --> K[ACTION: Sell Immediately];
        K --> L[End];
        J -- No --> M{New Higher Box Established?};
        M -- No --> I;
        M -- Yes --> N[ACTION: Raise Stop-Loss to New Box Bottom];
        N --> O(Optional: Consider Additional Purchase);
        O --> I;
    end

    %% Styling for Clarity
    style A fill:#2ecc71,stroke:#fff,stroke-width:2px,color:#fff
    style L fill:#e74c3c,stroke:#fff,stroke-width:2px,color:#fff
    style G fill:#3498db,stroke:#fff,stroke-width:2px,color:#fff
    style K fill:#3498db,stroke:#fff,stroke-width:2px,color:#fff
    style N fill:#f1c40f,stroke:#333,stroke-width:2px,color:#333
```

## Trading Method Overview

### Phase 1: Candidate Filtering
The Darvas method begins with fundamental screening to identify promising growth stocks:

- **Future Industries**: Focus on emerging sectors with high growth potential
- **Growing Earnings**: Companies showing consistent earnings growth
- **Market Leadership**: Stocks that demonstrate strong relative performance

### Phase 2: Technical Monitoring
Once fundamentally sound candidates are identified:

- **Box Formation**: Monitor price action to identify consolidation patterns (boxes)
- **52-Week High Proximity**: Boxes should form near or at 52-week highs
- **Volume Analysis**: Track volume patterns during consolidation

### Phase 3: Entry Trigger
Entry occurs when specific technical conditions align:

- **Breakout Confirmation**: Price breaks above the box top (resistance)
- **Volume Confirmation**: Breakout accompanied by significantly higher volume
- **Buy Stop Order**: Mechanical entry system to capture momentum

### Phase 4: Risk Management
Immediate risk control upon entry:

- **Initial Stop-Loss**: Placed below the box bottom (support level)
- **Mechanical Exit**: No emotions - exit immediately if stop is hit
- **Position Sizing**: Risk management through proper position sizing

### Phase 5: Profit Management
Trailing stop system for profit maximization:

- **Box Progression**: Monitor for new boxes forming at higher levels
- **Trailing Stops**: Raise stop-loss to the bottom of each new higher box
- **Pyramiding**: Optional additional purchases on new box breakouts
- **Trend Following**: Stay with the trend as long as boxes continue forming higher

## Key Principles

1. **Systematic Approach**: Removes emotion from trading decisions
2. **Trend Following**: Rides strong uptrends in leading stocks
3. **Risk Control**: Strict stop-loss discipline limits downside
4. **Profit Maximization**: Trailing stops capture extended moves
5. **Quality Focus**: Only trades the strongest stocks in the strongest sectors

## Color Legend

- **Green**: Start point - Beginning of analysis
- **Red**: End point - Position closed
- **Blue**: Action points - Buy/Sell execution
- **Yellow**: Management actions - Stop-loss adjustments
- **White**: Decision points - Conditional logic