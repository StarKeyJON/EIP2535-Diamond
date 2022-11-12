# EIP2535-Diamond
A blueprint for the EIP-2535 Diamond implementation

[more info on Diamonds](https://ethereum-blockchain-developer.com/110-upgrade-smart-contracts/11-eip-2535-diamond-standard/)
[AAVEGOTCHI-GHST-Staking](https://github.com/aavegotchi/ghst-staking) Uses the diamond-2 implementation of EIP-2535 Diamond Standard.

EIP-2535 is a proposal to modularize the code of contracts on Ethereum. Its purpose is to allow large smart contracts to break through the maximum size limit of 24kb, and to make it easier for contracts to update functions.

To understand the Diamond Protocol, there are several related concept definitions that need to be known:

1. Diamond : Diamond can be understood as a proxy contract (Proxy), and it is also the main contract that interacts with users
2. Facet : Just like a real diamond has different sides, a diamond contract also has different faces. The contract that needs to be called for each function of the diamond contract corresponds to a facet, so it can also be understood as an implementation contract (Implementation)
3. Diamond Cut : The Diamond Protocol Standard extends a function called Diamond Cut. Its main function is to add, replace or delete facets and functions from diamonds, which can be understood as the upgrade of the contract.
4. The Loupe : The magnifying glass function in the diamond protocol standard is mainly to return the information about the facet and the existence of the diamond, which is stored in the internal storage structure of the diamond contract – DiamondStorage

Cons: 

1. The proxy could be a central point of entry to a larger ecosystem of Smart Contracts. Unfortunately, larger systems often make use of inheritance quite heavily and therefore you have to be extremely careful with adding functions to the Diamond proxy. Also function signatures could easily collide for two different parts of the system with the same name.

2. Every Smart Contract in the System needs adoption for the Diamond Storage, unless you use only one single facet that uses unstructured storage. Simply adding the OpenZeppelin ERC20 or ERC777 tokens wouldn't be advised, as they would start writing to the Diamond Contract storage slot 0.

3. Sharing storage between facets is dangerous. It puts a lot of liability on the admin.

4. Adding functions to the Diamond via diamondCut is quite cumbersome. I do understand that there are other techniques where the facets bring their own configuration.

5. Adding functions to the Diamond via DiamondCut could become quite gas heavy. Adding the two functions for our FacetA Contract costs 109316. That's $20. Extra.
