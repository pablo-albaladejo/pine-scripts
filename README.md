# Pine Scripts Project

## Overview
This project contains Pine Script strategies and indicators for TradingView. Pine Script is TradingView's domain-specific language for creating custom technical analysis tools.

## Project Structure
```
pine-scripts/
├── .claude/
│   └── settings.local.json
├── .vscode/
│   └── settings.json
├── docs/
│   └── PINE_SCRIPT_GUIDE.md
├── strategies/
│   ├── darvas-box.pine
│   └── thesis.md
├── .cursorignore
├── .cursorrules
├── .gitignore
├── CLAUDE.md
└── README.md
```

## Development Guide
Before working with Pine Script files, **always reference** the comprehensive guide at `docs/PINE_SCRIPT_GUIDE.md`. This guide contains:

- Complete syntax reference
- Best practices and coding standards
- Performance optimization techniques
- Common patterns and examples
- Debugging tips and techniques

## Getting Started

1. **Read the Guide**: Start with `docs/PINE_SCRIPT_GUIDE.md` to understand Pine Script fundamentals
2. **Study Examples**: Review existing strategies in the `strategies/` folder
3. **Follow Conventions**: Use the established code structure and naming patterns
4. **Test Thoroughly**: Always backtest strategies before live implementation

## Code Standards

### File Structure
All Pine Script files should follow this structure:
```pine
//@version=5
strategy("Strategy Name", overlay=true, ...)

// =============================================================================
//                           CONFIGURATION (INPUTS)
// =============================================================================

// Input parameters with proper grouping

// =============================================================================
//                                CALCULATIONS
// =============================================================================

// Main logic and calculations

// =============================================================================
//                               STRATEGY LOGIC
// =============================================================================

// Entry and exit conditions

// =============================================================================
//                                PLOTTING
// =============================================================================

// Visual elements and plots
```

### Best Practices
- Use Pine Script v5 syntax (`//@version=5`)
- Group related inputs with the `group` parameter
- Implement proper risk management
- Add meaningful comments and documentation
- Follow performance optimization guidelines from the guide

## Tools Integration

### Claude CLI
This project includes context files for Claude CLI:
- `CLAUDE.md` file with project context and guidelines
- Comprehensive Pine Script reference guide
- Development workflow and best practices

### Cursor AI
This project is optimized for Cursor AI development with:
- `.cursorrules` file with Pine Script specific guidelines
- Project structure documentation
- Code standards and best practices

### TradingView
- Upload `.pine` files directly to TradingView Pine Editor
- Test strategies with historical data
- Publish indicators/strategies to TradingView community

## Contributing
When adding new strategies or indicators:
1. Follow the established file structure
2. Reference `docs/PINE_SCRIPT_GUIDE.md` for syntax and best practices
3. Include comprehensive input parameters
4. Add proper documentation and comments
5. Test thoroughly before committing

## Resources
- [PINE_SCRIPT_GUIDE.md](./docs/PINE_SCRIPT_GUIDE.md) - Complete Pine Script reference
- [TradingView Pine Script Documentation](https://www.tradingview.com/pine-script-docs/)
- [Pine Script v5 User Manual](https://www.tradingview.com/pine-script-docs/en/v5/)