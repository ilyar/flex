# Flex Fees

## Deployment fees

Flex consists of smart contracts communicating to each other to facilitate exchange.

Each of these contracts (Flex Client contract which governs all trader's funds on Flex, internal Flex token wallets, and a few utility contracts) need to be deployed to the blockchain for each Flex trader and an amount of tokens required to start operating needs to be sent to them.

This is done with the help of Flex Auth DeBot when you connect your wallet, deposit or buy a new asset on the exchange.

The initial deployment of Flex contracts costs about 50 EVERs.

Each new assets wallet will take about 30 EVERs. When you buy or deposit an asset you don't have on Flex yet, this amount will be taken from your [Gas balance](guides/keep-up-gas-balance.md) and allocated to your new asset wallet automatically.

What remains of these funds on the balances of these smart contracts can later be withdrawn.

## Trading fees

### Exchange fees

On Flex, takers pay the fees, while makers benefit from the fees. Fee configuration is determined by the `taker_fee` and `maker_vig` parameters of Flex configuration.

When a deal is fulfilled, taker pays the amount of the deal + `taker_fee`.

`taker_fee` is currently set at 0.1%.

Of that amount, the maker gets `maker_vig` amount. The rest (`taker_fee` - `maker_vig`) goes to the exchange.

`maker_vig` is currently set at 0.03%.

So the exchange gets 0.1% - 0.03% = 0.07%.

### Blockchain fees

Trades, being messages between smart contracts also are subject to blockchain fees in native EVER tokens.

* Order creation fee is 0.5 EVERs. It applies to every order.
* Trade completion fee is another 1 EVER. Trade completion fee is payed for by the seller of the major token in the pair if their order is completed with this trade. Otherwise the buyer pays this fee. Thus the maximum blockchain fee for a successful trade is 1.5 EVERs.
* If an order is cancelled, process queue (0.4 EVERs) and return ownership (0.1 EVER) fees are paid instead of trade completion. The total fee of a created and then cancelled order is 0.5 EVERs.
* If an order expires, only return ownership fee of 0.1 EVER is applied. The total fee of an expired order is 0.6 EVERs.

These funds are taken from your [Gas balance](guides/keep-up-gas-balance.md). Currently 3 EVERs  from the Gas balance are attached to order creation and cancellation. Change is returned.

**Note**: When placing large limit orders, in rare cases (low liquidity in token pair spread over many small orders), blockchain fees may be several times higher, as the total fees will include the fees for each of the existing smallers orders, which are needed to fulfill the large order being executed.

#### Detailed description

When an order is created, an account of the following value is created in the Price smart contract for the user:&#x20;

```
account = incoming_value - ev_cfg.process_queue - ev_cfg.order_answer;
```

When a deal is completed, one side pays `deal_costs`:&#x20;

```
deal_costs = ev_cfg.transfer_tip3 * 3 + ev_cfg.send_notify
```

where

```
const ev_cfg = {
        transfer_tip3: 0.3e9, 
        return_ownership: 0.2e9, 
        trading_pair_deploy: 1e9, 
        order_answer: 0.1e9, 
        process_queue: 0.4e9, 
        send_notify: 0.1e9, 
        dest_wallet_keep_evers: 0.01e9 
        };
```

(in nanotokens).

The side paying the `deal_costs` is decided according to the following:

```
 bool last_tip3_sell = (sell.amount == deal_amount) || (sell.amount < deal_amount + min_amount_);
 bool last_tip3_buy = (buy.amount == deal_amount) || (buy.amount < deal_amount + min_amount_);
 bool seller_pays_costs = last_tip3_sell;
```

The three TIP3 transfers included in the deal\_costs are these:

* Major token wallet of the seller -> major token wallet of the buyer
* Minor token wallet of the buyer -> minor token wallet of the seller
* Taker wallet -> exchange's reserve wrapper wallet ([exchange fee](flex-fees.md#exchange-fees))

## Withdraw fees

Unwrapping and withdrawing Flex tokens costs 1.5 EVERs.

## Re-login fees

If you log out of your current instance of Flex, you may need to update your authorization on the next login. This will cost < 1 EVER. You will see the exact amount in the Auth DeBot dialogue.
