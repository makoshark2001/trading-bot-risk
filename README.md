// Integration in ML feature pipeline
const riskFeatureEngineer = new RiskFeatureEngineer();

async function generateMLFeatures(pair, marketData) {
  // Generate base technical features
  const baseFeatures = await extractTechnicalFeatures(pair, marketData);
  
  // Enhance with risk features
  const enhancedFeatures = await riskFeatureEngineer.extractRiskFeatures(pair, baseFeatures);
  
  return enhancedFeatures;
}
```

---

## 🧪 Testing & Validation

### Available Test Scripts
```bash
# Test risk service connectivity
npm run test:connectivity

# Test risk calculations
npm run test:calculations

# Test position sizing algorithms
npm run test:position-sizing

# Test stress testing engine
npm run test:stress-testing

# Test risk monitoring and alerts
npm run test:monitoring

# Run comprehensive risk validation
npm run test:all
```

### Risk Calculation Validation
```bash
# Validate VaR calculations
curl -X POST http://localhost:3003/api/risk/validate/var \
  -H "Content-Type: application/json" \
  -d '{
    "returns": [-0.02, 0.01, -0.01, 0.03, -0.04],
    "portfolioValue": 10000,
    "confidenceLevel": 0.95
  }'

# Validate position sizing
curl -X POST http://localhost:3003/api/risk/validate/position-size \
  -H "Content-Type: application/json" \
  -d '{
    "pair": "TEST_PAIR",
    "winRate": 0.6,
    "avgWin": 100,
    "avgLoss": -50,
    "totalCapital": 10000
  }'

# Validate correlation calculations
curl -X POST http://localhost:3003/api/risk/validate/correlation \
  -H "Content-Type: application/json" \
  -d '{
    "series1": [1, 2, 3, 4, 5],
    "series2": [2, 4, 6, 8, 10]
  }'
```

### Performance Benchmarks
- **Risk Calculation Time**: <100ms for portfolio risk assessment
- **Position Sizing**: <50ms for all sizing methods
- **Correlation Analysis**: <200ms for 10x10 correlation matrix
- **VaR Calculation**: <150ms for 1000-day historical simulation
- **Stress Testing**: <5 seconds for comprehensive scenario analysis
- **Memory Usage**: <200MB under normal operation

---

## 🔧 Configuration

### Environment Variables (.env)
```bash
# Risk Service Configuration
PORT=3003
NODE_ENV=development

# Service URLs
CORE_SERVICE_URL=http://localhost:3000
ML_SERVICE_URL=http://localhost:3001
EXECUTION_SERVICE_URL=http://localhost:3004
BACKTEST_SERVICE_URL=http://localhost:3002

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
AUTO_ACKNOWLEDGE_LOW_ALERTS=true

# Calculation Settings
VAR_CONFIDENCE_LEVELS=0.95,0.99
VAR_TIME_HORIZONS=1,7,30
CORRELATION_WINDOW_DAYS=60
STRESS_TEST_SCENARIOS=market_crash,crypto_winter,flash_crash

# Logging
LOG_LEVEL=info
RISK_LOG_FILE=logs/risk.log
```

### Risk Configuration Files

#### config/risk-limits.json
```json
{
  "portfolio": {
    "maxPositionSize": 0.30,
    "maxDailyVaR": 0.02,
    "maxDrawdown": 0.15,
    "minCashReserve": 0.10,
    "maxLeverage": 1.0
  },
  "positions": {
    "maxIndividualRisk": 0.05,
    "maxCorrelatedRisk": 0.40,
    "liquidationBuffer": 0.20,
    "stopLossLimits": {
      "min": 0.02,
      "max": 0.15
    }
  },
  "correlations": {
    "maxPairCorrelation": 0.70,
    "maxAverageCorrelation": 0.60,
    "diversificationThreshold": 0.75,
    "rebalanceTrigger": 0.80
  },
  "volatility": {
    "spikeTresholds": {
      "warning": 1.5,
      "critical": 2.0
    },
    "regimeClassification": {
      "low": 0.02,
      "medium": 0.04,
      "high": 0.06
    }
  },
  "alerts": {
    "escalationRules": {
      "positionSize": {
        "threshold": 3,
        "timeWindow": 3600000
      },
      "correlation": {
        "threshold": 5,
        "timeWindow": 7200000
      }
    },
    "autoAcknowledge": {
      "low": 300000,
      "medium": 900000
    }
  }
}
```

#### config/stress-scenarios.json
```json
{
  "predefined": {
    "market_crash": {
      "name": "Market Crash (2008-style)",
      "description": "50% decline across all risk assets",
      "shocks": {
        "all_crypto": -0.50,
        "volatility_spike": 3.0,
        "correlation_increase": 0.90
      },
      "duration": "6-12 months",
      "probability": "low"
    },
    "crypto_winter": {
      "name": "Crypto Winter", 
      "description": "Extended 80% decline over 12 months",
      "shocks": {
        "all_crypto": -0.80,
        "volume_decline": -0.60,
        "correlation_increase": 0.95
      },
      "duration": "12-24 months",
      "probability": "medium"
    },
    "flash_crash": {
      "name": "Flash Crash",
      "description": "Sudden 30% drop in 24 hours",
      "shocks": {
        "all_crypto": -0.30,
        "volatility_spike": 5.0,
        "liquidity_crunch": -0.70
      },
      "duration": "1-7 days",
      "probability": "medium"
    },
    "exchange_hack": {
      "name": "Major Exchange Hack",
      "description": "Security breach causing market panic",
      "shocks": {
        "major_coins": -0.15,
        "alt_coins": -0.35,
        "confidence_shock": -0.50
      },
      "duration": "1-4 weeks",
      "probability": "medium"
    },
    "regulatory_crackdown": {
      "name": "Regulatory Crackdown",
      "description": "Severe regulatory restrictions",
      "shocks": {
        "all_crypto": -0.40,
        "trading_volume": -0.80,
        "liquidity": -0.60
      },
      "duration": "3-12 months",
      "probability": "high"
    }
  },
  "historical": {
    "march_2020": {
      "name": "COVID-19 Market Crash",
      "date": "2020-03-12",
      "actual_shocks": {
        "BTC": -0.39,
        "ETH": -0.43,
        "ALT_COINS": -0.50
      }
    },
    "may_2022": {
      "name": "LUNA/UST Collapse",
      "date": "2022-05-09",
      "actual_shocks": {
        "LUNA": -0.98,
        "BTC": -0.28,
        "ALT_COINS": -0.35
      }
    }
  }
}
```

---

## 📊 Data Structures & Interfaces

### Risk Assessment Response Format
```typescript
interface RiskAssessment {
  timestamp: number;
  portfolio: {
    totalValue: number;
    totalExposure: number;
    availableCash: number;
    utilizationRate: number;
  };
  riskMetrics: {
    portfolioVaR: {
      daily_95: number;
      daily_99: number;
      weekly_95: number;
      monthly_95: number;
    };
    expectedShortfall: {
      daily_95: number;
      daily_99: number;
    };
    volatility: {
      daily: number;
      weekly: number;
      monthly: number;
      annualized: number;
    };
    correlations: {
      averageCorrelation: number;
      maxCorrelation: number;
      diversificationRatio: number;
    };
    drawdown: {
      current: number;
      maximum: number;
      averageDrawdown: number;
      recoveryTime: number;
    };
  };
  riskScores: {
    overall: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
    concentration: 'LOW' | 'MEDIUM' | 'HIGH';
    correlation: 'LOW' | 'MEDIUM' | 'HIGH';
    volatility: 'LOW' | 'MEDIUM' | 'HIGH';
    liquidity: 'LOW' | 'MEDIUM' | 'HIGH';
  };
  alerts: RiskAlert[];
}

interface PositionRisk {
  pair: string;
  side: 'long' | 'short';
  size: number;
  entryPrice: number;
  currentPrice: number;
  marketValue: number;
  unrealizedPnL: number;
  unrealizedPnLPercent: number;
  risk: {
    positionVaR: {
      daily_95: number;
      daily_99: number;
    };
    volatility: number;
    beta: number;
    riskContribution: number;
    marginRequired: number;
    liquidationPrice: number;
    timeToLiquidation: string;
  };
  riskScore: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
  recommendations: string[];
}

interface RiskAlert {
  id: string;
  level: 'LOW' | 'MEDIUM' | 'HIGH' | 'CRITICAL';
  type: 'POSITION_SIZE' | 'VAR_BREACH' | 'CORRELATION' | 'VOLATILITY' | 'DRAWDOWN';
  pair?: string;
  message: string;
  currentValue: number;
  threshold: number;
  recommendation: string;
  createdAt: number;
  acknowledged: boolean;
  acknowledgedAt?: number;
  acknowledgedBy?: string;
}
```

### Position Sizing Recommendation Format
```typescript
interface PositionSizingRecommendation {
  pair: string;
  recommendations: {
    kelly: SizingMethod;
    volatility: SizingMethod;
    correlation: SizingMethod;
    recommended: {
      method: string;
      positionSize: number;
      units: number;
      maxRisk: number;
      reasoning: string;
    };
  };
  riskAnalysis: {
    expectedReturn: number;
    winProbability: number;
    riskRewardRatio: number;
    portfolioImpact: {
      newVaR: number;
      varIncrease: number;
      correlationImpact: number;
    };
  };
  constraints: {
    maxPositionSize: number;
    availableCash: number;
    marginRequired: number;
    diversificationLimit: number;
  };
}

interface SizingMethod {
  positionSize: number;
  units: number;
  riskAmount: number;
  riskPercent: number;
  explanation: string;
}
```

---

## 🔍 Monitoring & Debugging

### Log Files
```bash
logs/
├── risk.log              # General risk management operations
├── risk-error.log         # Error-specific logs
├── risk-calculations.log  # Detailed calculation logs
├── risk-alerts.log        # Alert generation and processing
└── risk-performance.log   # Performance metrics and timing
```

### Debug Commands
```bash
# Enable verbose risk calculation logging
LOG_LEVEL=debug npm start

# Monitor risk calculations in real-time
tail -f logs/risk-calculations.log | grep -E "(VaR|Correlation|PositionSize)"

# Check alert generation
tail -f logs/risk-alerts.log

# Monitor service connectivity
curl http://localhost:3003/api/health | jq '.services'

# Test specific risk calculations
curl -X POST http://localhost:3003/api/risk/debug/calculate \
  -H "Content-Type: application/json" \
  -d '{"type": "var", "params": {"confidence": 0.95}}'
```

### Common Issues & Solutions

#### 1. **Risk Calculations Failing**
```bash
# Check data availability from core service
curl http://localhost:3000/api/data | jq '.pairs'

# Verify sufficient historical data
curl http://localhost:3000/api/pair/RVN | jq '.history.closes | length'

# Check for NaN or invalid values in calculations
grep -E "(NaN|undefined|null)" logs/risk-calculations.log

# Common solutions:
# - Ensure core service has >30 data points per pair
# - Check for division by zero in volatility calculations
# - Validate correlation matrix for positive semi-definite property
```

#### 2. **Position Sizing Errors**
```bash
# Test Kelly criterion inputs
curl -X POST http://localhost:3003/api/risk/debug/kelly \
  -H "Content-Type: application/json" \
  -d '{
    "winRate": 0.6,
    "avgWin": 100,
    "avgLoss": -50,
    "capital": 10000
  }'

# Check for invalid win rates or risk-reward ratios
# Ensure avgLoss is negative and avgWin is positive
# Validate winRate is between 0 and 1
```

#### 3. **Alert System Issues**
```bash
# Check alert generation logic
curl http://localhost:3003/api/risk/alerts | jq '.activeAlerts'

# Test alert acknowledgment
curl -X POST http://localhost:3003/api/risk/alerts/TEST_ALERT/acknowledge

# Monitor alert escalation
grep "ESCALATED" logs/risk-alerts.log

# Common issues:
# - Alerts not clearing after conditions improve
# - Duplicate alerts for same condition
# - Escalation rules not triggering correctly
```

#### 4. **Service Integration Problems**
```bash
# Test connectivity to all integrated services
curl http://localhost:3000/api/health  # Core
curl http://localhost:3001/api/health  # ML
curl http://localhost:3004/api/health  # Execution

# Check for timeout issues
curl -w "%{time_total}\n" http://localhost:3003/api/risk/portfolio

# Verify data format compatibility
curl http://localhost:3003/api/risk/debug/integration
```

---

## 🚀 Performance Optimization

### Risk Calculation Optimization
```javascript
// Efficient correlation matrix calculation
class OptimizedCorrelationCalculator {
  constructor() {
    this.cache = new Map();
    this.cacheTimeout = 300000; // 5 minutes
  }
  
  async calculateCorrelationMatrix(priceData, useCache = true) {
    const cacheKey = this.generateCacheKey(priceData);
    
    if (useCache && this.cache.has(cacheKey)) {
      const cached = this.cache.get(cacheKey);
      if (Date.now() - cached.timestamp < this.cacheTimeout) {
        return cached.data;
      }
    }
    
    // Use efficient matrix operations
    const matrix = await this.calculateMatrixOptimized(priceData);
    
    this.cache.set(cacheKey, {
      data: matrix,
      timestamp: Date.now()
    });
    
    return matrix;
  }
  
  calculateMatrixOptimized(priceData) {
    // Use vectorized operations where possible
    const pairs = Object.keys(priceData);
    const returns = this.calculateReturnsMatrix(priceData);
    
    // Batch correlation calculations
    return this.batchCorrelationCalculation(returns, pairs);
  }
  
  calculateReturnsMatrix(priceData) {
    const returnsMatrix = {};
    
    Object.entries(priceData).forEach(([pair, prices]) => {
      returnsMatrix[pair] = this.calculateReturns(prices);
    });
    
    return returnsMatrix;
  }
  
  batchCorrelationCalculation(returns, pairs) {
    const matrix = {};
    const batchSize = 10;
    
    // Process correlations in batches to avoid blocking
    for (let i = 0; i < pairs.length; i += batchSize) {
      const batch = pairs.slice(i, i + batchSize);
      this.processBatch(batch, pairs, returns, matrix);
    }
    
    return matrix;
  }
}

// Memory-efficient VaR calculation
class OptimizedVaRCalculator {
  calculateHistoricalVaR(returns, portfolioValue, confidenceLevel) {
    // Use typed arrays for better performance
    const returnArray = new Float64Array(returns);
    
    // Efficient sorting using native sort
    returnArray.sort();
    
    const varIndex = Math.floor((1 - confidenceLevel) * returnArray.length);
    const varReturn = returnArray[varIndex];
    
    return portfolioValue * varReturn;
  }
  
  calculateRollingVaR(returns, window = 252, confidence = 0.95) {
    const rollingVaR = [];
    
    // Use sliding window with efficient array operations
    for (let i = window; i <= returns.length; i++) {
      const windowReturns = returns.slice(i - window, i);
      const var95 = this.calculateHistoricalVaR(windowReturns, 1, confidence);
      rollingVaR.push(var95);
    }
    
    return rollingVaR;
  }
}
```

### Concurrent Risk Processing
```javascript
// Worker threads for intensive calculations
const { Worker, isMainThread, parentPort, workerData } = require('worker_threads');

class ParallelRiskProcessor {
  constructor(maxWorkers = 4) {
    this.maxWorkers = maxWorkers;
    this.workers = [];
    this.taskQueue = [];
    this.results = new Map();
  }
  
  async calculateRiskMetrics(portfolio, pairs) {
    const tasks = pairs.map(pair => ({
      type: 'position_risk',
      pair: pair,
      data: portfolio.positions.find(p => p.pair === pair)
    }));
    
    // Distribute tasks across workers
    const results = await this.processTasksParallel(tasks);
    
    return this.aggregateResults(results);
  }
  
  async processTasksParallel(tasks) {
    const chunks = this.chunkArray(tasks, this.maxWorkers);
    const promises = chunks.map(chunk => this.processChunk(chunk));
    
    const results = await Promise.all(promises);
    return results.flat();
  }
  
  processChunk(chunk) {
    return new Promise((resolve, reject) => {
      const worker = new Worker(__filename, {
        workerData: { tasks: chunk }
      });
      
      worker.on('message', resolve);
      worker.on('error', reject);
      worker.on('exit', (code) => {
        if (code !== 0) {
          reject(new Error(`Worker stopped with exit code ${code}`));
        }
      });
    });
  }
  
  chunkArray(array, chunkSize) {
    const chunks = [];
    for (let i = 0; i < array.length; i += chunkSize) {
      chunks.push(array.slice(i, i + chunkSize));
    }
    return chunks;
  }
}

// Worker thread implementation
if (!isMainThread) {
  const { tasks } = workerData;
  const results = [];
  
  tasks.forEach(task => {
    switch (task.type) {
      case 'position_risk':
        results.push(calculatePositionRisk(task.pair, task.data));
        break;
      case 'correlation':
        results.push(calculateCorrelation(task.data));
        break;
    }
  });
  
  parentPort.postMessage(results);
}
```

---

## 🔒 Security & Production Considerations

### Risk Data Security
```javascript
// Secure risk data handling
class SecureRiskDataManager {
  constructor() {
    this.encryptionKey = process.env.RISK_ENCRYPTION_KEY;
    this.dataRetention = 7 * 24 * 60 * 60 * 1000; // 7 days
  }
  
  encryptSensitiveData(data) {
    // Encrypt portfolio values, positions, PnL
    const sensitiveFields = ['marketValue', 'unrealizedPnL', 'portfolioValue'];
    
    return this.recursiveEncrypt(data, sensitiveFields);
  }
  
  validateRiskRequest(req) {
    // Validate request source and parameters
    const allowedIPs = process.env.ALLOWED_IPS?.split(',') || ['127.0.0.1'];
    const clientIP = req.ip || req.connection.remoteAddress;
    
    if (!allowedIPs.includes(clientIP)) {
      throw new Error('Unauthorized IP address');
    }
    
    // Validate risk calculation parameters
    this.validateRiskParameters(req.body);
  }
  
  validateRiskParameters(params) {
    if (params.confidenceLevel && (params.confidenceLevel < 0.9 || params.confidenceLevel > 0.99)) {
      throw new Error('Invalid confidence level');
    }
    
    if (params.positionSize && (params.positionSize < 0 || params.positionSize > 1000000)) {
      throw new Error('Invalid position size');
    }
  }
  
  auditRiskCalculation(calculation, user, result) {
    const auditEntry = {
      timestamp: Date.now(),
      user: user,
      calculation: calculation.type,
      parameters: this.sanitizeParameters(calculation.parameters),
      result: this.sanitizeResult(result),
      ipAddress: calculation.source
    };
    
    this.writeAuditLog(auditEntry);
  }
}

// Production deployment configuration
const productionConfig = {
  security: {
    rateLimit: {
      windowMs: 60000,
      max: 100 // 100 requests per minute
    },
    cors: {
      origin: process.env.ALLOWED_ORIGINS?.split(',') || ['https://yourdomain.com'],
      credentials: false
    },
    headers: {
      'X-Content-Type-Options': 'nosniff',
      'X-Frame-Options': 'DENY',
      'X-XSS-Protection': '1; mode=block',
      'Strict-Transport-Security': 'max-age=31536000; includeSubDomains'
    }
  },
  monitoring: {
    healthCheckInterval: 30000,
    alertEscalationTimeout: 300000,
    maxConcurrentCalculations: 10
  },
  performance: {
    calculationTimeout: 30000,
    cacheTimeout: 300000,
    maxHistoryLength: 1000
  }
};
```

### Production Deployment
```bash
# Production environment setup
NODE_ENV=production npm start

# Process management with PM2
pm2 start src/main.js --name trading-bot-risk --instances 2

# Monitor risk service
pm2 monit trading-bot-risk

# Set up log rotation
pm2 install pm2-logrotate
pm2 set pm2-logrotate:max_size 50M
pm2 set pm2-logrotate:retain 30

# Configure system limits
ulimit -n 65536  # Increase file descriptor limit
echo 'trading-bot-risk soft nofile 65536' >> /etc/security/limits.conf
echo 'trading-bot-risk hard nofile 65536' >> /etc/security/limits.conf
```

---

## 📚 Best Practices & Guidelines

### Risk Management Best Practices
```javascript
// 1. Diversification Guidelines
const diversificationBestPractices = {
  maxPositionSize: 0.20,      // No position > 20% of portfolio
  maxCorrelation: 0.70,       // Keep correlations < 0.70
  minEffectiveAssets: 3,      // Effective number of assets
  maxSectorExposure: 0.40,    // No sector > 40% exposure
  rebalanceThreshold: 0.05    // Rebalance when allocation drifts 5%
};

// 2. Risk Budgeting
const riskBudgetingFramework = {
  portfolioVaR: {
    daily: 0.02,              // 2% daily VaR limit
    weekly: 0.05,             // 5% weekly VaR limit
    monthly: 0.10             // 10% monthly VaR limit
  },
  positionLimits: {
    individual: 0.05,         // 5% risk per position
    correlated: 0.15,         // 15% for correlated positions
    sector: 0.25              // 25% per sector/theme
  },
  stopLossFramework: {
    technical: 0.03,          // 3% technical stop
    volatility: 'dynamic',    // 2x daily volatility
    time: '30_days'           // Time-based stops
  }
};

// 3. Position Sizing Hierarchy
const positionSizingHierarchy = {
  primary: 'kelly_criterion',
  fallback: 'volatility_targeting',
  constraints: [
    'max_position_size',
    'correlation_limits',
    'liquidity_constraints',
    'margin_requirements'
  ],
  adjustments: [
    'signal_confidence',
    'market_regime',
    'portfolio_heat'
  ]
};
```

### Risk Monitoring Guidelines
```javascript
// Real-time monitoring framework
const monitoringFramework = {
  frequencies: {
    portfolio_risk: '30_seconds',
    position_risk: '1_minute',
    correlation_updates: '5_minutes',
    stress_tests: '1_hour',
    full_recalculation: '4_hours'
  },
  
  alertThresholds: {
    immediate: {
      portfolio_var_breach: 1.0,
      position_size_breach: 1.0,
      margin_call_risk: 0.8
    },
    warning: {
      correlation_spike: 0.8,
      volatility_spike: 2.0,
      concentration_risk: 0.7
    },
    info: {
      performance_drift: 0.1,
      regime_change: 'detected',
      rebalance_needed: 0.05
    }
  },
  
  escalationProcedures: {
    critical: [
      'immediate_notification',
      'position_review',
      'auto_hedge_consideration'
    ],
    high: [
      'alert_notification',
      'manual_review_required',
      'risk_committee_notification'
    ],
    medium: [
      'log_alert',
      'daily_review',
      'trend_monitoring'
    ]
  }
};
```

### Stress Testing Best Practices
```javascript
// Comprehensive stress testing framework
const stressTesting = {
  frequency: {
    standard_scenarios: 'daily',
    custom_scenarios: 'weekly',
    historical_scenarios: 'monthly',
    monte_carlo: 'weekly'
  },
  
  scenarios: {
    market_based: [
      'market_crash_2008',
      'flash_crash_2010',
      'covid_crash_2020',
      'crypto_winter_2018'
    ],
    crypto_specific: [
      'exchange_hack',
      'regulatory_crackdown',
      'stablecoin_depeg',
      'mining_ban'
    ],
    liquidity_based: [
      'liquidity_crunch',
      'market_maker_failure',
      'exchange_outage',
      'network_congestion'
    ]
  },
  
  validation: {
    backtesting: 'quarterly',
    model_validation: 'semi_annually',
    parameter_stability: 'monthly',
    performance_attribution: 'weekly'
  }
};
```

---

## 📋 API Usage Examples

### Complete Risk Management Integration
```javascript
const axios = require('axios');

class ComprehensiveRiskManager {
  constructor(riskServiceUrl = 'http://localhost:3003') {
    this.riskService = axios.create({ 
      baseURL: riskServiceUrl,
      timeout: 30000 
    });
    this.riskCache = new Map();
    this.alertSubscribers = new Set();
  }
  
  async initializeRiskManagement() {
    try {
      // Initial risk assessment
      const [portfolio, positions, correlations] = await Promise.all([
        this.getPortfolioRisk(),
        this.getPositionRisks(),
        this.getCorrelationAnalysis()
      ]);
      
      // Set up real-time monitoring
      this.startRiskMonitoring();
      
      return {
        status: 'initialized',
        portfolio,
        positions,
        correlations,
        timestamp: Date.now()
      };
    } catch (error) {
      console.error('Risk management initialization failed:', error);
      throw error;
    }
  }
  
  async getPortfolioRisk() {
    const response = await this.riskService.get('/api/risk/portfolio');
    return response.data;
  }
  
  async getPositionRisks() {
    const response = await this.riskService.get('/api/risk/positions');
    return response.data;
  }
  
  async getCorrelationAnalysis() {
    const response = await this.riskService.get('/api/risk/correlations');
    return response.data;
  }
  
  async calculateOptimalPositionSize(tradeRequest) {
    try {
      const response = await this.riskService.post('/api/risk/position-size', {
        pair: tradeRequest.pair,
        signal: tradeRequest.signal,
        confidence: tradeRequest.confidence,
        entryPrice: tradeRequest.entryPrice,
        stopLoss: tradeRequest.stopLoss,
        takeProfit: tradeRequest.takeProfit,
        method: 'kelly',
        riskTolerance: 'medium'
      });
      
      return response.data;
    } catch (error) {
      console.error('Position sizing calculation failed:', error);
      return this.getFallbackPositionSize(tradeRequest);
    }
  }
  
  async runStressTest(scenarios = ['market_crash', 'crypto_winter']) {
    try {
      const response = await this.riskService.post('/api/risk/stress-test', {
        scenarios: scenarios,
        confidenceLevel: 0.95
      });
      
      return this.analyzeStressTestResults(response.data);
    } catch (error) {
      console.error('Stress test failed:', error);
      return null;
    }
  }
  
  analyzeStressTestResults(results) {
    const analysis = {
      worstCase: null,
      averageLoss: 0,
      survivableScenarios: 0,
      recommendations: []
    };
    
    Object.entries(results.stressTestResults).forEach(([scenario, result]) => {
      if (!analysis.worstCase || result.portfolioLossPercent < analysis.worstCase.portfolioLossPercent) {
        analysis.worstCase = { scenario, ...result };
      }
      
      analysis.averageLoss += Math.abs(result.portfolioLossPercent);
      
      if (result.survivability === 'HIGH') {
        analysis.survivableScenarios++;
      }
    });
    
    analysis.averageLoss /= Object.keys(results.stressTestResults).length;
    
    // Generate recommendations based on stress test results
    if (analysis.worstCase.portfolioLossPercent < -50) {
      analysis.recommendations.push('Consider reducing overall portfolio exposure');
      analysis.recommendations.push('Implement hedging strategies for tail risk');
    }
    
    if (analysis.survivableScenarios < 2) {
      analysis.recommendations.push('Improve portfolio diversification');
      analysis.recommendations.push('Consider defensive positions');
    }
    
    return {
      ...results,
      analysis
    };
  }
  
  async monitorRealTimeRisk() {
    const riskData = await this.getPortfolioRisk();
    
    // Check for risk limit breaches
    const breaches = this.checkRiskLimitBreaches(riskData);
    
    if (breaches.length > 0) {
      await this.handleRiskBreaches(breaches);
    }
    
    return {
      riskData,
      breaches,
      timestamp: Date.now()
    };
  }
  
  checkRiskLimitBreaches(riskData) {
    const breaches = [];
    
    // Check VaR limits
    const dailyVaR = Math.abs(riskData.riskMetrics.portfolioVaR.daily_95);
    const portfolioValue = riskData.portfolio.totalValue;
    const varPercent = dailyVaR / portfolioValue;
    
    if (varPercent > 0.02) { // 2% daily VaR limit
      breaches.push({
        type: 'VAR_BREACH',
        severity: 'HIGH',
        current: varPercent,
        limit: 0.02,
        message: `Daily VaR (${(varPercent * 100).toFixed(2)}%) exceeds 2% limit`
      });
    }
    
    // Check position concentration
    if (riskData.riskScores.concentration === 'HIGH') {
      breaches.push({
        type: 'CONCENTRATION_RISK',
        severity: 'MEDIUM',
        message: 'Portfolio concentration risk is high'
      });
    }
    
    // Check correlation risk
    if (riskData.riskMetrics.correlations.averageCorrelation > 0.7) {
      breaches.push({
        type: 'CORRELATION_RISK',
        severity: 'MEDIUM',
        current: riskData.riskMetrics.correlations.averageCorrelation,
        limit: 0.7,
        message: 'Average portfolio correlation exceeds 70%'
      });
    }
    
    return breaches;
  }
  
  async handleRiskBreaches(breaches) {
    for (const breach of breaches) {
      // Log the breach
      console.warn(`Risk Breach Detected: ${breach.message}`);
      
      // Notify subscribers
      this.notifyAlertSubscribers(breach);
      
      // Take automated action for critical breaches
      if (breach.severity === 'HIGH') {
        await this.handleCriticalBreach(breach);
      }
    }
  }
  
  async handleCriticalBreach(breach) {
    switch (breach.type) {
      case 'VAR_BREACH':
        // Suggest position reductions
        const positions = await this.getPositionRisks();
        const highRiskPositions = positions.positions
          .filter(p => p.riskScore === 'HIGH')
          .sort((a, b) => b.risk.riskContribution - a.risk.riskContribution);
        
        if (highRiskPositions.length > 0) {
          console.log(`Recommend reducing ${highRiskPositions[0].pair} position`);
          // Could automatically trigger position reduction in execution service
        }
        break;
        
      case 'MARGIN_CALL_RISK':
        // Emergency position closure
        console.log('CRITICAL: Margin call risk detected - immediate action required');
        break;
    }
  }
  
  subscribeToAlerts(callback) {
    this.alertSubscribers.add(callback);
    return () => this.alertSubscribers.delete(callback);
  }
  
  notifyAlertSubscribers(alert) {
    this.alertSubscribers.forEach(callback => {
      try {
        callback(alert);
      } catch (error) {
        console.error('Alert subscriber error:', error);
      }
    });
  }
  
  startRiskMonitoring() {
    // Monitor risk every 30 seconds
    this.riskMonitoringInterval = setInterval(async () => {
      try {
        await this.monitorRealTimeRisk();
      } catch (error) {
        console.error('Risk monitoring error:', error);
      }
    }, 30000);
  }
  
  stopRiskMonitoring() {
    if (this.riskMonitoringInterval) {
      clearInterval(this.riskMonitoringInterval);
    }
  }
  
  getFallbackPositionSize(tradeRequest) {
    // Conservative fallback sizing when risk service is unavailable
    const baseSize = tradeRequest.accountBalance * 0.02; // 2% risk
    const stopDistance = Math.abs(tradeRequest.entryPrice - tradeRequest.stopLoss);
    const positionSize = baseSize / stopDistance;
    
    return {
      recommendations: {
        recommended: {
          method: 'fallback_fixed_risk',
          positionSize: positionSize,
          maxRisk: baseSize,
          reasoning: 'Conservative fallback sizing (2% account risk)'
        }
      }
    };
  }
}

// Usage example with trading bot integration
async function integrateRiskManagement() {
  const riskManager = new ComprehensiveRiskManager();
  
  try {
    // Initialize risk management
    const initialization = await riskManager.initializeRiskManagement();
    console.log('Risk management initialized:', initialization.status);
    
    // Subscribe to risk alerts
    const unsubscribe = riskManager.subscribeToAlerts((alert) => {
      console.log(`Risk Alert: ${alert.message}`);
      
      // Could send notifications, update dashboard, etc.
      if (alert.severity === 'HIGH') {
        // Send urgent notification
        sendUrgentNotification(alert);
      }
    });
    
    // Example: Calculate position size for a new trade
    const tradeRequest = {
      pair: 'RVN_USDT',
      signal: 'BUY',
      confidence: 0.78,
      entryPrice: 0.0245,
      stopLoss: 0.0233,
      takeProfit: 0.0270,
      accountBalance: 50000
    };
    
    const positionSizing = await riskManager.calculateOptimalPositionSize(tradeRequest);
    console.log('Optimal position size:', positionSizing.recommendations.recommended);
    
    // Run stress test
    const stressTest = await riskManager.runStressTest(['market_crash', 'crypto_winter', 'flash_crash']);
    console.log('Stress test results:', stressTest.analysis);
    
    // Monitor risk for 1 hour, then cleanup
    setTimeout(() => {
      riskManager.stopRiskMonitoring();
      unsubscribe();
      console.log('Risk monitoring stopped');
    }, 3600000); // 1 hour
    
  } catch (error) {
    console.error('Risk management integration failed:', error);
  }
}

// Helper function for urgent notifications
function sendUrgentNotification(alert) {
  // Implementation would depend on notification system
  console.log(`URGENT RISK ALERT: ${alert.message}`);
  
  // Could integrate with:
  // - Email notifications
  // - SMS alerts
  // - Slack/Discord webhooks
  // - Dashboard real-time updates
}
```

---

## 🎯 Future Roadmap

### Planned Features

#### 1. **Advanced Risk Models**
- **Factor Risk Models**: Multi-factor risk attribution and decomposition
- **Regime-Aware Models**: Risk models that adapt to market regimes
- **Machine Learning Risk**: ML-based risk prediction and early warning systems
- **Tail Risk Analytics**: Advanced extreme value theory and tail risk modeling

#### 2. **Enhanced Portfolio Management**
- **Dynamic Hedging**: Automated delta, gamma, and vega hedging
- **Risk Parity**: Advanced risk parity and equal risk contribution portfolios
- **Black-Litterman**: Bayesian portfolio optimization with views
- **Multi-Asset Support**: Traditional assets, crypto, derivatives, and alternatives

#### 3. **Real-time Risk Controls**
- **Circuit Breakers**: Automated trading halts on risk threshold breaches
- **Dynamic Stop-Losses**: Volatility-adjusted and trend-following stops
- **Position Limits**: Real-time position and exposure limit enforcement
- **Margin Management**: Automated margin monitoring and optimization

#### 4. **Advanced Analytics**
- **Attribution Analysis**: Performance and risk attribution to factors
- **Scenario Generation**: Monte Carlo and historical scenario generation
- **Backtesting Framework**: Risk model backtesting and validation
- **Benchmarking**: Risk-adjusted performance vs benchmarks

#### 5. **Integration Enhancements**
- **Options Strategies**: Volatility trading and option risk management
- **Cross-Exchange Risk**: Multi-exchange position and settlement risk
- **Counterparty Risk**: Credit risk and counterparty exposure monitoring
- **Regulatory Compliance**: Risk reporting and regulatory requirement adherence

### Technical Improvements

#### 1. **Performance & Scalability**
- **GPU Acceleration**: CUDA-based risk calculations for large portfolios
- **Distributed Computing**: Risk calculations across multiple nodes
- **Stream Processing**: Real-time risk calculation streaming architecture
- **Memory Optimization**: Efficient data structures for large-scale operations

#### 2. **Model Enhancement**
- **Ensemble Risk Models**: Multiple model combination and weighting
- **Dynamic Calibration**: Real-time model parameter updates
- **Robust Statistics**: Outlier-resistant risk metrics and calculations
- **Non-Parametric Methods**: Distribution-free risk measures

#### 3. **Data Infrastructure**
- **Time Series Database**: Specialized storage for risk metrics history
- **Data Quality**: Automated data validation and cleansing
- **Alternative Data**: Integration of sentiment, news, and social data
- **Real-time Feeds**: Low-latency market data integration

---

## 📚 Additional Resources

### Risk Management Literature
- **"Value at Risk" by Philippe Jorion**: Comprehensive VaR methodology
- **"Risk Management and Financial Institutions" by John Hull**: Institutional risk management
- **"Portfolio Risk Analysis" by Bernd Scherer**: Modern portfolio risk techniques
- **"The Concepts and Practice of Mathematical Finance" by Mark Joshi**: Quantitative risk modeling

### Regulatory Guidelines
- **Basel III**: International banking regulation framework
- **MiFID II**: European investment services regulation
- **CFTC Guidance**: US derivatives risk management requirements
- **IOSCO Principles**: International securities regulation standards

### Technical References
- **QuantLib**: Open-source quantitative finance library
- **Risk Management Standards**: ISO 31000 risk management principles
- **GARP FRM**: Financial Risk Manager certification materials
- **CFA Institute**: Risk management and portfolio theory resources

### Related Documentation
- **Trading-Bot-Core Integration**: See `trading-bot-core/README.md`
- **ML Service Integration**: See `trading-bot-ml/README.md`
- **Backtest Service Integration**: See `trading-bot-backtest/README.md`
- **Execution Service Integration**: See `trading-bot-execution/README.md` (planned)
- **Dashboard Integration**: See `trading-bot-dashboard/README.md`

---

## 📊 Version Information

- **Current Version**: 1.0.0 (Planned)
- **Node.js Compatibility**: >=16.0.0
- **Dependencies**: Express, Axios, Lodash, Math.js, Worker Threads
- **Last Updated**: January 2025
- **API Stability**: Development/Planning Phase

### Development Roadmap
- **Phase 1 (Q1 2025)**: Core risk calculations (VaR, correlation, position sizing)
- **Phase 2 (Q2 2025)**: Real-time monitoring and alerting system
- **Phase 3 (Q3 2025)**: Advanced analytics and stress testing
- **Phase 4 (Q4 2025)**: Machine learning integration and optimization

---

## 🚀 Implementation Checklist

### For Developers
- [ ] Set up development environment and dependencies
- [ ] Implement core risk calculation algorithms (VaR, correlation)
- [ ] Build position sizing engine with multiple methodologies
- [ ] Create real-time risk monitoring system
- [ ] Develop alerting and notification framework
- [ ] Implement stress testing capabilities
- [ ] Build RESTful API with comprehensive endpoints
- [ ] Create integration adapters for other services
- [ ] Implement comprehensive test suite
- [ ] Add performance optimization and caching

### For Risk Managers
- [ ] Define risk limits and thresholds for organization
- [ ] Configure stress testing scenarios
- [ ] Set up alerting rules and escalation procedures
- [ ] Validate risk model implementations
- [ ] Review and approve position sizing methodologies
- [ ] Establish risk reporting requirements
- [ ] Define risk control and override procedures
- [ ] Create risk management policies and procedures

### For System Administrators
- [ ] Configure production environment variables
- [ ] Set up secure data handling and encryption
- [ ] Configure monitoring and logging systems
- [ ] Set up backup and disaster recovery procedures
- [ ] Configure security headers and access controls
- [ ] Set up performance monitoring and alerting
- [ ] Configure process management and auto-restart
- [ ] Test system integration and connectivity

### For Integration Teams
- [ ] Plan integration with execution service
- [ ] Design risk-adjusted signal enhancement for core service
- [ ] Create risk feature engineering for ML service
- [ ] Build risk dashboard components for frontend
- [ ] Test cross-service communication and error handling
- [ ] Implement fallback mechanisms for service outages
- [ ] Create comprehensive integration test suite
- [ ] Document integration patterns and best practices

---

*This technical manual serves as the complete specification and development guide for the trading-bot-risk service. The risk management service provides comprehensive portfolio protection and optimization capabilities, ensuring safe and profitable trading operations through advanced risk analytics and real-time monitoring.*# Trading Bot Risk Management - Technical Manual

## 🛡️ Overview

The **trading-bot-risk** is the comprehensive risk management and portfolio protection service of the modular trading bot architecture, providing real-time risk assessment, position sizing optimization, and advanced risk analytics. Operating on **Port 3003**, it integrates with all other services to deliver sophisticated risk management capabilities and portfolio protection mechanisms.

### Key Capabilities
- **Real-time Risk Assessment** with position-level and portfolio-level risk metrics
- **Dynamic Position Sizing** using Kelly Criterion, volatility-based, and correlation-adjusted methods
- **Advanced Risk Analytics** including VaR, CVaR, and stress testing
- **Portfolio Correlation Analysis** with cross-asset risk attribution
- **Automated Risk Controls** with stop-loss optimization and drawdown protection
- **RESTful API** serving risk metrics and portfolio recommendations
- **Integration Layer** connecting with execution, core, ML, and backtest services

---

## 🏗️ Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│               TRADING-BOT-RISK (Port 3003)                 │
├─────────────────────────────────────────────────────────────┤
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│  │  RiskCalculator │  │ PositionSizer   │  │CorrelationEngine││
│  │                 │  │                 │  │                 ││
│  │ • VaR/CVaR      │  │ • Kelly Criterion│ • Cross-Asset    ││
│  │ • Volatility    │  │ • Vol Targeting │   Correlations    ││
│  │ • Drawdown      │  │ • Risk Parity   │ • Dynamic        ││
│  │ • Sharpe/Sortino│  │ • Fixed Frac    │   Rebalancing    ││
│  └─────────────────┘  └─────────────────┘  └─────────────────┘│
│                                 │                             │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐│
│  │  RiskMonitor    │  │  RiskServer     │  │ StressTestEngine││
│  │                 │  │                 │  │                 ││
│  │ • Real-time     │  │ • RESTful API   │  │ • Historical    ││
│  │   Alerts        │  │ • Health Checks │  │   Scenarios     ││
│  │ • Portfolio     │  │ • Data Endpoints│  │ • Monte Carlo   ││
│  │   Health        │  │ • Risk Reports  │  │ • Tail Risk     ││
│  └─────────────────┘  └─────────────────┘  └─────────────────┘│
└─────────────────────────────────────────────────────────────┘
                               │
                    ┌──────────┼──────────┐
                    │          │          │
          ┌─────────▼─┐  ┌─────▼─────┐  ┌─▼──────────┐
          │   Core    │  │    ML     │  │Execution   │
          │ Service   │  │ Service   │  │  Service   │
          │(Port 3000)│  │(Port 3001)│  │(Port 3004) │
          └───────────┘  └───────────┘  └────────────┘
                               │
                    ┌──────────┼──────────┐
                    │          │          │
          ┌─────────▼─┐  ┌─────▼─────┐
          │ Backtest  │  │ Dashboard │
          │ Service   │  │  Service  │
          │(Port 3002)│  │(Port 3005)│
          └───────────┘  └───────────┘
```

---

## 🛠️ Quick Start

### Prerequisites
- **Node.js** >= 16.0.0
- **npm** >= 8.0.0
- **trading-bot-core** running on Port 3000
- **trading-bot-ml** running on Port 3001 (optional)
- **trading-bot-backtest** running on Port 3002 (optional)
- **trading-bot-execution** running on Port 3004 (optional)

### Installation

1. **Clone and Setup**
```bash
git clone <repository-url>
cd trading-bot-risk
npm install
```

2. **Environment Configuration**
```bash
cp .env.example .env
# Configure service URLs and risk parameters
```

3. **Start the Risk Management Service**
```bash
npm start
```

4. **Verify Installation**
```bash
# Check risk service health
curl http://localhost:3003/api/health

# Get portfolio risk assessment
curl http://localhost:3003/api/risk/portfolio

# Test position sizing recommendation
curl -X POST http://localhost:3003/api/risk/position-size \
  -H "Content-Type: application/json" \
  -d '{"pair": "RVN", "signal": "BUY", "confidence": 0.75}'
```

### Verify Service Connections
```bash
# Ensure required services are running
curl http://localhost:3000/api/health  # Core service
curl http://localhost:3001/api/health  # ML service (optional)
curl http://localhost:3004/api/health  # Execution service (optional)

# Check risk service connectivity
curl http://localhost:3003/api/health | jq '.services'
```

---

## 🔌 API Reference

### Base URL
```
http://localhost:3003
```

### Core Endpoints

#### 1. **GET /api/health**
Risk management service health check with connected services status.

**Response:**
```json
{
  "status": "healthy",
  "service": "trading-bot-risk",
  "timestamp": 1704067200000,
  "uptime": "02:45:30",
  "services": {
    "core": {
      "status": "healthy",
      "lastCheck": 1704067195000
    },
    "ml": {
      "status": "healthy",
      "lastCheck": 1704067190000
    },
    "execution": {
      "status": "healthy", 
      "lastCheck": 1704067185000
    }
  },
  "riskEngine": {
    "status": "operational",
    "calculations": {
      "completed": 1542,
      "failed": 3,
      "averageTime": "45ms"
    }
  }
}
```

#### 2. **GET /api/risk/portfolio**
Comprehensive portfolio risk assessment.

**Response:**
```json
{
  "timestamp": 1704067200000,
  "portfolio": {
    "totalValue": 50000.00,
    "totalExposure": 45000.00,
    "availableCash": 5000.00,
    "utilizationRate": 0.90
  },
  "riskMetrics": {
    "portfolioVaR": {
      "daily_95": -850.50,
      "daily_99": -1205.75,
      "weekly_95": -1890.25,
      "monthly_95": -3780.50
    },
    "expectedShortfall": {
      "daily_95": -1125.30,
      "daily_99": -1654.20
    },
    "volatility": {
      "daily": 0.0234,
      "weekly": 0.0521,
      "monthly": 0.1045,
      "annualized": 0.3720
    },
    "correlations": {
      "averageCorrelation": 0.65,
      "maxCorrelation": 0.89,
      "diversificationRatio": 0.78
    },
    "drawdown": {
      "current": 0.0125,
      "maximum": 0.0890,
      "averageDrawdown": 0.0234,
      "recoveryTime": 5.5
    }
  },
  "riskScores": {
    "overall": "MEDIUM",
    "concentration": "HIGH",
    "correlation": "MEDIUM", 
    "volatility": "LOW",
    "liquidity": "HIGH"
  },
  "alerts": [
    {
      "level": "WARNING",
      "type": "CONCENTRATION_RISK",
      "message": "XMR position represents 35% of portfolio",
      "recommendation": "Consider reducing position or hedging"
    }
  ]
}
```

#### 3. **GET /api/risk/positions**
Individual position risk analysis.

**Response:**
```json
{
  "timestamp": 1704067200000,
  "positions": [
    {
      "pair": "XMR_USDT",
      "side": "long",
      "size": 100.5,
      "entryPrice": 158.45,
      "currentPrice": 162.30,
      "marketValue": 16311.15,
      "unrealizedPnL": 387.25,
      "unrealizedPnLPercent": 2.44,
      "risk": {
        "positionVaR": {
          "daily_95": -285.50,
          "daily_99": -412.75
        },
        "volatility": 0.0456,
        "beta": 1.23,
        "riskContribution": 0.42,
        "marginRequired": 1631.12,
        "liquidationPrice": 142.61,
        "timeToLiquidation": "∞"
      },
      "riskScore": "MEDIUM",
      "recommendations": [
        "Consider taking partial profits at current levels",
        "Monitor support at $155.00"
      ]
    },
    {
      "pair": "RVN_USDT", 
      "side": "long",
      "size": 45000,
      "entryPrice": 0.0234,
      "currentPrice": 0.0245,
      "marketValue": 1102.50,
      "unrealizedPnL": 49.50,
      "unrealizedPnLPercent": 4.70,
      "risk": {
        "positionVaR": {
          "daily_95": -45.25,
          "daily_99": -67.80
        },
        "volatility": 0.0678,
        "beta": 0.89,
        "riskContribution": 0.08,
        "marginRequired": 110.25,
        "liquidationPrice": 0.0210,
        "timeToLiquidation": "∞"
      },
      "riskScore": "LOW",
      "recommendations": [
        "Position size appropriate for volatility",
        "Consider adding to position on dips"
      ]
    }
  ],
  "summary": {
    "totalPositions": 2,
    "totalRisk": 0.50,
    "highestRisk": "XMR_USDT",
    "diversificationScore": 0.85,
    "recommendedActions": [
      "Reduce XMR position size",
      "Consider adding uncorrelated assets"
    ]
  }
}
```

#### 4. **POST /api/risk/position-size**
Calculate optimal position size for a new trade.

**Request Body:**
```json
{
  "pair": "BEL_USDT",
  "signal": "BUY",
  "confidence": 0.78,
  "entryPrice": 0.5640,
  "stopLoss": 0.5358,
  "takeProfit": 0.6204,
  "method": "kelly",
  "riskTolerance": "medium"
}
```

**Response:**
```json
{
  "pair": "BEL_USDT",
  "recommendations": {
    "kelly": {
      "positionSize": 2450.00,
      "units": 4345.74,
      "riskAmount": 245.00,
      "riskPercent": 0.49,
      "explanation": "Kelly criterion optimal sizing based on win rate and risk-reward"
    },
    "volatility": {
      "positionSize": 2100.00,
      "units": 3723.40,
      "riskAmount": 210.00,
      "riskPercent": 0.42,
      "explanation": "Volatility-adjusted sizing based on historical price movements"
    },
    "correlation": {
      "positionSize": 1850.00,
      "units": 3280.50,
      "riskAmount": 185.00,
      "riskPercent": 0.37,
      "explanation": "Correlation-adjusted sizing considering portfolio exposure"
    },
    "recommended": {
      "method": "kelly",
      "positionSize": 2450.00,
      "units": 4345.74,
      "maxRisk": 245.00,
      "reasoning": "Kelly method selected due to high signal confidence and favorable risk-reward"
    }
  },
  "riskAnalysis": {
    "expectedReturn": 0.10,
    "winProbability": 0.78,
    "riskRewardRatio": 3.0,
    "portfolioImpact": {
      "newVaR": -892.30,
      "varIncrease": -41.80,
      "correlationImpact": 0.03
    }
  },
  "constraints": {
    "maxPositionSize": 5000.00,
    "availableCash": 5000.00,
    "marginRequired": 245.00,
    "diversificationLimit": 4500.00
  }
}
```

#### 5. **GET /api/risk/correlations**
Portfolio correlation analysis and diversification metrics.

**Response:**
```json
{
  "timestamp": 1704067200000,
  "correlationMatrix": {
    "XMR_USDT": {
      "RVN_USDT": 0.65,
      "BEL_USDT": 0.72,
      "DOGE_USDT": 0.58
    },
    "RVN_USDT": {
      "BEL_USDT": 0.45,
      "DOGE_USDT": 0.67
    },
    "BEL_USDT": {
      "DOGE_USDT": 0.52
    }
  },
  "diversificationMetrics": {
    "effectiveAssets": 2.34,
    "diversificationRatio": 0.78,
    "concentrationIndex": 0.42,
    "maxWeight": 0.35,
    "herfindahlIndex": 0.28
  },
  "timeframes": {
    "daily": {
      "averageCorrelation": 0.58,
      "maxCorrelation": 0.72
    },
    "weekly": {
      "averageCorrelation": 0.62,
      "maxCorrelation": 0.78
    },
    "monthly": {
      "averageCorrelation": 0.65,
      "maxCorrelation": 0.81
    }
  },
  "recommendations": [
    "Portfolio shows high correlation during stress periods",
    "Consider adding non-crypto assets for diversification",
    "XMR and BEL show concerning correlation of 0.72"
  ]
}
```

#### 6. **POST /api/risk/stress-test**
Perform stress testing on portfolio under various scenarios.

**Request Body:**
```json
{
  "scenarios": ["market_crash", "crypto_winter", "flash_crash", "custom"],
  "customScenario": {
    "name": "Exchange Hack",
    "shocks": {
      "XMR_USDT": -0.15,
      "RVN_USDT": -0.25,
      "BEL_USDT": -0.30
    }
  },
  "confidenceLevel": 0.95
}
```

**Response:**
```json
{
  "timestamp": 1704067200000,
  "stressTestResults": {
    "market_crash": {
      "scenario": "2008-style market crash (-50% across assets)",
      "portfolioLoss": -22500.00,
      "portfolioLossPercent": -45.0,
      "timeToRecover": "18-24 months",
      "worstPosition": "XMR_USDT",
      "survivability": "HIGH"
    },
    "crypto_winter": {
      "scenario": "Extended crypto bear market (-80% over 12 months)",
      "portfolioLoss": -36000.00,
      "portfolioLossPercent": -72.0,
      "timeToRecover": "36-48 months",
      "worstPosition": "All positions",
      "survivability": "MEDIUM"
    },
    "flash_crash": {
      "scenario": "Sudden 30% drop in 24 hours",
      "portfolioLoss": -13500.00,
      "portfolioLossPercent": -27.0,
      "timeToRecover": "3-6 months",
      "worstPosition": "XMR_USDT",
      "survivability": "HIGH"
    },
    "custom": {
      "scenario": "Exchange Hack",
      "portfolioLoss": -8925.00,
      "portfolioLossPercent": -17.85,
      "timeToRecover": "2-4 months",
      "worstPosition": "BEL_USDT",
      "survivability": "HIGH"
    }
  },
  "recommendations": [
    "Portfolio shows resilience to individual shocks",
    "Consider hedging strategies for crypto winter scenario", 
    "Reduce position concentration to improve stress test results"
  ],
  "riskLimits": {
    "maxAcceptableLoss": -10000.00,
    "breachedScenarios": ["crypto_winter", "market_crash"],
    "requiredAdjustments": "Reduce total exposure by 25%"
  }
}
```

#### 7. **GET /api/risk/alerts**
Active risk alerts and warnings.

**Response:**
```json
{
  "timestamp": 1704067200000,
  "activeAlerts": [
    {
      "id": "ALERT_001",
      "level": "HIGH",
      "type": "POSITION_SIZE",
      "pair": "XMR_USDT",
      "message": "Position exceeds 30% portfolio limit",
      "currentValue": 0.35,
      "threshold": 0.30,
      "recommendation": "Reduce position by $2,500",
      "createdAt": 1704065400000,
      "acknowledged": false
    },
    {
      "id": "ALERT_002",
      "level": "MEDIUM",
      "type": "CORRELATION",
      "pairs": ["XMR_USDT", "BEL_USDT"],
      "message": "High correlation detected between major positions",
      "currentValue": 0.72,
      "threshold": 0.70,
      "recommendation": "Consider diversifying into uncorrelated assets",
      "createdAt": 1704066000000,
      "acknowledged": false
    },
    {
      "id": "ALERT_003",
      "level": "LOW",
      "type": "VOLATILITY",
      "pair": "RVN_USDT",
      "message": "Volatility spike detected",
      "currentValue": 0.089,
      "threshold": 0.080,
      "recommendation": "Monitor position closely, consider reducing size",
      "createdAt": 1704066600000,
      "acknowledged": true
    }
  ],
  "summary": {
    "total": 3,
    "high": 1,
    "medium": 1,
    "low": 1,
    "acknowledged": 1,
    "unacknowledged": 2
  }
}
```

#### 8. **POST /api/risk/alerts/:id/acknowledge**
Acknowledge a risk alert.

**Response:**
```json
{
  "alertId": "ALERT_001",
  "acknowledged": true,
  "acknowledgedAt": 1704067200000,
  "acknowledgedBy": "system"
}
```

---

## 🧮 Risk Calculation Engine

### Core Risk Metrics

#### 1. **Value at Risk (VaR)**
Statistical measure of potential loss over a specific time period.

**Implementation:**
```javascript
class VaRCalculator {
  constructor(confidenceLevel = 0.95, timeHorizon = 1) {
    this.confidenceLevel = confidenceLevel;
    this.timeHorizon = timeHorizon;
  }
  
  // Historical simulation VaR
  calculateHistoricalVaR(returns, portfolioValue) {
    const sortedReturns = returns.sort((a, b) => a - b);
    const varIndex = Math.floor((1 - this.confidenceLevel) * sortedReturns.length);
    const varReturn = sortedReturns[varIndex];
    
    return {
      var: portfolioValue * varReturn * Math.sqrt(this.timeHorizon),
      percentile: this.confidenceLevel * 100,
      worstReturn: varReturn,
      timeHorizon: this.timeHorizon
    };
  }
  
  // Parametric VaR (assumes normal distribution)
  calculateParametricVaR(returns, portfolioValue) {
    const mean = this.calculateMean(returns);
    const std = this.calculateStandardDeviation(returns);
    const zScore = this.getZScore(this.confidenceLevel);
    
    const varReturn = mean - (zScore * std);
    
    return {
      var: portfolioValue * varReturn * Math.sqrt(this.timeHorizon),
      mean: mean,
      volatility: std,
      zScore: zScore
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
  
  getZScore(confidenceLevel) {
    const zScores = {
      0.90: 1.282,
      0.95: 1.645,
      0.99: 2.326
    };
    return zScores[confidenceLevel] || 1.645;
  }
}
```

#### 2. **Expected Shortfall (CVaR)**
Average of losses beyond the VaR threshold.

**Implementation:**
```javascript
class ExpectedShortfallCalculator {
  calculateExpectedShortfall(returns, portfolioValue, confidenceLevel = 0.95) {
    const sortedReturns = returns.sort((a, b) => a - b);
    const varIndex = Math.floor((1 - confidenceLevel) * sortedReturns.length);
    
    // Take average of all returns worse than VaR
    const tailReturns = sortedReturns.slice(0, varIndex + 1);
    const averageTailReturn = this.calculateMean(tailReturns);
    
    return {
      expectedShortfall: portfolioValue * averageTailReturn,
      varThreshold: portfolioValue * sortedReturns[varIndex],
      tailReturns: tailReturns.length,
      averageTailReturn: averageTailReturn
    };
  }
  
  // Conditional VaR for specific time horizons
  calculateConditionalVaR(returns, portfolioValue, timeHorizons = [1, 7, 30]) {
    const results = {};
    
    timeHorizons.forEach(horizon => {
      const adjustedReturns = returns.map(r => r * Math.sqrt(horizon));
      results[`${horizon}d`] = this.calculateExpectedShortfall(
        adjustedReturns, 
        portfolioValue
      );
    });
    
    return results;
  }
}
```

#### 3. **Portfolio Correlation Analysis**
Measure diversification and concentration risk.

**Implementation:**
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
  
  calculateCorrelation(series1, series2) {
    if (series1.length !== series2.length) {
      throw new Error('Series must have equal length');
    }
    
    const mean1 = this.calculateMean(series1);
    const mean2 = this.calculateMean(series2);
    
    let numerator = 0;
    let sumSq1 = 0;
    let sumSq2 = 0;
    
    for (let i = 0; i < series1.length; i++) {
      const diff1 = series1[i] - mean1;
      const diff2 = series2[i] - mean2;
      
      numerator += diff1 * diff2;
      sumSq1 += diff1 * diff1;
      sumSq2 += diff2 * diff2;
    }
    
    return numerator / Math.sqrt(sumSq1 * sumSq2);
  }
  
  calculateDiversificationRatio(weights, correlationMatrix, volatilities) {
    // Diversification ratio = weighted avg volatility / portfolio volatility
    const weightedAvgVol = weights.reduce((sum, weight, i) => 
      sum + weight * volatilities[i], 0);
    
    const portfolioVolatility = this.calculatePortfolioVolatility(
      weights, correlationMatrix, volatilities
    );
    
    return weightedAvgVol / portfolioVolatility;
  }
  
  calculateHerfindahlIndex(weights) {
    // Concentration measure: sum of squared weights
    return weights.reduce((sum, weight) => sum + weight * weight, 0);
  }
}
```

#### 4. **Position Sizing Algorithms**
Optimal position sizing based on various methodologies.

**Implementation:**
```javascript
class PositionSizer {
  // Kelly Criterion optimal sizing
  calculateKellySize(winRate, avgWin, avgLoss, totalCapital) {
    const b = Math.abs(avgWin / avgLoss); // Win/loss ratio
    const p = winRate; // Win probability
    const q = 1 - winRate; // Loss probability
    
    const kellyPercent = (b * p - q) / b;
    
    // Apply Kelly fraction with safety margin
    const safeKelly = Math.max(0, Math.min(kellyPercent * 0.25, 0.20));
    
    return {
      kellyPercent: kellyPercent,
      safeKellyPercent: safeKelly,
      positionSize: totalCapital * safeKelly,
      rationale: `Kelly: ${(kellyPercent * 100).toFixed(2)}%, Safe: ${(safeKelly * 100).toFixed(2)}%`
    };
  }
  
  // Volatility-based position sizing
  calculateVolatilitySize(targetVolatility, assetVolatility, totalCapital, confidence) {
    const volatilityRatio = targetVolatility / assetVolatility;
    const baseSize = totalCapital * volatilityRatio;
    
    // Adjust for signal confidence
    const confidenceAdjusted = baseSize * confidence;
    
    return {
      baseSize: baseSize,
      adjustedSize: confidenceAdjusted,
      volatilityRatio: volatilityRatio,
      targetVol: targetVolatility,
      assetVol: assetVolatility
    };
  }
  
  // Risk parity position sizing
  calculateRiskParitySize(portfolio, newAsset, targetRiskContribution = 0.20) {
    const currentRiskContributions = this.calculateRiskContributions(portfolio);
    const availableRiskBudget = 1.0 - currentRiskContributions.total;
    
    const maxRiskContribution = Math.min(targetRiskContribution, availableRiskBudget);
    const positionSize = this.calculateSizeForRiskContribution(
      newAsset, maxRiskContribution, portfolio
    );
    
    return {
      positionSize: positionSize,
      riskContribution: maxRiskContribution,
      availableRiskBudget: availableRiskBudget
    };
  }
  
  // Fixed fractional position sizing
  calculateFixedFractionalSize(totalCapital, riskPercent, stopLossPercent) {
    const riskAmount = totalCapital * riskPercent;
    const positionSize = riskAmount / stopLossPercent;
    
    return {
      positionSize: positionSize,
      riskAmount: riskAmount,
      riskPercent: riskPercent,
      stopLossPercent: stopLossPercent
    };
  }
}
```

---

## 🚨 Risk Monitoring & Alerts

### Real-time Risk Monitoring
```javascript
class RiskMonitor {
  constructor(config) {
    this.config = config;
    this.alerts = new Map();
    this.riskLimits = config.riskLimits;
    this.monitoringInterval = config.monitoringInterval || 30000; // 30 seconds
  }
  
  startMonitoring() {
    this.monitoringTimer = setInterval(async () => {
      try {
        await this.performRiskChecks();
      } catch (error) {
        console.error('Risk monitoring error:', error);
      }
    }, this.monitoringInterval);
  }
  
  async performRiskChecks() {
    const portfolio = await this.getPortfolioData();
    const riskMetrics = await this.calculateRiskMetrics(portfolio);
    
    // Check various risk thresholds
    this.checkPositionSizeLimits(portfolio);
    this.checkPortfolioVaR(riskMetrics.var);
    this.checkCorrelationLimits(riskMetrics.correlations);
    this.checkVolatilitySpikes(portfolio);
    this.checkDrawdownLimits(portfolio);
    this.checkConcentrationRisk(portfolio);
    
    // Process and escalate alerts
    this.processAlerts();
  }
  
  checkPositionSizeLimits(portfolio) {
    portfolio.positions.forEach(position => {
      const positionPercent = position.marketValue / portfolio.totalValue;
      
      if (positionPercent > this.riskLimits.maxPositionSize) {
        this.createAlert({
          type: 'POSITION_SIZE',
          level: 'HIGH',
          pair: position.pair,
          message: `Position exceeds ${(this.riskLimits.maxPositionSize * 100).toFixed(1)}% portfolio limit`,
          currentValue: positionPercent,
          threshold: this.riskLimits.maxPositionSize,
          recommendation: `Reduce position by ${this.formatCurrency((positionPercent - this.riskLimits.maxPositionSize) * portfolio.totalValue)}`
        });
      }
    });
  }
  
  checkPortfolioVaR(varMetrics) {
    const dailyVaR = Math.abs(varMetrics.daily_95);
    const varLimit = this.riskLimits.maxDailyVaR;
    
    if (dailyVaR > varLimit) {
      this.createAlert({
        type: 'VAR_BREACH',
        level: 'HIGH',
        message: `Portfolio VaR exceeds daily limit`,
        currentValue: dailyVaR,
        threshold: varLimit,
        recommendation: 'Reduce overall portfolio exposure or hedge positions'
      });
    }
  }
  
  checkCorrelationLimits(correlations) {
    const maxCorrelation = Math.max(...Object.values(correlations.correlationMatrix).flat());
    
    if (maxCorrelation > this.riskLimits.maxCorrelation) {
      this.createAlert({
        type: 'CORRELATION',
        level: 'MEDIUM',
        message: `High correlation detected between major positions`,
        currentValue: maxCorrelation,
        threshold: this.riskLimits.maxCorrelation,
        recommendation: 'Consider diversifying into uncorrelated assets'
      });
    }
  }
  
  checkVolatilitySpikes(portfolio) {
    portfolio.positions.forEach(position => {
      const currentVol = position.risk.volatility;
      const avgVol = position.risk.averageVolatility;
      const volSpike = currentVol / avgVol;
      
      if (volSpike > this.riskLimits.volatilitySpike) {
        this.createAlert({
          type: 'VOLATILITY',
          level: 'MEDIUM',
          pair: position.pair,
          message: `Volatility spike detected`,
          currentValue: currentVol,
          threshold: avgVol * this.riskLimits.volatilitySpike,
          recommendation: 'Monitor position closely, consider reducing size'
        });
      }
    });
  }
  
  checkDrawdownLimits(portfolio) {
    const currentDrawdown = portfolio.drawdown.current;
    
    if (currentDrawdown > this.riskLimits.maxDrawdown) {
      this.createAlert({
        type: 'DRAWDOWN',
        level: 'HIGH',
        message: `Portfolio drawdown exceeds maximum limit`,
        currentValue: currentDrawdown,
        threshold: this.riskLimits.maxDrawdown,
        recommendation: 'Consider defensive measures or position reduction'
      });
    }
  }
  
  createAlert(alertData) {
    const alertId = `ALERT_${Date.now()}_${Math.random().toString(36).substr(2, 9)}`;
    const alert = {
      id: alertId,
      ...alertData,
      createdAt: Date.now(),
      acknowledged: false
    };
    
    this.alerts.set(alertId, alert);
    this.notifyAlert(alert);
  }
  
  async notifyAlert(alert) {
    // Send to dashboard, execution service, etc.
    console.log(`Risk Alert [${alert.level}]: ${alert.message}`);
    
    // Could implement email, SMS, webhook notifications
    if (alert.level === 'HIGH') {
      await this.sendHighPriorityNotification(alert);
    }
  }
}
```

### Risk Alert System
```javascript
class RiskAlertManager {
  constructor() {
    this.alertHistory = [];
    this.subscribers = new Set();
    this.escalationRules = new Map();
  }
  
  subscribeToAlerts(callback) {
    this.subscribers.add(callback);
    return () => this.subscribers.delete(callback);
  }
  
  defineEscalationRule(alertType, rule) {
    this.escalationRules.set(alertType, rule);
  }
  
  processAlert(alert) {
    // Log alert
    this.alertHistory.push(alert);
    
    // Check for escalation
    this.checkEscalation(alert);
    
    // Notify subscribers
    this.notifySubscribers(alert);
    
    // Auto-acknowledge low priority alerts after time
    if (alert.level === 'LOW') {
      setTimeout(() => {
        this.acknowledgeAlert(alert.id, 'auto');
      }, 300000); // 5 minutes
    }
  }
  
  checkEscalation(alert) {
    const rule = this.escalationRules.get(alert.type);
    if (!rule) return;
    
    const recentAlerts = this.getRecentAlertsOfType(alert.type, rule.timeWindow);
    
    if (recentAlerts.length >= rule.threshold) {
      this.escalateAlert(alert, recentAlerts);
    }
  }
  
  escalateAlert(alert, relatedAlerts) {
    const escalatedAlert = {
      ...alert,
      id: `ESC_${alert.id}`,
      level: 'CRITICAL',
      message: `ESCALATED: ${alert.message} (${relatedAlerts.length} occurrences)`,
      escalated: true,
      originalAlert: alert.id,
      relatedAlerts: relatedAlerts.map(a => a.id)
    };
    
    this.processAlert(escalatedAlert);
  }
  
  acknowledgeAlert(alertId, acknowledgedBy = 'user') {
    const alert = this.findAlert(alertId);
    if (alert) {
      alert.acknowledged = true;
      alert.acknowledgedAt = Date.now();
      alert.acknowledgedBy = acknowledgedBy;
      
      this.notifySubscribers({
        type: 'alert_acknowledged',
        alert: alert
      });
    }
  }
}
```

---

## 🏗️ Integration Guide for Other Modules

### For Execution Service (trading-bot-execution)

#### Pre-trade Risk Checks
```javascript
class ExecutionRiskIntegration {
  constructor(riskServiceUrl = 'http://localhost:3003') {
    this.riskService = axios.create({ baseURL: riskServiceUrl });
  }
  
  async validateTradeRisk(tradeRequest) {
    try {
      // Get position sizing recommendation
      const positionSizeResponse = await this.riskService.post('/api/risk/position-size', {
        pair: tradeRequest.pair,
        signal: tradeRequest.signal,
        confidence: tradeRequest.confidence,
        entryPrice: tradeRequest.price,
        stopLoss: tradeRequest.stopLoss,
        takeProfit: tradeRequest.takeProfit
      });
      
      const { recommended } = positionSizeResponse.data.recommendations;
      
      // Check if requested size exceeds risk limits
      if (tradeRequest.size > recommended.positionSize) {
        return {
          approved: false,
          reason: 'Position size exceeds risk management recommendation',
          recommendedSize: recommended.positionSize,
          requestedSize: tradeRequest.size,
          maxRisk: recommended.maxRisk
        };
      }
      
      // Check portfolio impact
      const portfolioResponse = await this.riskService.get('/api/risk/portfolio');
      const portfolioRisk = portfolioResponse.data;
      
      if (portfolioRisk.riskScores.overall === 'HIGH') {
        return {
          approved: false,
          reason: 'Portfolio risk already at maximum acceptable level',
          currentRisk: portfolioRisk.riskScores.overall,
          recommendation: 'Reduce existing positions before adding new trades'
        };
      }
      
      return {
        approved: true,
        recommendedSize: recommended.positionSize,
        riskAssessment: recommended,
        portfolioImpact: portfolioResponse.data.riskAnalysis?.portfolioImpact
      };
      
    } catch (error) {
      console.error('Risk validation failed:', error);
      return {
        approved: false,
        reason: 'Risk service unavailable - defaulting to conservative sizing',
        fallbackSize: tradeRequest.size * 0.5 // Conservative fallback
      };
    }
  }
  
  async monitorPositionRisk(positionId) {
    try {
      const positionsResponse = await this.riskService.get('/api/risk/positions');
      const position = positionsResponse.data.positions.find(p => p.id === positionId);
      
      if (!position) return null;
      
      return {
        currentRisk: position.riskScore,
        var: position.risk.positionVaR,
        recommendations: position.recommendations,
        shouldClose: position.riskScore === 'HIGH' && position.recommendations.includes('close'),
        shouldReduce: position.recommendations.some(r => r.includes('reduce'))
      };
      
    } catch (error) {
      console.error('Position risk monitoring failed:', error);
      return null;
    }
  }
}

// Usage in execution service
const riskIntegration = new ExecutionRiskIntegration();

async function executeTradeWithRiskCheck(tradeRequest) {
  // Pre-trade risk validation
  const riskValidation = await riskIntegration.validateTradeRisk(tradeRequest);
  
  if (!riskValidation.approved) {
    throw new Error(`Trade rejected by risk management: ${riskValidation.reason}`);
  }
  
  // Use risk-adjusted size
  const adjustedTradeRequest = {
    ...tradeRequest,
    size: riskValidation.recommendedSize,
    riskMetadata: riskValidation.riskAssessment
  };
  
  // Execute trade
  const executionResult = await executeTrade(adjustedTradeRequest);
  
  // Start position monitoring
  if (executionResult.success) {
    setInterval(async () => {
      const riskStatus = await riskIntegration.monitorPositionRisk(executionResult.positionId);
      if (riskStatus?.shouldClose) {
        await closePosition(executionResult.positionId, 'RISK_MANAGEMENT');
      }
    }, 60000); // Check every minute
  }
  
  return executionResult;
}
```

### For Dashboard Service (trading-bot-dashboard)

#### Risk Dashboard Integration
```javascript
class RiskDashboardIntegration {
  async getRiskDashboardData() {
    const riskService = axios.create({ baseURL: 'http://localhost:3003' });
    
    try {
      const [portfolio, positions, alerts, correlations] = await Promise.allSettled([
        riskService.get('/api/risk/portfolio'),
        riskService.get('/api/risk/positions'),
        riskService.get('/api/risk/alerts'),
        riskService.get('/api/risk/correlations')
      ]);
      
      return {
        portfolio: portfolio.status === 'fulfilled' ? portfolio.value.data : null,
        positions: positions.status === 'fulfilled' ? positions.value.data : null,
        alerts: alerts.status === 'fulfilled' ? alerts.value.data : null,
        correlations: correlations.status === 'fulfilled' ? correlations.value.data : null,
        timestamp: Date.now()
      };
    } catch (error) {
      console.error('Risk dashboard integration error:', error);
      return {
        error: error.message,
        portfolio: null,
        positions: null,
        alerts: null,
        correlations: null
      };
    }
  }
  
  formatRiskDataForDisplay(riskData) {
    if (!riskData.portfolio) return null;
    
    const { portfolio, positions, alerts } = riskData;
    
    return {
      overview: {
        totalValue: this.formatCurrency(portfolio.portfolio.totalValue),
        totalRisk: portfolio.riskScores.overall,
        dailyVaR: this.formatCurrency(Math.abs(portfolio.riskMetrics.portfolioVaR.daily_95)),
        alerts: alerts?.summary || { total: 0 }
      },
      riskBreakdown: {
        concentration: portfolio.riskScores.concentration,
        correlation: portfolio.riskScores.correlation,
        volatility: portfolio.riskScores.volatility,
        liquidity: portfolio.riskScores.liquidity
      },
      topRisks: positions?.positions
        ?.sort((a, b) => this.getRiskScore(b.riskScore) - this.getRiskScore(a.riskScore))
        ?.slice(0, 3)
        ?.map(pos => ({
          pair: pos.pair,
          riskScore: pos.riskScore,
          exposure: this.formatCurrency(pos.marketValue),
          var: this.formatCurrency(Math.abs(pos.risk.positionVaR.daily_95))
        })) || [],
      activeAlerts: alerts?.activeAlerts?.filter(alert => !alert.acknowledged) || []
    };
  }
  
  getRiskScore(score) {
    const scores = { 'LOW': 1, 'MEDIUM': 2, 'HIGH': 3, 'CRITICAL': 4 };
    return scores[score] || 0;
  }
  
  formatCurrency(amount) {
    return new Intl.NumberFormat('en-US', {
      style: 'currency',
      currency: 'USD'
    }).format(amount);
  }
}

// React component for risk dashboard
const RiskDashboard = () => {
  const [riskData, setRiskData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  const riskIntegration = new RiskDashboardIntegration();
  
  useEffect(() => {
    const fetchRiskData = async () => {
      try {
        const data = await riskIntegration.getRiskDashboardData();
        const formattedData = riskIntegration.formatRiskDataForDisplay(data);
        setRiskData(formattedData);
        setError(null);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchRiskData();
    const interval = setInterval(fetchRiskData, 30000); // Update every 30 seconds
    
    return () => clearInterval(interval);
  }, []);
  
  if (loading) return <div className="card"><h3>⚡ Risk Management</h3><p>Loading risk data...</p></div>;
  if (error) return <div className="card"><h3>⚡ Risk Management</h3><p>Error: {error}</p></div>;
  if (!riskData) return <div className="card"><h3>⚡ Risk Management</h3><p>No risk data available</p></div>;
  
  return (
    <div className="card">
      <h3>⚡ Risk Management</h3>
      
      {/* Risk Overview */}
      <div className="grid" style={{ gridTemplateColumns: 'repeat(4, 1fr)', gap: '10px', marginBottom: '20px' }}>
        <div style={{ textAlign: 'center', padding: '10px', background: 'rgba(0, 212, 170, 0.1)', borderRadius: '6px' }}>
          <div style={{ color: '#00d4aa', fontWeight: 'bold', fontSize: '1.2rem' }}>
            {riskData.overview.totalValue}
          </div>
          <div style={{ fontSize: '0.8rem', opacity: 0.8 }}>Portfolio Value</div>
        </div>
        
        <div style={{ 
          textAlign: 'center', 
          padding: '10px', 
          background: riskData.overview.totalRisk === 'HIGH' ? 'rgba(255, 71, 87, 0.1)' : 'rgba(255, 165, 2, 0.1)', 
          borderRadius: '6px' 
        }}>
          <div style={{ 
            color: riskData.overview.totalRisk === 'HIGH' ? '#ff4757' : '#ffa502',
            fontWeight: 'bold', 
            fontSize: '1.2rem' 
          }}>
            {riskData.overview.totalRisk}
          </div>
          <div style={{ fontSize: '0.8rem', opacity: 0.8 }}>Risk Level</div>
        </div>
        
        <div style={{ textAlign: 'center', padding: '10px', background: 'rgba(255, 255, 255, 0.05)', borderRadius: '6px' }}>
          <div style={{ color: '#ff4757', fontWeight: 'bold', fontSize: '1.2rem' }}>
            {riskData.overview.dailyVaR}
          </div>
          <div style={{ fontSize: '0.8rem', opacity: 0.8 }}>Daily VaR</div>
        </div>
        
        <div style={{ textAlign: 'center', padding: '10px', background: 'rgba(255, 255, 255, 0.05)', borderRadius: '6px' }}>
          <div style={{ color: '#ffa502', fontWeight: 'bold', fontSize: '1.2rem' }}>
            {riskData.overview.alerts.total}
          </div>
          <div style={{ fontSize: '0.8rem', opacity: 0.8 }}>Active Alerts</div>
        </div>
      </div>
      
      {/* Top Risk Positions */}
      {riskData.topRisks.length > 0 && (
        <div style={{ marginBottom: '20px' }}>
          <h4 style={{ marginBottom: '10px' }}>Highest Risk Positions</h4>
          {riskData.topRisks.map((position, index) => (
            <div key={index} style={{
              display: 'flex',
              justifyContent: 'space-between',
              alignItems: 'center',
              padding: '8px',
              margin: '4px 0',
              background: 'rgba(255, 255, 255, 0.03)',
              borderRadius: '6px',
              border: '1px solid rgba(255, 255, 255, 0.1)'
            }}>
              <div>
                <strong>{position.pair}</strong>
                <div style={{ fontSize: '0.8rem', opacity: 0.7 }}>
                  {position.exposure}
                </div>
              </div>
              <div style={{ textAlign: 'center' }}>
                <div style={{
                  color: position.riskScore === 'HIGH' ? '#ff4757' : 
                        position.riskScore === 'MEDIUM' ? '#ffa502' : '#00d4aa',
                  fontWeight: 'bold'
                }}>
                  {position.riskScore}
                </div>
                <div style={{ fontSize: '0.8rem', opacity: 0.7 }}>
                  VaR: {position.var}
                </div>
              </div>
            </div>
          ))}
        </div>
      )}
      
      {/* Active Alerts */}
      {riskData.activeAlerts.length > 0 && (
        <div>
          <h4 style={{ marginBottom: '10px' }}>Active Risk Alerts</h4>
          {riskData.activeAlerts.slice(0, 3).map((alert, index) => (
            <div key={index} style={{
              padding: '10px',
              margin: '5px 0',
              background: alert.level === 'HIGH' ? 'rgba(255, 71, 87, 0.1)' : 'rgba(255, 165, 2, 0.1)',
              borderRadius: '6px',
              border: `1px solid ${alert.level === 'HIGH' ? '#ff4757' : '#ffa502'}`
            }}>
              <div style={{ 
                color: alert.level === 'HIGH' ? '#ff4757' : '#ffa502',
                fontWeight: 'bold',
                fontSize: '0.9rem'
              }}>
                {alert.level}: {alert.message}
              </div>
              <div style={{ fontSize: '0.8rem', opacity: 0.8, marginTop: '4px' }}>
                {alert.recommendation}
              </div>
            </div>
          ))}
        </div>
      )}
    </div>
  );
};
```

### For Core Service (trading-bot-core)

#### Risk-Adjusted Signal Enhancement
```javascript
class RiskAdjustedSignals {
  constructor(riskServiceUrl = 'http://localhost:3003') {
    this.riskService = axios.create({ baseURL: riskServiceUrl });
  }
  
  async enhanceSignalsWithRisk(technicalSignals, pair) {
    try {
      // Get current portfolio risk
      const portfolioResponse = await this.riskService.get('/api/risk/portfolio');
      const portfolio = portfolioResponse.data;
      
      // Get position-specific risk if position exists
      const positionsResponse = await this.riskService.get('/api/risk/positions');
      const existingPosition = positionsResponse.data.positions.find(p => p.pair === pair);
      
      // Enhance signals with risk considerations
      const enhancedSignals = this.applyRiskAdjustments(
        technicalSignals, 
        portfolio, 
        existingPosition
      );
      
      return enhancedSignals;
      
    } catch (error) {
      console.warn('Risk service unavailable, using original signals:', error.message);
      return technicalSignals;
    }
  }
  
  applyRiskAdjustments(signals, portfolio, existingPosition) {
    const riskAdjustments = {
      portfolioRisk: this.getPortfolioRiskAdjustment(portfolio),
      positionRisk: this.getPositionRiskAdjustment(existingPosition),
      correlationRisk: this.getCorrelationRiskAdjustment(portfolio)
    };
    
    // Adjust signal confidence based on risk factors
    Object.entries(signals).forEach(([indicator, signal]) => {
      if (signal.suggestion === 'buy' || signal.suggestion === 'sell') {
        // Reduce confidence if portfolio risk is high
        if (riskAdjustments.portfolioRisk < 1.0) {
          signal.confidence *= riskAdjustments.portfolioRisk;
          signal.riskAdjustment = 'Portfolio risk high - confidence reduced';
        }
        
        // Adjust for existing position risk
        if (existingPosition && riskAdjustments.positionRisk < 1.0) {
          signal.confidence *= riskAdjustments.positionRisk;
          signal.riskAdjustment = 'Position risk high - confidence reduced';
        }
        
        // Adjust for correlation risk
        if (riskAdjustments.correlationRisk < 1.0) {
          signal.confidence *= riskAdjustments.correlationRisk;
          signal.riskAdjustment = 'High correlation risk - confidence reduced';
        }
        
        // Override signals if risk is too high
        if (portfolio.riskScores.overall === 'HIGH' && signal.suggestion === 'buy') {
          signal.suggestion = 'hold';
          signal.confidence = 0.1;
          signal.riskOverride = 'Buy signal overridden due to high portfolio risk';
        }
      }
    });
    
    return signals;
  }
  
  getPortfolioRiskAdjustment(portfolio) {
    switch (portfolio.riskScores.overall) {
      case 'LOW': return 1.0;
      case 'MEDIUM': return 0.8;
      case 'HIGH': return 0.5;
      default: return 1.0;
    }
  }
  
  getPositionRiskAdjustment(position) {
    if (!position) return 1.0;
    
    switch (position.riskScore) {
      case 'LOW': return 1.0;
      case 'MEDIUM': return 0.7;
      case 'HIGH': return 0.3;
      default: return 1.0;
    }
  }
  
  getCorrelationRiskAdjustment(portfolio) {
    const avgCorrelation = portfolio.riskMetrics.correlations.averageCorrelation;
    
    if (avgCorrelation > 0.8) return 0.6;
    if (avgCorrelation > 0.7) return 0.8;
    return 1.0;
  }
}

// Integration in core service signal generation
const riskAdjustedSignals = new RiskAdjustedSignals();

async function generateEnhancedSignals(pair, marketData) {
  // Generate technical signals
  const technicalSignals = await generateTechnicalSignals(pair, marketData);
  
  // Enhance with risk considerations
  const enhancedSignals = await riskAdjustedSignals.enhanceSignalsWithRisk(
    technicalSignals, 
    pair
  );
  
  return enhancedSignals;
}
```

### For ML Service (trading-bot-ml)

#### Risk-Informed Feature Engineering
```javascript
class RiskFeatureEngineer {
  constructor(riskServiceUrl = 'http://localhost:3003') {
    this.riskService = axios.create({ baseURL: riskServiceUrl });
  }
  
  async extractRiskFeatures(pair, baseFeatures) {
    try {
      const [portfolio, correlations, positions] = await Promise.all([
        this.riskService.get('/api/risk/portfolio'),
        this.riskService.get('/api/risk/correlations'),
        this.riskService.get('/api/risk/positions')
      ]);
      
      const riskFeatures = this.generateRiskFeatures(
        pair,
        portfolio.data,
        correlations.data,
        positions.data
      );
      
      return {
        ...baseFeatures,
        ...riskFeatures
      };
      
    } catch (error) {
      console.warn('Risk features unavailable:', error.message);
      return baseFeatures;
    }
  }
  
  generateRiskFeatures(pair, portfolio, correlations, positions) {
    const currentPosition = positions.positions.find(p => p.pair === pair);
    
    return {
      // Portfolio-level risk features
      portfolio_var_normalized: Math.abs(portfolio.riskMetrics.portfolioVaR.daily_95) / portfolio.portfolio.totalValue,
      portfolio_utilization: portfolio.portfolio.utilizationRate,
      portfolio_diversification: correlations.diversificationMetrics.diversificationRatio,
      portfolio_concentration: correlations.diversificationMetrics.concentrationIndex,
      
      // Position-level risk features
      position_exists: currentPosition ? 1 : 0,
      position_risk_score: this.encodeRiskScore(currentPosition?.riskScore || 'NONE'),
      position_var_ratio: currentPosition ? 
        Math.abs(currentPosition.risk.positionVaR.daily_95) / currentPosition.marketValue : 0,
      position_beta: currentPosition?.risk.beta || 1.0,
      
      // Correlation features
      avg_correlation: correlations.diversificationMetrics.averageCorrelation || 0,
      max_correlation: this.getMaxCorrelationForPair(pair, correlations.correlationMatrix),
      correlation_cluster_size: this.getCorrelationClusterSize(pair, correlations.correlationMatrix, 0.7),
      
      // Risk regime features
      market_stress_indicator: this.calculateMarketStressIndicator(portfolio),
      volatility_regime: this.classifyVolatilityRegime(portfolio.riskMetrics.volatility.daily),
      liquidity_indicator: this.encodeLiquidityScore(portfolio.riskScores.liquidity)
    };
  }
  
  encodeRiskScore(riskScore) {
    const scores = { 'NONE': 0, 'LOW': 0.25, 'MEDIUM': 0.5, 'HIGH': 0.75, 'CRITICAL': 1.0 };
    return scores[riskScore] || 0;
  }
  
  getMaxCorrelationForPair(pair, correlationMatrix) {
    const pairCorrelations = correlationMatrix[pair] || {};
    return Math.max(...Object.values(pairCorrelations), 0);
  }
  
  getCorrelationClusterSize(pair, correlationMatrix, threshold) {
    const pairCorrelations = correlationMatrix[pair] || {};
    return Object.values(pairCorrelations).filter(corr => Math.abs(corr) > threshold).length;
  }
  
  calculateMarketStressIndicator(portfolio) {
    // Composite stress indicator based on multiple risk metrics
    const stressFactors = [
      portfolio.riskMetrics.volatility.daily > 0.05 ? 1 : 0,
      Math.abs(portfolio.riskMetrics.portfolioVaR.daily_95) / portfolio.portfolio.totalValue > 0.02 ? 1 : 0,
      portfolio.riskMetrics.correlations.averageCorrelation > 0.8 ? 1 : 0,
      portfolio.riskMetrics.drawdown.current > 0.1 ? 1 : 0
    ];
    
    return stressFactors.reduce((sum, factor) => sum + factor, 0) / stressFactors.length;
  }
  
  classifyVolatilityRegime(dailyVolatility) {
    if (dailyVolatility < 0.02) return 0.25; // Low volatility
    if (dailyVolatility < 0.04) return 0.5;  // Medium volatility
    if (dailyVolatility < 0.06) return 0.75; // High volatility
    return 1.0; // Extreme volatility
  }
  
  encodeLiquidityScore(liquidityScore) {
    const scores = { 'LOW': 0.25, 'MEDIUM': 0.5, 'HIGH': 0.75, 'VERY_HIGH': 1.0 };
    return scores[liquidityScore] || 0.5;
  }
}

// Integration in ML feature pipeline
const riskFeatureEngineer = new RiskFeatureEngineer();

async function generateMLFeatures(pair, marketData)