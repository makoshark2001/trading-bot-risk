# trading-bot-risk - Development Guide

**Repository**: https://github.com/makoshark2001/trading-bot-risk  
**Port**: 3003  
**Priority**: 4 (Depends on trading-bot-core + trading-bot-ml)

## 🎯 Service Purpose

Comprehensive risk management and portfolio protection service providing real-time risk assessment, position sizing optimization, and advanced risk analytics. Integrates with all other services to ensure safe trading operations.

## 💬 Chat Instructions for Claude

```
I'm building the risk management service that provides position sizing, portfolio risk assessment, and risk monitoring. This service integrates with core, ML, and other services to ensure safe trading operations.

Key requirements:
- Real-time risk assessment with portfolio-level and position-level metrics
- Position sizing optimization (Kelly Criterion, volatility-based, correlation-adjusted)
- Advanced risk analytics (VaR, CVaR, correlation analysis, stress testing)
- Integration with core service (port 3000) and ML service (port 3001)
- RESTful API on port 3003
- Automated risk controls and alerting system

The core and ML services are already running. I need to build comprehensive risk management capabilities.
```

## 📋 Implementation To-Do List

### ✅ Phase 4A: Project Setup & Service Integration

- [ ] **Project Infrastructure**
  - [ ] Initialize Node.js project: `npm init -y`
  - [ ] Install dependencies:
    ```bash
    npm install express axios lodash winston cors dotenv mathjs
    npm install --save-dev jest nodemon
    ```
  - [ ] Create folder structure:
    ```
    src/
    ├── server/
    ├── risk/
    ├── calculations/
    ├── monitoring/
    ├── services/
    ├── routes/
    └── utils/
    config/
    logs/
    tests/
    ```

- [ ] **Service Integration**
  - [ ] File: `src/services/ServiceIntegration.js` - Core + ML service clients
  - [ ] Health monitoring for all dependent services
  - [ ] Fallback mechanisms for service outages
  - [ ] Test connectivity with core (3000) and ML (3001)

### ✅ Phase 4B: Risk Calculation Engine

- [ ] **Core Risk Metrics**
  - [ ] File: `src/calculations/VaRCalculator.js` - Value at Risk calculations
    - Historical simulation VaR
    - Parametric VaR (normal distribution)
    - Monte Carlo VaR
    - Multiple confidence levels (95%, 99%)
    - Multiple time horizons (1d, 7d, 30d)
  
  - [ ] File: `src/calculations/CorrelationAnalyzer.js` - Portfolio correlation analysis
    - Correlation matrix calculation
    - Diversification ratio
    - Concentration risk metrics
    - Dynamic correlation tracking
  
  - [ ] File: `src/calculations/VolatilityCalculator.js` - Volatility metrics
    - Historical volatility
    - EWMA volatility
    - Volatility regimes
    - Volatility forecasting

- [ ] **Position Sizing Engine**
  - [ ] File: `src/risk/PositionSizer.js` - Multiple sizing methodologies
    - **Kelly Criterion**: Optimal growth sizing based on win rate and risk-reward
    - **Volatility-based**: Size based on target portfolio volatility
    - **Risk Parity**: Equal risk contribution sizing
    - **Fixed Fractional**: Fixed percentage of capital
    - **Correlation-adjusted**: Sizing adjusted for portfolio correlations

### ✅ Phase 4C: Risk Assessment System

- [ ] **Portfolio Risk Assessment**
  - [ ] File: `src/risk/PortfolioRiskAssessor.js` - Complete portfolio analysis
    - Portfolio VaR and Expected Shortfall
    - Risk decomposition by position
    - Concentration risk analysis
    - Stress testing scenarios
    - Risk budgeting and allocation

- [ ] **Position Risk Analysis**
  - [ ] File: `src/risk/PositionRiskAnalyzer.js` - Individual position risk
    - Position-level VaR
    - Beta and correlation analysis
    - Liquidity risk assessment
    - Time to liquidation estimates
    - Risk contribution to portfolio

### ✅ Phase 4D: Risk Monitoring & Alerts

- [ ] **Real-time Risk Monitor**
  - [ ] File: `src/monitoring/RiskMonitor.js` - Continuous risk monitoring
    - Real-time portfolio risk updates
    - Risk limit breach detection
    - Automated alert generation
    - Risk threshold monitoring
    - Escalation procedures

- [ ] **Alert Management System**
  - [ ] File: `src/monitoring/AlertManager.js` - Alert handling
    - Alert classification (LOW, MEDIUM, HIGH, CRITICAL)
    - Alert acknowledgment system
    - Auto-escalation rules
    - Alert history and analytics
    - Notification integration

### ✅ Phase 4E: Advanced Risk Analytics

- [ ] **Stress Testing Engine**
  - [ ] File: `src/risk/StressTestEngine.js` - Scenario analysis
    - Predefined scenarios (market crash, crypto winter, flash crash)
    - Custom scenario testing
    - Historical scenario replay
    - Monte Carlo stress testing
    - Tail risk analysis

- [ ] **Risk API Implementation**
  - [ ] File: `src/routes/` - Complete API endpoints
    - `health.js` - Service health and connectivity
    - `portfolio.js` - Portfolio risk assessment
    - `positions.js` - Individual position risk
    - `positionSize.js` - Position sizing recommendations
    - `correlations.js` - Correlation analysis
    - `stressTest.js` - Stress testing
    - `alerts.js` - Alert management

## 📊 Key API Endpoints to Implement

```javascript
// Service health check
GET /api/health
Response: {
  status: "healthy",
  service: "trading-bot-risk",
  services: {
    core: { status: "healthy", lastCheck: 1704067195000 },
    ml: { status: "healthy", lastCheck: 1704067190000 }
  },
  riskEngine: {
    status: "operational",
    calculations: { completed: 1542, failed: 3 }
  }
}

// Portfolio risk assessment
GET /api/risk/portfolio
Response: {
  portfolio: {
    totalValue: 50000.00,
    totalExposure: 45000.00,
    utilizationRate: 0.90
  },
  riskMetrics: {
    portfolioVaR: {
      daily_95: -850.50,
      daily_99: -1205.75,
      weekly_95: -1890.25
    },
    expectedShortfall: {
      daily_95: -1125.30,
      daily_99: -1654.20
    },
    volatility: {
      daily: 0.0234,
      annualized: 0.3720
    },
    correlations: {
      averageCorrelation: 0.65,
      maxCorrelation: 0.89,
      diversificationRatio: 0.78
    }
  },
  riskScores: {
    overall: "MEDIUM",
    concentration: "HIGH",
    correlation: "MEDIUM",
    volatility: "LOW"
  }
}

// Position sizing recommendation
POST /api/risk/position-size
Body: {
  pair: "RVN_USDT",
  signal: "BUY",
  confidence: 0.78,
  entryPrice: 0.5640,
  stopLoss: 0.5358,
  takeProfit: 0.6204,
  method: "kelly"
}
Response: {
  pair: "RVN_USDT",
  recommendations: {
    kelly: {
      positionSize: 2450.00,
      riskAmount: 245.00,
      riskPercent: 0.49,
      explanation: "Kelly criterion optimal sizing"
    },
    volatility: {
      positionSize: 2100.00,
      riskAmount: 210.00,
      explanation: "Volatility-adjusted sizing"
    },
    recommended: {
      method: "kelly",
      positionSize: 2450.00,
      maxRisk: 245.00,
      reasoning: "Kelly selected due to high confidence"
    }
  }
}

// Correlation analysis
GET /api/risk/correlations
Response: {
  correlationMatrix: {
    "XMR_USDT": { "RVN_USDT": 0.65, "BEL_USDT": 0.72 },
    "RVN_USDT": { "BEL_USDT": 0.45 }
  },
  diversificationMetrics: {
    effectiveAssets: 2.34,
    diversificationRatio: 0.78,
    concentrationIndex: 0.42
  },
  recommendations: [
    "Portfolio shows high correlation during stress periods",
    "Consider adding non-crypto assets for diversification"
  ]
}

// Stress testing
POST /api/risk/stress-test
Body: {
  scenarios: ["market_crash", "crypto_winter", "flash_crash"],
  confidenceLevel: 0.95
}
Response: {
  stressTestResults: {
    market_crash: {
      scenario: "2008-style market crash (-50%)",
      portfolioLoss: -22500.00,
      portfolioLossPercent: -45.0,
      survivability: "HIGH"
    },
    crypto_winter: {
      scenario: "Extended crypto bear market (-80%)",
      portfolioLoss: -36000.00,
      portfolioLossPercent: -72.0,
      survivability: "MEDIUM"
    }
  },
  recommendations: [
    "Portfolio shows resilience to individual shocks",
    "Consider hedging for crypto winter scenario"
  ]
}

// Risk alerts
GET /api/risk/alerts
Response: {
  activeAlerts: [
    {
      id: "ALERT_001",
      level: "HIGH",
      type: "POSITION_SIZE",
      pair: "XMR_USDT",
      message: "Position exceeds 30% portfolio limit",
      currentValue: 0.35,
      threshold: 0.30,
      recommendation: "Reduce position by $2,500",
      acknowledged: false
    }
  ],
  summary: { total: 3, high: 1, medium: 1, low: 1 }
}
```

## 🧮 Risk Calculation Implementations

### 1. **Value at Risk (VaR)**
```javascript
class VaRCalculator {
  // Historical simulation VaR
  calculateHistoricalVaR(returns, portfolioValue, confidenceLevel = 0.95) {
    const sortedReturns = returns.sort((a, b) => a - b);
    const varIndex = Math.floor((1 - confidenceLevel) * sortedReturns.length);
    const varReturn = sortedReturns[varIndex];
    
    return {
      var: portfolioValue * varReturn,
      percentile: confidenceLevel * 100,
      worstReturn: varReturn
    };
  }
  
  // Monte Carlo VaR
  async calculateMonteCarloVaR(portfolio, simulations = 10000) {
    const simulatedReturns = [];
    
    for (let i = 0; i < simulations; i++) {
      const portfolioReturn = await this.simulatePortfolioReturn(portfolio);
      simulatedReturns.push(portfolioReturn);
    }
    
    return this.calculateHistoricalVaR(simulatedReturns, portfolio.totalValue);
  }
}
```

### 2. **Position Sizing Algorithms**
```javascript
class PositionSizer {
  // Kelly Criterion
  calculateKellySize(winRate, avgWin, avgLoss, totalCapital) {
    const b = Math.abs(avgWin / avgLoss);
    const p = winRate;
    const q = 1 - winRate;
    
    const kellyPercent = (b * p - q) / b;
    const safeKelly = Math.max(0, Math.min(kellyPercent * 0.25, 0.20));
    
    return {
      kellyPercent: kellyPercent,
      safeKellyPercent: safeKelly,
      positionSize: totalCapital * safeKelly
    };
  }
  
  // Volatility-based sizing
  calculateVolatilitySize(targetVolatility, assetVolatility, totalCapital) {
    const volatilityRatio = targetVolatility / assetVolatility;
    const positionSize = totalCapital * volatilityRatio;
    
    return {
      positionSize: positionSize,
      volatilityRatio: volatilityRatio,
      targetVol: targetVolatility,
      assetVol: assetVolatility
    };
  }
}
```

### 3. **Correlation Analysis**
```javascript
class CorrelationAnalyzer {
  calculateCorrelationMatrix(priceData) {
    const pairs = Object.keys(priceData);
    const matrix = {};
    
    pairs.forEach(pair1 => {
      matrix[pair1] = {};
      pairs.forEach(pair2 => {
        if (pair1 !== pair2) {
          matrix[pair1][pair2] = this.calculateCorrelation(
            priceData[pair1], 
            priceData[pair2]
          );
        }
      });
    });
    
    return matrix;
  }
  
  calculateDiversificationRatio(weights, correlationMatrix, volatilities) {
    const weightedAvgVol = weights.reduce((sum, weight, i) => 
      sum + weight * volatilities[i], 0);
    
    const portfolioVolatility = this.calculatePortfolioVolatility(
      weights, correlationMatrix, volatilities
    );
    
    return weightedAvgVol / portfolioVolatility;
  }
}
```

## ⚙️ Configuration Requirements

### Environment Variables (.env)
```bash
# Risk Service Configuration
PORT=3003
NODE_ENV=development

# Service URLs
CORE_SERVICE_URL=http://localhost:3000
ML_SERVICE_URL=http://localhost:3001

# Risk Management Parameters
MAX_POSITION_SIZE=0.30
MAX_PORTFOLIO_VAR=0.02
MAX_DAILY_DRAWDOWN=0.05
MAX_CORRELATION=0.70
VOLATILITY_SPIKE_THRESHOLD=2.0

# Position Sizing Defaults
DEFAULT_KELLY_FRACTION=0.25
DEFAULT_VOLATILITY_TARGET=0.15
DEFAULT_RISK_PER_TRADE=0.02

# Monitoring Configuration
RISK_CHECK_INTERVAL=30000
ALERT_RETENTION_HOURS=168
```

### Risk Configuration (config/risk-limits.json)
```json
{
  "portfolio": {
    "maxPositionSize": 0.30,
    "maxDailyVaR": 0.02,
    "maxDrawdown": 0.15,
    "minCashReserve": 0.10
  },
  "positions": {
    "maxIndividualRisk": 0.05,
    "maxCorrelatedRisk": 0.40,
    "stopLossLimits": { "min": 0.02, "max": 0.15 }
  },
  "correlations": {
    "maxPairCorrelation": 0.70,
    "maxAverageCorrelation": 0.60,
    "diversificationThreshold": 0.75
  },
  "alerts": {
    "escalationRules": {
      "positionSize": { "threshold": 3, "timeWindow": 3600000 },
      "correlation": { "threshold": 5, "timeWindow": 7200000 }
    }
  }
}
```

## 🧪 Testing & Validation

```bash
# Test commands to implement
npm run test:calculations  # Risk calculation tests
npm run test:position-size # Position sizing tests
npm run test:monitoring    # Risk monitoring tests
npm run test:integration   # Service integration tests
npm run test:all          # Comprehensive test suite

# Health verification
curl http://localhost:3003/api/health
curl http://localhost:3003/api/risk/portfolio

# Test position sizing
curl -X POST http://localhost:3003/api/risk/position-size \
  -H "Content-Type: application/json" \
  -d '{"pair": "RVN", "signal": "BUY", "confidence": 0.75}'

# Test stress testing
curl -X POST http://localhost:3003/api/risk/stress-test \
  -H "Content-Type: application/json" \
  -d '{"scenarios": ["market_crash", "crypto_winter"]}'
```

## 📊 Performance Benchmarks

- **Risk Calculation Time**: <100ms for portfolio risk assessment
- **Position Sizing**: <50ms for all sizing methods
- **Correlation Analysis**: <200ms for 6x6 correlation matrix
- **VaR Calculation**: <150ms for 1000-day historical simulation
- **Stress Testing**: <5 seconds for comprehensive scenario analysis
- **Memory Usage**: <200MB under normal operation

## 🔗 Integration Points

**Consumes from:**
- trading-bot-core (Port 3000) - Market data and technical analysis
- trading-bot-ml (Port 3001) - ML predictions for risk-adjusted features

**Provides to:**
- trading-bot-execution (Port 3004) - Position sizing and pre-trade risk checks
- trading-bot-backtest (Port 3002) - Risk validation for strategy testing
- trading-bot-dashboard (Port 3005) - Risk metrics for visualization

## ✅ Success Criteria

**Phase 4A Complete When:**
- Service connects successfully to core and ML services
- Health monitoring shows all dependencies operational

**Phase 4B Complete When:**
- VaR calculations produce accurate risk estimates
- Position sizing algorithms provide optimal recommendations
- Correlation analysis identifies portfolio risks

**Phase 4C Complete When:**
- Portfolio risk assessment provides comprehensive metrics
- Position-level analysis identifies individual risks
- Risk scoring accurately classifies portfolio health

**Phase 4D Complete When:**
- Real-time monitoring detects risk limit breaches
- Alert system generates and manages notifications
- Risk thresholds trigger appropriate responses

**Phase 4E Complete When:**
- Stress testing validates portfolio resilience
- All API endpoints provide accurate risk data
- Integration with other services works reliably

## 🚨 Common Issues & Solutions

### 1. **Risk Calculations Failing**
```bash
# Check data from core service
curl http://localhost:3000/api/data | jq '.pairs'

# Verify sufficient historical data (need 60+ points)
curl http://localhost:3000/api/pair/RVN | jq '.history.closes | length'

# Check for invalid calculations
grep -E "(NaN|undefined)" logs/risk-calculations.log
```

### 2. **Service Integration Problems**
```bash
# Test connectivity
curl http://localhost:3000/api/health  # Core
curl http://localhost:3001/api/health  # ML

# Check integration logs
tail -f logs/risk-error.log | grep -E "(core|ml)"
```

---

*Save this file as `DEVELOPMENT_GUIDE.md` in the trading-bot-risk repository root*