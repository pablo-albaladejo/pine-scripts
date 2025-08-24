# Darvas Box Strategy - Flow Diagram

Este diagrama representa la lógica de estados de la estrategia Darvas Box implementada en `strategies/darvas-box.pine`.

## Diagrama de Flujo

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

## Descripción de Estados

### Estados Principales

1. **IDLE**: Estado inicial, esperando nuevos máximos relativos
2. **SEEKING_CEILING_CONF**: Confirmando el techo de una posible caja Darvas
3. **CEILING_CONFIRMED**: Techo confirmado, buscando confirmación del piso
4. **BOX_FULLY_CONFIRMED**: Caja completamente formada, monitoreando breakouts

### Condiciones de Transición

- **New Relative High**: Precio actual supera el máximo de los últimos N períodos
- **Ceiling Confirmed**: N barras consecutivas sin nuevos máximos
- **Floor Confirmed**: N barras consecutivas sin nuevos mínimos
- **Box Quality Valid**: La caja cumple criterios de ancho mínimo y altura mínima
- **Breakout Detection**: Precio rompe por encima o debajo de los límites de la caja

### Tipos de Breakout

- **CEILING_BREAK**: Precio rompe por encima del techo → Entrada larga
- **FLOOR_BREAK**: Precio rompe por debajo del piso → Caja inválida
- **NO_BREAK**: Precio permanece dentro de la caja → Continuar monitoreando

### Gestión de Riesgo

- **Initial Stop Loss**: Establecido en el piso de la caja de entrada
- **Trailing Stop**: Se actualiza cuando se forman nuevas cajas a niveles superiores
- **Bear Market Exit**: Salida forzada cuando las condiciones de mercado se deterioran

### Filtros de Calidad

- **Market Trend Filter**: Solo opera en mercados alcistas
- **Annual High Filter**: Solo forma cajas cerca de máximos anuales
- **Volume Filter**: Requiere volumen elevado en breakouts
- **Box Quality**: Ancho mínimo y altura mínima de cajas

## Colores del Diagrama

- **Azul claro**: Estado inicial (IDLE)
- **Verde**: Estado de confirmación completa (BOX_FULLY_CONFIRMED)
- **Amarillo**: Ejecución de entrada (Execute Long Entry)
- **Rojo**: Breakouts de piso (Floor Break)
- **Rosa**: Salidas forzadas (Bear Market Exit)