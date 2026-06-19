# NSE F&O Trend Swing Trader

Automated NSE swing trading system using the Dual Trend signal on the F&O eligible stock universe (~209 stocks).

## Strategy
- **Signal:** 15-bar highest-high and lowest-low step lines — entry when both flip upward simultaneously (fresh confluence)
- **Exit:** Both lines flip bearish, OR 10% stop loss hit
- **Universe:** NSE F&O eligible stocks (~209 stocks)
- **Position size:** ₹10,000 per trade (paper trading)
- **Tracking:** Paper trades only

## Setup Instructions

### 1. Prerequisites
- DigitalOcean account with Functions enabled
- GitHub account
- Gmail account with App Password enabled
- `doctl` CLI installed and authenticated

### 2. Clone the repo
```bash
git clone https://github.com/finnyrajmann/nse-fno-trend.git
cd nse-fno-trend
```

### 3. Folder structure
nse-fno-trend/

├── data/

│   ├── watchlist.csv        # F&O stock universe

│   ├── positions_fno.csv    # Open paper positions

│   └── fno_trade_log.csv    # Closed trade history

├── functions/

│   ├── project.yml          # DO Functions config

│   └── packages/

│       └── nse_fno/

│           └── daily_run/

│               └── main.py

└── README.md

### 4. Environment variables (set in project.yml)
| Variable | Description |
|---|---|
| GMAIL_SENDER | Gmail address to send from |
| GMAIL_APP_PASSWORD | Gmail App Password |
| GMAIL_RECIPIENT | Email to receive reports |
| GITHUB_PAT | GitHub Personal Access Token |
| GITHUB_REPO | GitHub repo (user/repo-name) |

### 5. Deploy to DigitalOcean
```bash
cd ~/nse-fno-trend
doctl serverless deploy functions --verbose
```

### 6. Verify trigger
```bash
doctl serverless triggers list
```

## Schedule
Runs at **08:55 IST** (03:25 UTC) on weekdays (Mon–Fri).
BB system runs at 08:45, Dual Trend at 08:55, F&O Trend at 09:00.

## Data flow
- Reads `watchlist.csv` and `positions_fno.csv` from GitHub
- Fetches OHLC data from Yahoo Finance
- Writes updated positions and trade log back to GitHub
- Sends HTML email report via Gmail SMTP

## Related systems
- [nse-bb-system](https://github.com/finnyrajmann/nse-bb-system) — Bollinger Band, Nifty 500
- [nse-dual_trend](https://github.com/finnyrajmann/nse-dual_trend) — Dual Trend, Nifty 500
