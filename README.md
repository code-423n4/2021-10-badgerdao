# ✨ So you want to sponsor a contest

This `README.md` contains a set of checklists for our contest collaboration.

Your contest will use two repos: 
- **a _contest_ repo** (this one), which is used for scoping your contest and for providing information to contestants (wardens)
- **a _findings_ repo**, where issues are submitted. 

Ultimately, when we launch the contest, this contest repo will be made public and will contain the smart contracts to be reviewed and all the information needed for contest participants. The findings repo will be made public after the contest is over and your team has mitigated the identified issues.

Some of the checklists in this doc are for **C4 (🐺)** and some of them are for **you as the contest sponsor (⭐️)**.

---

# Contest setup

## ⭐️ Sponsor: Provide contest details

Under "SPONSORS ADD INFO HERE" heading below, include the following:

- [ ] Name of each contract and:
  - [ ] lines of code in each
  - [ ] external contracts called in each
  - [ ] libraries used in each
- [ ] Describe any novel or unique curve logic or mathematical models implemented in the contracts
- [ ] Does the token conform to the ERC-20 standard? In what specific ways does it differ?
- [ ] Describe anything else that adds any special logic that makes your approach unique
- [ ] Identify any areas of specific concern in reviewing the code
- [ ] Add all of the code to this repo that you want reviewed
- [ ] Create a PR to this repo with the above changes.

---

# Contest prep

## ⭐️ Sponsor: Contest prep
- [X] Make sure your code is thoroughly commented using the [NatSpec format](https://docs.soliditylang.org/en/v0.5.10/natspec-format.html#natspec-format).
- [X] Modify the bottom of this `README.md` file to describe how your code is supposed to work with links to any relevent documentation and any other criteria/details that the C4 Wardens should keep in mind when reviewing. ([Here's a well-constructed example.](https://github.com/code-423n4/2021-06-gro/blob/main/README.md))
- [ ] Please have final versions of contracts and documentation added/updated in this repo **no less than 8 hours prior to contest start time.**
- [X] Ensure that you have access to the _findings_ repo where issues will be submitted.
- [X] Promote the contest on Twitter (optional: tag in relevant protocols, etc.)
- [X] Share it with your own communities (blog, Discord, Telegram, email newsletters, etc.)
- [ ] Optional: pre-record a high-level overview of your protocol (not just specific smart contract functions). This saves wardens a lot of time wading through documentation.
- [X] Designate someone (or a team of people) to monitor DMs & questions in the C4 Discord (**#questions** channel) daily (Note: please *don't* discuss issues submitted by wardens in an open channel, as this could give hints to other wardens.)
- [X] Delete this checklist and all text above the line below when you're ready.

---

# BadgerDAO ibBTC Wrapper contest details
- $28,500 worth of ETH main award pot
- $1,500 worth of ETH gas optimization award pot
- Join [C4 Discord](https://discord.gg/code4rena) to register
- Submit findings [using the C4 form](https://code423n4.com/2021-10-badgerdao-ibbtc-wrapper-contest/submit)
- [Read our guidelines for more details](https://docs.code4rena.com/roles/wardens)
- Starts October 28, 2021 00:00 UTC
- Ends October 30, 2021 23:59 UTC

This repo will be made public before the start of the contest. (C4 delete this line when made public)

[ ⭐️ SPONSORS ADD INFO HERE ]



# ibBTC Rebasing Wrapper
When ibBTC is placed in a curve BTC metapool, the value of ibBTC tokens will deviate relative to the value of the Curve LP tokens. This is due to the yearn vault-like price per share mechanic of ibBTC. StableSwap invariant means you only get low or even reasonable slippage when the balances of both coins are very close. The variance between value may perpetually increase as ibBTC increases in value.

While we develop a custom pool, this wrapper allows for the value of each ibBTC coin in the pool to remain equal to the underlying tokenized BTC collateral so that the metapool can function as intended.

Introducing, **wibBTC**.

## How it works
* Users deposit ibBTC into the contract and are minted shares at 1-1 to the token value deposited. This value is tracked in `sharesOf(account)`
* `totalShares()` is the number of ibBTC tokens in the wrapper
* `totalSupply()` is ibBTC pricePerShare * totalShares. The total supply increases relative to the number of shares as the pricePerShare increases. This means 1 unit of wibBTC = 1 tokenized BTC in value.
    * ibBTC pricePerShare is read directly from the ibBTC Core contract on Ethereum, and is read from an oracle on other chains.
* `balanceOf(account)` = sharesOf(account) * pricePerShare. As per typical rebasing tokens, each accounts' balance scales

## Admin Functionality
* Governance 
* Governance can set a new pendingGovernance address, which must `acceptPendingGovernance()`


## Risks
* We introduce a trusted oracle on non-ETH chains. If it goes haywire and reports bad data, the pool could have 1000x the ribBTC, or 1/1000 the riBBTC, compared to iBBTC. What will happen in this case?
* On Ethereum, if there's a pricePerShare bug in the ibBTC Core, what new considerations are introduced by the curve pool?
* As with any rebaser we introduce precision loss / _dust_. Are there any concerns here?

* What sanity checks to have on oracle?
    * Ensure oracle value never changes by an extreme amount?
    * Ensure oracle never goes down?
    * What should happen IF ibBTC is legitimately exploited... Is it better to keep this value at the new value or the pre-exploit expected value?

## Contracts
A live instance of the wrapper can be found here (pending). It's using a proxy + logic split for upgradeability. 
There is a test curve pool vs sbtc on Ethereum (pending), and a test pool vs ren on Polygon.

# UX Considerations
Users need to convert ibBTC to wibBTC before depositing into curve pool. They then need to deposit values into the curve pool denominated in balances rather than shares.

# CodeArena Spec
4. How many external calls? 
    * One, to ibBTC Core contract on ETH mainnet, and to a defined oracle address on other chains.
5. Does it use an oracle?
    * Yes, this is a custom ibBTC pricePerShare oracle in development by chainlink.
6. Does the token conform to the ERC20 standard?
    * Yes, with custom minting & burning functions. It's based on the OZ ERC20Upgradeable contract.
12. Is it multichain?
    * Yes in the sense that it reads oracle data from other chains when not used on ETH mainnet.
13. Does it use a sidechain?
    * It will exist on sidechains as per above
13a. If yes, is the sidechain evm-compatible?
    * Yes, it will only be present on EVM sidechains

# Basic Sample Hardhat Project
This project demonstrates a basic Hardhat use case. It comes with a sample contract, a test for that contract, a sample script that deploys that contract, and an example of a task implementation, which simply lists the available accounts.

Try running some of the following tasks:

```shell
npx hardhat accounts
npx hardhat compile
npx hardhat clean
npx hardhat test
npx hardhat node
node scripts/sample-script.js
npx hardhat help
```

