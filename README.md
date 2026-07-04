# base-42import requests

API_KEY = "YOUR_BASESCAN_API_KEY"
ADDRESS = "0x1120324D8266d73cca3cc71CcCbd44D919e06E6a"

url = (
    "https://api.basescan.org/api"
    "?module=account"
    "&action=txlist"
    f"&address={ADDRESS}"
    "&startblock=0"
    "&endblock=99999999"
    "&sort=asc"
    f"&apikey={API_KEY}"
)

r = requests.get(url)
data = r.json()

txs = data["result"]

incoming = 0
outgoing = 0

for tx in txs:
    value = int(tx["value"]) / 1e18

    if tx["to"] and tx["to"].lower() == ADDRESS.lower():
        incoming += value

    if tx["from"].lower() == ADDRESS.lower():
        outgoing += value

print("=" * 40)
print("Wallet:", ADDRESS)
print("Transactions :", len(txs))
print("Incoming ETH :", round(incoming, 6))
print("Outgoing ETH :", round(outgoing, 6))
print("Total Volume :", round(incoming + outgoing, 6))
