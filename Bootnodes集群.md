雖然Status是一個分散的應用程序，但它使用\[[http://scaleway.com/calendar.com\]上託管的集群，該集群可用於下述多種目的](http://scaleway.com/calendar.com%5D上託管的集群，該集群可用於下述多種目的)。

# 目標

## Mine Ropsten ether

我們有自己的發測試用以太幣的服務(faucet)在應用程序內按需發送以太幣。這個以太幣是由我們的集群開采的。

## 為移動客戶端提供“bootnodes”

從Geth的角度來看，bootnode是一個只能參與發現的剝離節點（請參閱https://ethereum.stackexchange.com/questions/12503/what-is-the-bootnode-command-in-the-go-ethereum
-documentation）。

從狀態的角度來看，bootnode可以幫助移動的節點連接到其他節點，並且可以從中獲取數據塊。因此，我們不僅僅是發現其他節點的工具，我們集群中的節點也將它們的塊共享給請求者。

這種解決方案通過加快發現速度，使移動節點的區塊鏈同步更快。但是，如果移動節點運行其本地以太坊節點而不是使用遠程節點（請參閱有關遠程節點：TBD），則僅有意義。

## Relay whisper messages with low PoW

由於移動節點的計算能力有限，我們將PoW設置為低於geth默認值的值。這使得常規節點無法中繼這些消息，並且我們允許群集處理該消息。

# 運作

Enode被崁入移動的二進製文件中，無需重新編譯就無法更改。具體來說，他們的地址可以在https://github.com/status-im/status-go/blob/develop/static/config/cht.json以及CHT（Canonical
Hash Trie）以太坊數據塊中找到。

實際的集群部署配置可以在https://github.com/status-im/status-cluster找到

[Category:Status-go](Category:Status-go "wikilink")