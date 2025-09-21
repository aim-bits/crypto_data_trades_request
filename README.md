# crypto_data_trades_request

SELECT * FROM trades;
SELECT * FROM users;

## Transaction Review – The risk team wants a complete list of all trades this week for compliance review
SELECT * 
FROM trades
WHERE trade_date BETWEEN '2025-09-01' AND '2025-09-03';

## Trade Summary – Finance only needs the price and quantity of each trade for quick reporting.
SELECT price, quantity
FROM trades;

## BTC Monitoring – Compliance asks for all BTCUSDT trades to monitor wash trading activity.
SELECT * 
FROM trades
WHERE symbol = 'BTCUSDT';


## High-Value Trades – Finance wants to identify all trades above $2000.
SELECT * 
FROM trades
WHERE price > 2000;

## Finance wants to identify all trades of high value above $2000.
SELECT trade_id, trade_date, symbol, price, quantity, price * quantity AS trade_value
FROM trades
WHERE price * quantity > 2000;

## Daily Recon – Operations needs to reconcile trades from September 2, 2025.
SELECT trade_id, trade_date, symbol, price, quantity
FROM trades
WHERE trade_date = '2025-09-02';

## Largest Trades –  The Product team wants to see the 3 biggest trades by quantity this week.
SELECT trade_date, symbol, price, quantity,price * quantity AS trade_value, user_id
FROM trades
WHERE trade_date BETWEEN '2025-09-01' AND '2025-09-03'
ORDER BY quantity DESC
LIMIT 3;

## Recent ETH Trade – Marketing asks for the most recent ETHUSDT trade for campaign insights.
SELECT trade_id, trade_date, symbol, price, quantity,price * quantity AS trade_value
FROM trades
WHERE symbol= 'ETHUSDT'
ORDER BY trade_date DESC, symbol;

## Identify the user with the highest trade value
SELECT user_id, SUM(price * quantity) AS total_trade_value
FROM trades
GROUP BY user_id
ORDER BY trade_value DESC
LIMIT 1;

## BTC & ETH Focus – Liquidity team tracks only BTC and ETH trades; fetch just those.
SELECT * t
FROM trades
WHERE symbol IN('BTCUSDT','ETHUSDT');

## Price Range Filter –  Risk control wants BTC trades priced between $25,000 and $28,000.
SELECT *
FROM trades
WHERE symbol = 'BTCUSDT'
  AND price BETWEEN 25000 AND 28000;

## USDT Validation – Operations needs to confirm all trades are USDT pairs (symbols containing “USDT”).
SELECT *
FROM trades
WHERE symbol LIKE '%USDT%';

## User Insights – Compliance wants to know the countries of BTCUSDT traders, combining trade and user data.
SELECT 
  u.user_id,
  t.symbol,
  u.country,
  u.kyc_verified,
  SUM(price * quantity) AS total_trade_value
FROM trades AS t
INNER JOIN users AS u
ON t.user_id = u.user_id
WHERE symbol = 'BTCUSDT'
GROUP BY u.user_id,t.symbol,u.country ;


# Mini-Project 
## Prepare a SQL report answering the following:
## Total number of trades this week.
SELECT COUNT(*) AS total_trades
FROM trades
WHERE trade_date BETWEEN '2025-09-01' AND '2025-09-07';


## Top 5 trades by quantity.
SELECT trade_id, trade_date, symbol, price, quantity, user_id
FROM trades
ORDER BY quantity DESC
LIMIT 5;


## All BTCUSDT trades with user country attached.
SELECT 
  u.user_id,
  t.symbol,
  u.country,
  u.kyc_verified,
  SUM(price * quantity) AS total_trade_value
FROM trades AS t
INNER JOIN users AS u
ON t.user_id = u.user_id
WHERE symbol = 'BTCUSDT'
GROUP BY u.user_id,t.symbol,u.country ;

## Most recent trade for ETHUSDT
SELECT *
FROM trades
WHERE symbol = 'ETHUSDT'
ORDER BY trade_date DESC
LIMIT 1;

# Final Deliverable (as if for management)

Total trades this week: 6

Top 5 trades by quantity: SOL (50), ETH (3.5, 2), BTC (1.2, 0.8)

BTC traders’ countries: Nigeria (KYC’d), Germany (non-KYC)

Most recent ETH trade: Sept 3, 2025, 3.5 ETH at $1850
