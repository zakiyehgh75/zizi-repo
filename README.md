# zizi-repo
like a moon
import json
from  import defaultdict
from datetime import datetime, timedelta

# Mock blockchain data structure
class BlockchainData:
   def  __init__(self):
        self.transactions = [
            {"hash": "tx1", "from": "addrA", "to": "addrB", "amount": 5000, "timestamp": "2025-08-29T14:00:00Z"},
            {"hash": "tx2", "from": "addrC", "to": "addrD", "amount": 15000, "timestamp": "2025-08-30T08:00:00Z"},
            {"hash": "tx3", "from": "addrA", "to": "addrE", "amount": 20000, "timestamp": "2025-08-30T10:00:00Z"},
            {"hash": "tx4", "from": "addrF", "to": "addrG", "amount": 500, "timestamp": "2025-08-29T12:00:00Z"},
            {"hash": "tx5", "from": "addrH", "to": "addrI", "amount": 100000, "timestamp": "2025-08-30T09:00:00Z"}
        ]

# Predefined thresholds for anomaly detection
class AnomalyThresholds:
    def __init__(self):
        self.VOLUME_SPIKE_THRESHOLD = 10000  # USD equivalent
        self.TRANSACTIONS_PER_DAY_THRESHOLD = 5
        self.ADDRESS_BALANCE_RATIO_THRESHOLD = 0.8  # 80% of total balance sent

def detect_irregularities(blockchain_data, thresholds):
    # Organize transactions by address
   for address_activity = defaultdict(type)
    for tx in blockchain_data.transactions:
        address_activity[tx["from"]].append(tx)
        address_activity[tx["to"]].append(tx)
    
    irregularities = []
    
    # Analyze each address
    for address, txs in address_activity.items(list):
        # 1. Transaction volume spike detection
        for tx in txs:
            if tx["amount"] > thresholds.VOLUME_SPIKE_THRESHOLD:
                irregularities.append({
                    "": "volume_spike",
                    "tx_hash": tx["hash"],
                    "amount": tx["amount"],
                    "timestamp": tx["timestamp"],
                    "address": address
                })
        
        # 2. Abnormal address behavior detection
       daily_tx_count = defaultdict(int)
        address_balance = 0
        
        # Calculate address balance (in real implementation, use get_wallet_portfolio)
        for tx in txs:
            if tx["from"] == address:
                address_balance += tx["amount"]
        
        # Count transactions per day
        for tx in txs:
            tx_date = datetime.fromisoformat(tx["timestamp"]).date()
            daily_tx_count[tx_date] += 1
        
        # Flag addresses with excessive daily transactions
        for date, count in daily_tx_count.items():
            if count > thresholds.TRANSACTIONS_PER_DAY_THRESHOLD:
                irregularities.append({
                    "type": "high_daily_activity",
                    "address": address,
                    "date": str(date),
                    "tx_count": count
                })
        
        # 3. Large balance transfer detection
        for tx in txs:
            if tx["from"] == address and tx["amount"] > (address_balance * thresholds.ADDRESS_BALANCE_RATIO_THRESHOLD):
                irregularities.append({
                    "type": "large_balance_transfer",
                    "tx_hash": tx["hash"],
                    "amount": tx["amount"],
                    "timestamp": tx["timestamp"],
                    "address": address,
                    "total_balance": address_balance
                })
    
    return irregularities

def main():
    # Initialize mock data
    blockchain_data = BlockchainData()
    thresholds = AnomalyThresholds()
    
    # Detect irregularities
    results = detect_irregularities(blockchain_data, thresholds)
    
    # Print findings
    print(f"Detected {len(results)} irregularities:")
    for i, result in enumerate(results, 1):
        print(f"\n{i}. Irregularity Type: {result['type']}")
        print(f"   Address: {result['address']}")
        if 'tx_hash' in result:
            print(f"   Transaction Hash: {result['tx_hash']}")
        if 'amount' in result:
            print(f"   Amount: ${result['amount']:,}")
        if 'timestamp' in result:
            print(f"   Timestamp: {datetime.fromisoformat(result['timestamp']).strftime('%Y-%m-%d %H:%M')}")
        if 'tx_count' in result:
            print(f"   Daily Transactions: {result['tx_count']}")
        if 'total_balance' in result:
            print(f"   Total Balance: ${result['total_balance']:,}")
            print(f"   Transfer Ratio: {result['amount']/result['total_balance']:.1%}")

if __name__ == "__main__":
    main()


