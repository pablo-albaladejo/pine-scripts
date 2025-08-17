# Pine Script Programming Guide

## Table of Contents
- [Overview](#overview)
- [Script Structure](#script-structure)
- [Data Types](#data-types)
- [Variables and Constants](#variables-and-constants)
- [Functions](#functions)
- [Built-in Variables](#built-in-variables)
- [Plotting](#plotting)
- [Strategies vs Indicators](#strategies-vs-indicators)
- [Input Parameters](#input-parameters)
- [Control Structures](#control-structures)
- [Arrays and Matrices](#arrays-and-matrices)
- [Security Functions](#security-functions)
- [Performance Optimization](#performance-optimization)
- [Best Practices](#best-practices)
- [Common Patterns](#common-patterns)
- [Debugging Tips](#debugging-tips)

## Overview

Pine Script is TradingView's domain-specific language for creating custom technical indicators and trading strategies. It's designed to be simple yet powerful, with built-in functions for financial calculations.

### Version Declaration
Always start scripts with version declaration (use v6 for new scripts):
```pine
//@version=6
```

## Script Structure

### Basic Structure
```pine
//@version=6
indicator("My Indicator", shorttitle="MI", overlay=true)

// Input parameters
length = input.int(20, "Length", minval=1)

// Calculations
ma = ta.sma(close, length)

// Plotting
plot(ma, color=color.blue, linewidth=2)
```

### Script Types
- `indicator()` - Technical indicators
- `strategy()` - Trading strategies with backtesting
- `library()` - Reusable functions

## Data Types

### Basic Types
- **int**: Integer numbers (`1`, `100`, `-50`)
- **float**: Floating-point numbers (`1.5`, `3.14159`, `-0.5`)
- **bool**: Boolean values (`true`, `false`)
- **color**: Color values (`color.red`, `#FF0000`)
- **string**: Text strings (`"Hello"`, "AAPL")

### Series Types
All basic types can be series (time-series values that change on each bar):
- `series<int>`
- `series<float>`
- `series<bool>`
- `series<color>`
- `series<string>`

### Special Types
- **line**: Drawing objects for lines
- **label**: Drawing objects for labels
- **box**: Drawing objects for boxes
- **table**: Drawing objects for tables

## Variables and Constants

### Variable Declaration
```pine
// Explicit type
var int counter = 0
var float highestPrice = 0.0

// Implicit type (inferred)
length = 20
isUpTrend = close > open

// Series variables (recalculate on each bar)
currentMA = ta.sma(close, 20)
```

### Variable Keywords
- `var`: Variable persists between bars, initialized only on first bar
- `varip`: Variable persists between bars and ticks (intrabar)
- No keyword: Variable recalculates on each bar

### Constants
```pine
// Constants (compile-time)
GOLDEN_RATIO = 1.618
MAX_BARS = 5000
```

## Functions

### Built-in Functions
Pine Script provides extensive built-in functions:

#### Technical Analysis (`ta.*`)
```pine
ta.sma(source, length)           // Simple Moving Average
ta.ema(source, length)           // Exponential Moving Average
ta.rsi(source, length)           // Relative Strength Index
ta.macd(source, fast, slow, signal) // MACD
ta.highest(source, length)       // Highest value
ta.lowest(source, length)        // Lowest value
ta.crossover(series1, series2)   // Cross over detection
ta.crossunder(series1, series2)  // Cross under detection
```

#### Math Functions (`math.*`)
```pine
math.abs(value)                  // Absolute value
math.max(value1, value2)         // Maximum
math.min(value1, value2)         // Minimum
math.round(value, precision)     // Round to precision
math.floor(value)                // Floor
math.ceil(value)                 // Ceiling
```

### Custom Functions
```pine
// Function definition
myFunction(param1, param2) =>
    result = param1 + param2
    result

// Usage
value = myFunction(10, 20)
```

### Function Best Practices
- Use descriptive names
- Keep functions small and focused
- Return meaningful values
- Document complex functions
- **IMPORTANT**: Function parameters cannot be separated by line breaks. All parameters must be on the same line as the function call.

## Built-in Variables

### Price Data
- `open`: Opening price
- `high`: High price
- `low`: Low price
- `close`: Closing price
- `volume`: Volume
- `hl2`: (high + low) / 2
- `hlc3`: (high + low + close) / 3
- `ohlc4`: (open + high + low + close) / 4

### Bar Information
- `bar_index`: Current bar index
- `time`: Current bar timestamp
- `timeframe.period`: Current timeframe
- `syminfo.ticker`: Current symbol ticker

## Plotting

### Basic Plotting
```pine
// Simple plot
plot(close, color=color.blue, title="Close Price")

// Conditional coloring
plotColor = close > open ? color.green : color.red
plot(close, color=plotColor)

// Multiple plots
plot(ta.sma(close, 20), color=color.blue, title="SMA 20")
plot(ta.ema(close, 20), color=color.red, title="EMA 20")
```

### Plot Styles
```pine
plot(series, color=color.blue, linewidth=2, style=plot.style_line)
// Styles: plot.style_line, plot.style_stepline, plot.style_histogram, 
//         plot.style_cross, plot.style_area, plot.style_columns
```

### Conditional Plotting
```pine
// Plot only when condition is true
plotshape(ta.crossover(close, ta.sma(close, 20)), 
          style=shape.triangleup, 
          color=color.green, 
          size=size.small)
```

## Strategies vs Indicators

### Indicators
```pine
//@version=5
indicator("My Indicator", overlay=true)

// Indicator logic here
sma20 = ta.sma(close, 20)
plot(sma20)
```

### Strategies
```pine
//@version=5
strategy("My Strategy", overlay=true, default_qty_type=strategy.percent_of_equity, default_qty_value=100)

// Strategy logic
longCondition = ta.crossover(ta.sma(close, 14), ta.sma(close, 28))
shortCondition = ta.crossunder(ta.sma(close, 14), ta.sma(close, 28))

if longCondition
    strategy.entry("Long", strategy.long)

if shortCondition
    strategy.entry("Short", strategy.short)
```

## Input Parameters

### Input Types
```pine
// Integer input
length = input.int(20, "Period", minval=1, maxval=200)

// Float input
multiplier = input.float(2.0, "Multiplier", minval=0.1, maxval=5.0, step=0.1)

// Boolean input
showSignals = input.bool(true, "Show Signals")

// String input
source = input.source(close, "Source")

// Color input
lineColor = input.color(color.blue, "Line Color")

// Timeframe input
higherTF = input.timeframe("1D", "Higher Timeframe")
```

### Input Groups
```pine
// Organize inputs into groups
var g1 = "Moving Averages"
fastLength = input.int(12, "Fast MA Length", group=g1)
slowLength = input.int(26, "Slow MA Length", group=g1)

var g2 = "Display Options"
showMA = input.bool(true, "Show Moving Average", group=g2)
lineWidth = input.int(2, "Line Width", minval=1, maxval=5, group=g2)
```

## Control Structures

### Conditional Statements
```pine
// if-else
if close > open
    // Bullish bar logic
    color.green
else
    // Bearish bar logic
    color.red

// Ternary operator
barColor = close > open ? color.green : color.red

// Switch statement
timeframeColor = switch timeframe.period
    "1"  => color.red
    "5"  => color.orange
    "15" => color.yellow
    "1H" => color.green
    => color.gray
```

### Loops
```pine
// for loop
sum = 0.0
for i = 1 to 10
    sum := sum + i

// while loop
var counter = 0
while counter < 10
    counter := counter + 1
```

## Arrays and Matrices

### Arrays
```pine
// Array declaration
var prices = array.new<float>(0)

// Array operations
array.push(prices, close)
lastPrice = array.get(prices, array.size(prices) - 1)
array.pop(prices)

// Array with fixed size
var recentPrices = array.new<float>(10, 0.0)
array.set(recentPrices, 0, close)
```

### Matrices
```pine
// Matrix operations (advanced use cases)
var matrix<float> correlation = matrix.new<float>(3, 3, 0.0)
matrix.set(correlation, 0, 0, 1.0)
```

## Security Functions

### Multi-timeframe Analysis
```pine
// Get data from higher timeframe
htfClose = request.security(syminfo.tickerid, "1D", close)
htfMA = request.security(syminfo.tickerid, "1D", ta.sma(close, 20))

// Avoid repainting with lookahead
htfCloseNoRepaint = request.security(syminfo.tickerid, "1D", close[1], lookahead=barmerge.lookahead_off)
```

## Performance Optimization

### Best Practices for Performance

1. **Minimize Security Calls**
```pine
// Bad: Multiple security calls
daily_open = request.security(syminfo.tickerid, "1D", open)
daily_high = request.security(syminfo.tickerid, "1D", high)
daily_low = request.security(syminfo.tickerid, "1D", low)

// Good: Single security call with tuple
[daily_open, daily_high, daily_low] = request.security(syminfo.tickerid, "1D", [open, high, low])
```

2. **Use Efficient Data Structures**
```pine
// Use arrays for collections of data
var pivotPoints = array.new<float>(0)

// Limit array size to prevent memory issues
if array.size(pivotPoints) > 100
    array.shift(pivotPoints)
```

3. **Avoid Unnecessary Calculations**
```pine
// Calculate only when needed
longMA = na(longMA[1]) ? ta.sma(close, 50) : 
         math.abs(close - close[1]) > 0.01 ? ta.sma(close, 50) : longMA[1]
```

## Best Practices

### Code Organization
1. **Use meaningful variable names**
```pine
// Bad
l = 20
v = ta.sma(close, l)

// Good
smaLength = 20
smaValue = ta.sma(close, smaLength)
```

2. **Group related code**
```pine
// Input parameters section
fastLength = input.int(12, "Fast Length")
slowLength = input.int(26, "Slow Length")

// Calculations section
fastMA = ta.ema(close, fastLength)
slowMA = ta.ema(close, slowLength)

// Signal generation section
bullishSignal = ta.crossover(fastMA, slowMA)
bearishSignal = ta.crossunder(fastMA, slowMA)

// Plotting section
plot(fastMA, color=color.blue, title="Fast EMA")
plot(slowMA, color=color.red, title="Slow EMA")
```

3. **Use comments for complex logic**
```pine
// Calculate pivot points using traditional method
// High/Low must be confirmed by subsequent bars
isPivotHigh = high[1] > high[2] and high[1] > high[0] and volume[1] > volume[2]
```

4. **Comment Formatting Rules**
```pine
// Use single-line comments with // for all comments
// AVOID multi-line comments with /* */ as they can cause issues

// =============================================================================
//                              SECTION HEADERS
// =============================================================================

// -- Subsection headers use double dashes
subValue = input.int(20, "Sub Value")

// Regular comments for code explanation
calculatedValue = ta.sma(close, subValue)
```

### Error Handling
```pine
// Check for valid data
if not na(close) and not na(volume)
    // Perform calculations
    result = close * volume
else
    result := na
```

### Memory Management
```pine
// Limit historical data usage
if bar_index > 5000
    // Only process recent bars
    // Your calculations here
```

## Common Patterns

### Signal Detection
```pine
// Golden Cross pattern
fastMA = ta.sma(close, 50)
slowMA = ta.sma(close, 200)
goldenCross = ta.crossover(fastMA, slowMA)

// Breakout pattern
resistance = ta.highest(high, 20)
breakout = close > resistance[1] and volume > ta.sma(volume, 10) * 1.5
```

### Dynamic Support/Resistance
```pine
// Dynamic support using pivot lows
var line supportLine = na
if ta.pivotlow(low, 5, 5)
    line.delete(supportLine)
    supportLine := line.new(bar_index - 5, low[5], bar_index, low[5], 
                           color=color.green, width=2)
```

### Multi-timeframe Confirmation
```pine
// Current timeframe signal
currentSignal = ta.crossover(ta.rsi(close, 14), 30)

// Higher timeframe confirmation
htfTrend = request.security(syminfo.tickerid, "4H", close > ta.sma(close, 20))

// Combined signal
confirmedSignal = currentSignal and htfTrend
```

## Debugging Tips

### Using Labels for Debug Output
```pine
// Debug values with labels
if barstate.islast
    label.new(bar_index, high, 
              text="RSI: " + str.tostring(ta.rsi(close, 14)), 
              style=label.style_label_down,
              color=color.yellow)
```

### Plot Debug Values
```pine
// Plot values in separate pane for debugging
plot(ta.rsi(close, 14), title="RSI Debug")
hline(70, "Overbought")
hline(30, "Oversold")
```

### Conditional Alerts
```pine
// Debug with alerts
if ta.crossover(close, ta.sma(close, 20))
    alert("MA Crossover at: " + str.tostring(close))
```

### Table for Multiple Debug Values
```pine
// Debug table
var debugTable = table.new(position.top_right, 2, 5, bgcolor=color.white)
if barstate.islast
    table.cell(debugTable, 0, 0, "RSI", text_color=color.black)
    table.cell(debugTable, 1, 0, str.tostring(ta.rsi(close, 14)), text_color=color.black)
    table.cell(debugTable, 0, 1, "MA", text_color=color.black)
    table.cell(debugTable, 1, 1, str.tostring(ta.sma(close, 20)), text_color=color.black)
```

## Advanced Topics

### Custom Drawing Objects
```pine
// Custom function to draw trend lines
drawTrendLine(x1, y1, x2, y2, lineColor) =>
    line.new(x1, y1, x2, y2, color=lineColor, width=2, style=line.style_solid)
```

### Libraries (Pine v5)
```pine
//@version=5
// @description Custom utility functions
library("MyUtils", overlay=true)

// Export function
export myUtility(value) =>
    // Function implementation
    result = value * 2
    result
```

This guide provides a comprehensive foundation for Pine Script development. Remember to always test your scripts thoroughly and follow TradingView's best practices for script publication.