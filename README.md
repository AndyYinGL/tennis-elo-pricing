# Tennis Elo Pricing Model

A quantitative research project applying Elo rating systems and Kelly criterion betting strategies to ATP tennis match prediction.

## Overview

This project develops a complete pipeline for pricing tennis matches using historical ATP data:

1. Baseline Elo rating system implementation
2. Surface-adjusted Elo (separate ratings per court type)
3. Hybrid blending approach with sample-size weighting
4. Probability calibration analysis
5. Kelly criterion betting strategy with sensitivity analysis

The pipeline analyzes 75,000+ ATP matches (2000-2024) and evaluates strategy performance through simulated market backtesting with realistic vig modeling.

## Key Results

- **Basic Elo**: 63.7% prediction accuracy on 2024 holdout (log-loss 0.6263)
- **Surface-adjusted Elo**: -1.4% improvement (counterintuitive negative result — per-surface data dilution outweighs surface-specificity benefit in this dataset)
- **Hybrid Elo** (sample-size weighted blending of overall and surface ratings): 63.9% accuracy, log-loss 0.6233 — best performing model
- **Calibration**: Well-calibrated probabilities (error <0.04 in trading-relevant 0.45-0.65 probability range)
- **Kelly criterion backtest**: +51% return on 1.9% selective bet rate, Sharpe ratio 2.22, max drawdown 16.57%

## Methodology

### Data

- **ATP Matches**: [Jeff Sackmann's open dataset](https://github.com/JeffSackmann/tennis_atp) (2000-2024)
- **Surface labels**: Court type per match (Clay/Hard/Grass)
- **Market odds**: Simulated using true probability with noise and overround vig model (no external odds data used)

### Models

1. **Baseline Elo**: Standard rating system with K=32, expected score formula `1 / (1 + 10^((R_opp - R_player) / 400))`
2. **Surface-Adjusted Elo**: Three separate Elo systems (Clay/Hard/Grass) maintaining surface-specific ratings
3. **Hybrid Elo**: Sample-size weighted blending — `w = n_surface / (n_surface + 20)`, combining surface-specific and overall ratings

### Validation

- Train: 2000-2023
- Test: 2024 (out-of-sample, ~3,200 matches)
- Metrics: prediction accuracy, log-loss, Brier score, calibration error

### Trading Application

- Convert hybrid model probability to fair odds
- Simulate market odds with noise and bilateral vig (overround applied to both implied probabilities)
- Identify edge as `model_prob - market_implied_prob`
- Apply fractional Kelly criterion (α=0.25 baseline) with edge thresholds
- Backtest PnL with threshold and Kelly fraction sensitivity analysis

## Notable Findings

### Surface-Adjusted Elo Underperformance

Initial hypothesis predicted surface-adjusted ratings would outperform basic Elo. Results showed the opposite: -1.4% accuracy drop. Analysis revealed that splitting the dataset by surface reduces per-surface sample sizes, increasing rating noise more than the surface-specificity gains. This motivated the hybrid blending approach.

### Vig Modeling Bug Discovery

During strategy backtest development, initial results showed -100% returns with only 2.9% win rate. Diagnostic analysis identified a subtle bug: the vig adjustment was applied only to the winner's implied probability, artificially inflating it and systematically biasing edge calculations toward the losing side. After correcting the vig application to both implied probabilities (proper bilateral overround model), the backtest produced realistic profitable returns. The Kelly fraction sensitivity analysis (α=0.10 → +20%, α=0.50 → +101%, α=1.00 → +165% with -55% drawdown) confirmed textbook risk/return scaling, validating the corrected implementation.

## Project Structure

```
tennis-elo-pricing/
├── README.md
├── requirements.txt
├── LICENSE
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_basic_elo_implementation.ipynb
│   ├── 03_surface_adjusted_elo.ipynb
│   ├── 04_backtest_and_calibration.ipynb
│   └── 05_kelly_betting_strategy.ipynb
└── results/
    └── plots/
```

## Setup

```bash
git clone https://github.com/AndyYinGL/tennis-elo-pricing.git
cd tennis-elo-pricing
pip install -r requirements.txt
```

## Tech Stack

- Python 3.10+
- pandas, NumPy
- matplotlib, seaborn
- Jupyter

## Author

**Taosheng (Andy) Yin**

- UCLA Anderson MFE (December 2025)
- Penn State undergraduate (Mathematics & Economics)
- Focus: Quantitative finance, prediction markets, sports analytics

## License

MIT License — see [LICENSE](LICENSE) file
