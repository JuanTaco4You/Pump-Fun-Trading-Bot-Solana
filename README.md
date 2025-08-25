## Pump.fun Solana Trading and Sniping Bot
![Screenshot](bot.png)

Welcome to the Pump.fun Solana Trading Bot! This tool, developed by TreeCityWes.eth of HashHead.io, is designed for trading and sniping new token launches on pump.fun. It includes strategies for buying and selling tokens based on market cap changes and bonding curve progress.

## Overview

The Solana Trading Bot helps you trade tokens on the Pump.fun on Solana blockchain based on bonding curves and market cap changes. The bot scrapes data to identify new tokens with favorable bonding curves, monitors market cap changes, and makes decisions on when to sell to maximize profits.

## Trading Strategy

- **Initial Buy**: The bot scrapes pump.fun to identify new token pairs with favorable bonding curves.
- **Monitoring**: Once a token is bought, the bot monitors the market cap and bonding curve progress.
- **Profit Targets**: 
  - The bot aims to take profit at a 25% increase and then again at another 25% increase.
  - It sells 50% of the tokens at the first 25% increase and 75% of the remaining tokens at the next 25% increase.
- **Stop Loss**: The bot will sell all tokens if the market cap falls by 10%.
- **Bonding Curve**: The bot will sell 75% of the tokens if the bonding curve reaches a critical level and keep 25% as a moon bag.
- **Timing**: The bot resets the timer if the price goes up and monitors the trade for a set period, adjusting its actions based on market conditions.

## Requirements

- **Node.js**: JavaScript runtime built on Chrome's V8 JavaScript engine.
- **Solana CLI**: Command-line interface for interacting with the Solana blockchain.
- **Selenium WebDriver (Chrome)**: Automated web browser.

## Installation

1. **Clone the repository**:
   \`\`\`sh
   git clone [https://github.com/yourusername/solana-trading-bot.git](https://github.com/TreeCityWes/Pump-Fun-Trading-Bot-Solana.git)
   cd solana-trading-bot
   \`\`\`

2. **Install dependencies**:
   \`\`\`sh
   npm install dotenv axios @solana/web3.js @solana/spl-token selenium-webdriver fs bs58 blessed blessed-contrib
   \`\`\`

3. **Set up your environment variables**:
   Create a .env file in the root directory and add the following:
  
   ```env
   SOLANA_WALLET_PATH=/path/to/your/solana/wallet.json
   MINIMUM_BUY_AMOUNT=0.015
   MAX_BONDING_CURVE_PROGRESS=10
   SELL_BONDING_CURVE_PROGRESS=15
  

4. **Configure Solana CLI**:
   \`\`\`sh
   solana config set --url https://api.mainnet-beta.solana.com
   solana config set --keypair /path/to/your/solana/wallet.json
   \`\`\`

## Usage

- **Run the trading bot**:
  \`\`\`sh
  node script.mjs
  \`\`\`

- **Sell all SPL tokens**:
  \`\`\`sh
  node sell.js
  \`\`\`

## Donations

If you find this tool useful, please consider supporting the developer. Donations can be sent to:

- Solana Address: 8bXf8Rg3u4Prz71LgKR5mpa7aMe2F4cSKYYRctmqro6x
- Buy Me a Coffee: [buymeacoffee.com/treecitywes](https://buymeacoffee.com/treecitywes)

## About

This tool is developed by TreeCityWes.eth of HashHead.io. It is designed to help traders effectively manage their trades on the Solana blockchain, especially for new token launches on pump.fun. The bot keeps moon bags and times trades efficiently to maximize profits and minimize losses.

## Disclaimer

Use this tool at your own risk. Trading cryptocurrencies involves significant risk and can result in the loss of your capital. Always do your own research before making any trading decisions.

Thank you for using the Solana Trading Bot! Happy trading!

TreeCityWes.eth of HashHead.io




# Pump.fun Solana Trading and Sniping Bot

Trade and snipe new token launches on pump.fun. Strategy uses market-cap changes and bonding-curve progress.

## Requirements

- Node.js
- Solana CLI
- Selenium WebDriver with Google Chrome

## Quick start (Ubuntu WSL)

```bash
set -euo pipefail

# 0) Base packages
sudo apt-get update
sudo apt-get install -y git curl wget unzip ca-certificates gnupg xz-utils build-essential

# 1) Solana CLI (Anza installer) + PATH
sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc
export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"
solana --version

# 2) Clone
cd ~
git clone https://github.com/TreeCityWes/Pump-Fun-Trading-Bot-Solana.git
cd Pump-Fun-Trading-Bot-Solana

# 3) Wallet
[ -f wallet.json ] || solana-keygen new --outfile wallet.json --no-passphrase
chmod 600 wallet.json

# 4) Point Solana CLI to mainnet and this wallet
solana config set -um
solana config set --keypair "$PWD/wallet.json"
solana config get

# 5) Node.js via nvm if npm missing
if ! command -v npm >/dev/null 2>&1; then
  export NVM_DIR="$HOME/.nvm"
  curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
  . "$NVM_DIR/nvm.sh"
  nvm install --lts
  nvm use --lts
fi
node -v
npm -v

# 6) Google Chrome + matching ChromeDriver (WSL headless ok)
sudo mkdir -p /etc/apt/keyrings
wget -qO- https://dl.google.com/linux/linux_signing_key.pub | sudo gpg --dearmor -o /etc/apt/keyrings/google.gpg
echo 'deb [arch=amd64 signed-by=/etc/apt/keyrings/google.gpg] http://dl.google.com/linux/chrome/deb/ stable main' | \
  sudo tee /etc/apt/sources.list.d/google-chrome.list >/dev/null
sudo apt-get update
sudo apt-get install -y google-chrome-stable
CHROME_MAJOR=$(google-chrome --version | awk '{print $3}' | cut -d. -f1)
DRV_VER=$(curl -fsSL "https://chromedriver.storage.googleapis.com/LATEST_RELEASE_${CHROME_MAJOR}")
wget -q "https://chromedriver.storage.googleapis.com/${DRV_VER}/chromedriver_linux64.zip" -O /tmp/chromedriver.zip
unzip -o /tmp/chromedriver.zip -d /tmp
sudo install -m 0755 /tmp/chromedriver /usr/local/bin/chromedriver
chromedriver --version

# 7) Install Node deps (per upstream README)
npm install dotenv axios @solana/web3.js @solana/spl-token selenium-webdriver fs bs58 blessed blessed-contrib

# 8) .env
cat > .env <<EOF
SOLANA_WALLET_PATH="$(pwd)/wallet.json"
MINIMUM_BUY_AMOUNT=0.015
MAX_BONDING_CURVE_PROGRESS=10
SELL_BONDING_CURVE_PROGRESS=15
EOF

echo
echo "Run bot:  node script.mjs"
echo "Sell all SPL:  node sell.js"

