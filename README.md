# Tennis Elo Pricing Model

A surface-adjusted Elo rating system for ATP tennis match pricing, with backtesting against historical betting odds and Kelly criterion sizing application.

## Thesis

ATP tennis exhibits significant surface-specific skill variance — players have substantially different effective ratings on clay vs hard vs grass courts. Standard Elo systems ignoring surface information leave consistent pricing edge on the table, particularly during surface transition periods.

This project:
1. Implements baseline Elo (K=32) on ATP match history
2. Extends to surface-adjusted Elo with surface-specific K factors
3. Backtests model win probabilities against historical odds
4. Applies Kelly criterion sizing to identify profitable betting strategies

## Methodology

### Data
- **ATP Matches**: [Jeff Sackmann's open dataset](https://github.com/JeffSackmann/tennis_atp) (2000-2024)
- **Historical Odds**: tennis-data.co.uk (closing odds, major books)
- **Surface labels**: Court type per match (Clay/Hard/Grass)

### Models
1. **Baseline Elo**: Standard rating system, single K=32 factor
2. **Surface-Adjusted Elo**: Three separate Elo systems (Clay/Hard/Grass) with surface-specific K factors tuned via cross-validation
3. **Hybrid Elo**: Weighted combination of overall and surface-specific ratings

### Validation
- Train: 2000-2022
- Validation: 2023
- Test: 2024 (out-of-sample)
- Metrics: log-loss, calibration, AUC-ROC

### Trading Application
- Convert win probability to implied odds
- Compare to market odds (after vig adjustment)
- Apply Kelly criterion for position sizing
- Backtest PnL with realistic execution assumptions

## Project Structure
tennis-elo-pricing/
├── README.md
├── requirements.txt
├── LICENSE
├── notebooks/
│   ├── 01_data_exploration.ipynb
│   ├── 02_basic_elo.ipynb
│   ├── 03_surface_adjusted_elo.ipynb
│   ├── 04_backtest_and_calibration.ipynb
│   └── 05_kelly_betting_strategy.ipynb
└── results/
└── plots/
## Status

🚧 In active development (started May 2026)

## Tech Stack
- Python 3.10+
- pandas, NumPy, scipy
- matplotlib, seaborn
- scikit-learn

## Author

**Taosheng (Andy) Yin**
- UCLA Anderson MFE (December 2025)
- Penn State undergraduate
- Focus: Quantitative finance, prediction markets research

## License

MIT License - see [LICENSE](LICENSE) file
