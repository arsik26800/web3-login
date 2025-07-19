# @buildship/web3-login (beta)

<img src="public/screenshot.png" width="500" />

This is a design-focused web3 wallet connecting modal for React based on [Material UI](https://github.com/mui/material-ui). 

It supports [Metamask](https://metamask.io/), [WalletConnect](https://walletconnect.com/), [Coinbase Wallet](https://walletlink.org/) and wallet-less email auth via [Magic](https://magic.link).

## Getting started

Install with yarn:

```bash
yarn add @buildship/web3-login
```
Install with npm:

```bash
npm i @buildship/web3-login
```

Use it in your code:

```javascript
import { Web3Provider, ConnectWeb3Modal, useWeb3 } from "@buildship/web3-login";

// Wallets that you want to support
const connectors = {
    // Metamask
    injected: {},
    magic: {
        apiKey: "pk_...", // Your Magic api key
        chainId: 1, // The chain ID you want to allow on Magic
    },
    walletconnect: {},
    // Coinbase
    walletlink: {
        appName: "Buildship Example",
        url: "https://buildship.dev", 
        darkMode: false,
    }
}

const App = () => {
    const { address } = useWeb3()
    const [isOpen, setIsOpen] = useState(false)
    
    return <Web3Provider
        supportedChainIds={[1, 4]}
        connectors={connectors}>
            Connected address: {address}    
            <button onClick={() => setIsOpen(true)}>
              Connect wallet
            </button>
            <ConnectWeb3Modal 
              open={isOpen} 
              setOpen={setIsOpen}
            /> 
    </Web3Provider>
}

```

## Theming
Follow Material UI [guide on theming](https://mui.com/customization/theming/), then pass your `theme` object like this:

```javascript
<Web3Provider
    theme={theme}
    connectors={connectors}> 
    // ...
</Web3Provider>
```

[Default theme example](https://github.com/buildship-dev/web3-login/blob/main/src/styles/theme.tsx)

## Plans
- [ ] Support hooks for backend auth
- [ ] Improve experience for Metamask users on mobile
- [ ] Fix WalletConnect mobile deeplink issues
- [ ] Vanilla JS (pure JS) support for in-browser games, etc.
- [ ] Native support for Ledger
- [ ] Support hooks for ENS

## Contributing & issues
Feel free to open a PR or an issue! Contact us at https://buildship.dev/ if you have additional questions

## Thanks
Huge thanks to [context.app](https://context.app) & [web3Modal](https://github.com/Web3Modal/web3modal) for inspiration, and to [web3-react](https://github.com/NoahZinsmeister/web3-react) and [Thirdweb](https://github.com/thirdweb-dev/ui) for making this easy.
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Empire Coin Clicker</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
        }
        
        .game-container {
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            border: 1px solid rgba(255, 255, 255, 0.2);
            max-width: 800px;
            width: 100%;
        }
        
        .header {
            text-align: center;
            margin-bottom: 30px;
        }
        
        .title {
            font-size: 2.5em;
            font-weight: bold;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
            margin-bottom: 10px;
        }
        
        .coin-display {
            font-size: 3em;
            font-weight: bold;
            color: #ffd700;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
            margin-bottom: 20px;
        }
        
        .main-area {
            display: flex;
            gap: 30px;
            align-items: flex-start;
        }
        
        .clicker-section {
            flex: 1;
            text-align: center;
        }
        
        .empire-coin {
            width: 200px;
            height: 200px;
            border-radius: 50%;
            background: linear-gradient(45deg, #ffd700, #ffed4e, #ffd700);
            border: 8px solid #b8860b;
            cursor: pointer;
            transition: all 0.1s ease;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            font-weight: bold;
            color: #8b4513;
            text-shadow: 1px 1px 2px rgba(0, 0, 0, 0.3);
            margin: 0 auto 20px;
            box-shadow: 0 10px 20px rgba(0, 0, 0, 0.3);
            position: relative;
            overflow: hidden;
        }
        
        .empire-coin:hover {
            transform: scale(1.05);
            box-shadow: 0 15px 30px rgba(0, 0, 0, 0.4);
        }
        
        .empire-coin:active {
            transform: scale(0.95);
        }
        
        .empire-coin::before {
            content: '';
            position: absolute;
            top: 20px;
            left: 20px;
            right: 60px;
            height: 40px;
            background: linear-gradient(45deg, rgba(255, 255, 255, 0.8), transparent);
            border-radius: 50%;
            transform: rotate(-20deg);
        }
        
        .click-power {
            font-size: 1.2em;
            margin-bottom: 10px;
            color: #ffed4e;
        }
        
        .upgrades-section {
            flex: 1;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 15px;
            padding: 20px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }
        
        .upgrades-title {
            font-size: 1.5em;
            margin-bottom: 20px;
            text-align: center;
            color: #ffd700;
        }
        
        .upgrade-item {
            background: rgba(255, 255, 255, 0.1);
            border-radius: 10px;
            padding: 15px;
            margin-bottom: 10px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .upgrade-item:hover {
            background: rgba(255, 255, 255, 0.2);
            transform: translateX(5px);
        }
        
        .upgrade-item.disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
        
        .upgrade-item.disabled:hover {
            background: rgba(255, 255, 255, 0.1);
            transform: none;
        }
        
        .upgrade-name {
            font-weight: bold;
            font-size: 1.1em;
            margin-bottom: 5px;
        }
        
        .upgrade-description {
            font-size: 0.9em;
            opacity: 0.8;
            margin-bottom: 8px;
        }
        
        .upgrade-cost {
            color: #ffd700;
            font-weight: bold;
        }
        
        .upgrade-owned {
            float: right;
            background: rgba(255, 215, 0, 0.2);
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 0.8em;
        }
        
        .stats {
            margin-top: 20px;
            text-align: center;
        }
        
        .stat-item {
            margin: 5px 0;
            font-size: 0.9em;
        }
        
        .floating-number {
            position: absolute;
            font-size: 2em;
            font-weight: bold;
            color: #ffd700;
            pointer-events: none;
            animation: floatUp 1s ease-out forwards;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.8);
        }
        
        @keyframes floatUp {
            0% {
                opacity: 1;
                transform: translateY(0);
            }
            100% {
                opacity: 0;
                transform: translateY(-100px);
            }
        }
        
        .passive-income {
            text-align: center;
            margin-top: 20px;
            padding: 15px;
            background: rgba(0, 255, 0, 0.1);
            border-radius: 10px;
            border: 1px solid rgba(0, 255, 0, 0.3);
        }
        
        @media (max-width: 768px) {
            .main-area {
                flex-direction: column;
            }
            
            .empire-coin {
                width: 150px;
                height: 150px;
                font-size: 1.5em;
            }
            
            .coin-display {
                font-size: 2em;
            }
        }
    </style>
</head>
<body>
    <div class="game-container">
        <div class="header">
            <div class="title">🏛️ EMPIRE COIN CLICKER 🏛️</div>
            <div class="coin-display" id="coinDisplay">0 EC</div>
        </div>
        
        <div class="main-area">
            <div class="clicker-section">
                <div class="empire-coin" id="empireCoin">
                    EMPIRE
                </div>
                <div class="click-power">
                    Сила клика: <span id="clickPower">1</span> EC
                </div>
                <div class="stats">
                    <div class="stat-item">Всего кликов: <span id="totalClicks">0</span></div>
                    <div class="stat-item">Всего заработано: <span id="totalEarned">0</span> EC</div>
                    <div class="stat-item" style="color: #90EE90; margin-top: 10px;">💾 Прогресс автоматически сохраняется</div>
                    <div class="stat-item" style="color: #87CEEB; margin-top: 5px;">🌙 Офлайн доходы работают!</div>
                </div>
                <div class="passive-income" id="passiveIncome" style="display: none;">
                    <div>💰 Пассивный доход</div>
                    <div><span id="passiveRate">0</span> EC/сек</div>
                </div>
            </div>
            
            <div class="upgrades-section">
                <div class="upgrades-title">⚡ УЛУЧШЕНИЯ ⚡</div>
                <div id="upgradesList">
                    <!-- Upgrades will be dynamically generated here -->
                </div>
            </div>
        </div>
    </div>

    <script>
        class EmpireClickerGame {
            constructor() {
                // Load saved data or use defaults
                const savedData = this.loadGame();
                this.coins = savedData.coins || 0;
                this.clickPower = savedData.clickPower || 1;
                this.totalClicks = savedData.totalClicks || 0;
                this.totalEarned = savedData.totalEarned || 0;
                this.passiveIncome = savedData.passiveIncome || 0;
                this.lastSaveTime = savedData.lastSaveTime || Date.now();
                
                this.upgrades = [
                    {
                        id: 'cursor',
                        name: 'Золотой курсор',
                        description: '+1 к силе клика',
                        baseCost: 10,
                        cost: savedData.upgrades?.cursor?.cost || 10,
                        owned: savedData.upgrades?.cursor?.owned || 0,
                        effect: () => this.clickPower += 1
                    },
                    {
                        id: 'farm',
                        name: 'Золотая ферма',
                        description: '+1 EC/сек',
                        baseCost: 100,
                        cost: savedData.upgrades?.farm?.cost || 100,
                        owned: savedData.upgrades?.farm?.owned || 0,
                        effect: () => this.passiveIncome += 1
                    },
                    {
                        id: 'mine',
                        name: 'Золотая шахта',
                        description: '+5 EC/сек',
                        baseCost: 500,
                        cost: savedData.upgrades?.mine?.cost || 500,
                        owned: savedData.upgrades?.mine?.owned || 0,
                        effect: () => this.passiveIncome += 5
                    },
                    {
                        id: 'factory',
                        name: 'Монетная фабрика',
                        description: '+25 EC/сек',
                        baseCost: 2500,
                        cost: savedData.upgrades?.factory?.cost || 2500,
                        owned: savedData.upgrades?.factory?.owned || 0,
                        effect: () => this.passiveIncome += 25
                    },
                    {
                        id: 'powerClick',
                        name: 'Мощный клик',
                        description: '+10 к силе клика',
                        baseCost: 1000,
                        cost: savedData.upgrades?.powerClick?.cost || 1000,
                        owned: savedData.upgrades?.powerClick?.owned || 0,
                        effect: () => this.clickPower += 10
                    },
                    {
                        id: 'bank',
                        name: 'Имперский банк',
                        description: '+100 EC/сек',
                        baseCost: 10000,
                        cost: savedData.upgrades?.bank?.cost || 10000,
                        owned: savedData.upgrades?.bank?.owned || 0,
                        effect: () => this.passiveIncome += 100
                    }
                ];
                
                this.init();
            }
            
            init() {
                this.calculateOfflineProgress();
                this.bindEvents();
                this.renderUpgrades();
                this.startPassiveIncome();
                this.updateDisplay();
            }
            
            bindEvents() {
                const coin = document.getElementById('empireCoin');
                coin.addEventListener('click', (e) => this.clickCoin(e));
            }
            
            clickCoin(event) {
                this.coins += this.clickPower;
                this.totalClicks++;
                this.totalEarned += this.clickPower;
                
                this.createFloatingNumber(event, `+${this.clickPower}`);
                this.updateDisplay();
                this.updateUpgrades();
                this.saveGame();
                this.saveGame();
            }
            
            createFloatingNumber(event, text) {
                const floatingNumber = document.createElement('div');
                floatingNumber.className = 'floating-number';
                floatingNumber.textContent = text;
                
                const rect = event.target.getBoundingClientRect();
                floatingNumber.style.left = (event.clientX - rect.left) + 'px';
                floatingNumber.style.top = (event.clientY - rect.top) + 'px';
                floatingNumber.style.position = 'absolute';
                
                event.target.style.position = 'relative';
                event.target.appendChild(floatingNumber);
                
                setTimeout(() => {
                    if (floatingNumber.parentNode) {
                        floatingNumber.parentNode.removeChild(floatingNumber);
                    }
                }, 1000);
            }
            
            saveGame() {
                const gameData = {
                    coins: this.coins,
                    clickPower: this.clickPower,
                    totalClicks: this.totalClicks,
                    totalEarned: this.totalEarned,
                    passiveIncome: this.passiveIncome,
                    lastSaveTime: Date.now(),
                    upgrades: {}
                };
                
                this.upgrades.forEach(upgrade => {
                    gameData.upgrades[upgrade.id] = {
                        owned: upgrade.owned,
                        cost: upgrade.cost
                    };
                });
                
                // В этой среде localStorage не поддерживается,
                // но данные сохраняются в памяти для текущей сессии
                window.gameData = gameData;
            }
            
            loadGame() {
                // Загружаем данные из памяти (в реальном приложении был бы localStorage)
                return window.gameData || {};
            }
            
            calculateOfflineProgress() {
                if (this.passiveIncome > 0 && this.lastSaveTime) {
                    const currentTime = Date.now();
                    const offlineTime = Math.floor((currentTime - this.lastSaveTime) / 1000); // в секундах
                    
                    if (offlineTime > 0) {
                        const offlineEarnings = this.passiveIncome * offlineTime;
                        this.coins += offlineEarnings;
                        this.totalEarned += offlineEarnings;
                        
                        // Показываем уведомление о офлайн доходах
                        if (offlineTime > 10) { // показываем только если прошло больше 10 секунд
                            this.showOfflineModal(offlineTime, offlineEarnings);
                        }
                    }
                }
                this.lastSaveTime = Date.now();
            }
            
            showOfflineModal(offlineTime, earnings) {
                const hours = Math.floor(offlineTime / 3600);
                const minutes = Math.floor((offlineTime % 3600) / 60);
                const seconds = offlineTime % 60;
                
                let timeString = '';
                if (hours > 0) timeString += `${hours}ч `;
                if (minutes > 0) timeString += `${minutes}м `;
                if (seconds > 0 || timeString === '') timeString += `${seconds}с`;
                
                // Создаем модальное окно
                const modal = document.createElement('div');
                modal.style.cssText = `
                    position: fixed;
                    top: 0;
                    left: 0;
                    width: 100%;
                    height: 100%;
                    background: rgba(0, 0, 0, 0.8);
                    display: flex;
                    justify-content: center;
                    align-items: center;
                    z-index: 1000;
                    animation: fadeIn 0.3s ease;
                `;
                
                modal.innerHTML = `
                    <div style="
                        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
                        padding: 30px;
                        border-radius: 20px;
                        text-align: center;
                        color: white;
                        box-shadow: 0 20px 40px rgba(0, 0, 0, 0.5);
                        border: 2px solid #ffd700;
                        max-width: 400px;
                        animation: slideIn 0.5s ease;
                    ">
                        <div style="font-size: 3em; margin-bottom: 20px;">💰</div>
                        <h2 style="margin-bottom: 20px; color: #ffd700;">Добро пожаловать!</h2>
                        <p style="margin-bottom: 15px; font-size: 1.1em;">
                            Вы отсутствовали: <strong>${timeString}</strong>
                        </p>
                        <p style="margin-bottom: 25px; font-size: 1.3em; color: #90EE90;">
                            Заработано офлайн: <strong>+${earnings} EC</strong>
                        </p>
                        <button onclick="this.parentElement.parentElement.remove()" style="
                            background: #ffd700;
                            border: none;
                            padding: 15px 30px;
                            border-radius: 25px;
                            color: #333;
                            font-size: 1.1em;
                            font-weight: bold;
                            cursor: pointer;
                            transition: all 0.3s ease;
                        " onmouseover="this.style.background='#ffed4e'; this.style.transform='scale(1.05)'" 
                           onmouseout="this.style.background='#ffd700'; this.style.transform='scale(1)'">
                            Забрать награду!
                        </button>
                    </div>
                `;
                
                // Добавляем стили анимации
                const style = document.createElement('style');
                style.textContent = `
                    @keyframes fadeIn {
                        from { opacity: 0; }
                        to { opacity: 1; }
                    }
                    @keyframes slideIn {
                        from { transform: translateY(-50px); opacity: 0; }
                        to { transform: translateY(0); opacity: 1; }
                    }
                `;
                document.head.appendChild(style);
                
                document.body.appendChild(modal);
                
                // Автоматически закрываем через 10 секунд
                setTimeout(() => {
                    if (document.body.contains(modal)) {
                        modal.remove();
                    }
                }, 10000);
            }
            
            buyUpgrade(upgradeId) {
                const upgrade = this.upgrades.find(u => u.id === upgradeId);
                if (!upgrade || this.coins < upgrade.cost) return;
                
                this.coins -= upgrade.cost;
                upgrade.owned++;
                upgrade.effect();
                upgrade.cost = Math.floor(upgrade.baseCost * Math.pow(1.5, upgrade.owned));
                
                this.updateDisplay();
                this.updateUpgrades();
            }
            
            renderUpgrades() {
                const upgradesList = document.getElementById('upgradesList');
                upgradesList.innerHTML = '';
                
                this.upgrades.forEach(upgrade => {
                    const upgradeElement = document.createElement('div');
                    upgradeElement.className = 'upgrade-item';
                    upgradeElement.innerHTML = `
                        <div class="upgrade-name">${upgrade.name}</div>
                        <div class="upgrade-description">${upgrade.description}</div>
      
