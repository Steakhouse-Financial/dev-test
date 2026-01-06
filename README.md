# Interview test

As a developper at Steakhouse Financial, your job is to develop a model that generate the orders for Morpho Markets V2. We will consider that we are providing orders for the next 6 months (current date + 30 days, current_date + 60 days, ...). For each maturity we will have a ladder. As exemple, for current_date + 30 days, we could have this ladder (BUY $1M 6%, BUY $1M 7%, BUY $1M 8%) meaning we are open to lend $1M at 6%, another $1M at 7%, ... 

The current vault is represented as a dataclass with a liquidity amount invested in a Morpho Vault V1 and a set of term loans the vault own (maturity, amount, and other stuffs as needed). The total amount of orders can exceed the liquidity. For simplicity, each order will be 1% of the remaining liquidity of the vault.

To define those orders ladders, we are using historical prices as input which are provided in the `data.csv` file, using the `borrow_defi_rate`.

For each maturity, we want to use the use the same window of historical time to compute what are the variable rate expected over the maturity (x last days when we forecast next x days variable rates). This need to be converted to a fixed rate which would provide the same interest over the period. From there, we make a ladder of orders by assuming we want a target term premium of 0.1% for each month of duration. Orders will be provided between 0% premium and 2x the target premium depending on the liquidity we have. This mean that is we have the vault fully liquid, we are open to lend 1 month without a premium, but if the vault is almost fully utilized, it willing to lend only at close to 2x the target term premium of 1 month (0.1%).

Bonus: Implement something to favour the vault having a duration of 3 months.

