journalctl -u okp4d -f
systemctl restart okp4d
curl localhost:26657/status
curl -s localhost:26657/status | jq .result.sync_info.catching_up
okp4d keys show wallet --bech val -a
okp4d tx staking delegate YOUR_VALOPER_ADDRESS 10000000uknow --from wallet --chain-id okp4-nemeton --fees 5000uknow
okp4d query staking validators --limit 2000 -o json | jq -r '.validators[] | select(.status=="BOND_STATUS_BONDED") | [.operator_address, .status, (.tokens|tonumber / pow(10; 6)), .description.moniker] | @csv' | column -t -s"," | sort -k3 -n -r
okp4d query staking validators --limit 2000 -o json | jq -r '.validators[] | select(.status=="BOND_STATUS_UNBONDED") | [.operator_address, .status, (.tokens|tonumber / pow(10; 6)), .description.moniker] | @csv' | column -t -s"," | sort -k3 -n -r

remove node;
systemctl stop okp4d
systemctl disable okp4d
rm -rf $(which okp4d) ~/.okp4d ~/okp4d
