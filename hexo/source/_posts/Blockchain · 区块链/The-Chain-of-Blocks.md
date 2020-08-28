---
title: The Chain of Blocks
author: FadingWinds
img: >-
  https://images.cointelegraph.com/images/1480_aHR0cHM6Ly9zMy5jb2ludGVsZWdyYXBoLmNvbS9zdG9yYWdlL3VwbG9hZHMvdmlldy8wNmY3YTk1MDNiOWZiODg1ZGNlMjQ5ZDU3ZjA4OWIwMy5qcGc=.jpg
categories:
  - Blockchain · 区块链
tags:
  - notes
  - CT courses
  - updating
summary: 'The story among Alice, Bob and Charlie(Carol). Sometimes more.'
date: 2020-08-17 17:40:20
top:
hidden:
password:
---

**A slightly different writing style for notes. Somehow it just feels right for Blockchain.*


### Chronicle

The year was 2009, when BitCoin appears, also known as 1 A.B.

But the story didn't just start then. It could actually trace all the way back to 32 B.B. - 1977.

RSA => Byzantine Generals problem => Proof of Work

In an economic story, there are two flavors of *financial instrument* existing in the world. 

1. **Bearer Instrument**
   
   Physical holder of object (or secret) possesses asset, e.g. cash, bearer bond.

   Looser definition includes holder of secret, e.g. Swiss bank account (well, once upon a time).

   Pros: Easy to transfer and anonymous; Cons: Easy to lose

2. **Registered instrument**

    Asset with centralized record of ownership, e.g. most types of shares or bonds.

    Pros: Can’t (physically) lose it; Cons: Not anonymous; hard to transfer, unless register is digital

Both types of instrument have a venerable history.

What is money? :dollar:

Four key ingredients for bearer version, mechanisms for:

1. Creation
2. Forgery prevention
3. Verification of validity / ownership
4. Transfer between parties

#### A little history about money

*The Lost Sheep Problem*

Suppose you deposited some sheep in the communal herd...how to ensure no one forgets your sheep, or falsely claims you didn’t deposit them?

A solution: To keep track of goods, clay accountancy tokens were used, could be kept in a safe place to keep track of ownership

More cleverness: How to protect against tampering with or
theft of tokens? 

Tokens sealed in clay envelopes. If in doubt, envelope could be broken open, but only once (*Security Protocol*).

Some challenging security goals:

- Only trusted authority should be able to create tokens.
- Authenticity should be verifiable by anyone.

For *forgery prevention*, coinage usually relied on three things:

- Tokens made from scarce resource
- Sign / signature that was hard to duplicate
- (Death penalty for forgers didn’t hurt)

#### With regard to Bitcoin... :moneybag:	

Scarce resource: **computation**

Hard-to-forge data: **cryptography**

It's a **combination** of bearer instruments and registered instruments.

- Public record of asset ownership called the **blockchain**
- **Secret keys** for control of individual's assets
- Quasi-anonymous (pseudonymous) (like Swiss Bank)
- Loose comparisons to Yap currency (stones), etc., but no perfect physical analog

Where did Bitcoin / blockchains come from?

Created by "Satoshi Nakamoto", but the real identity remains a secret.

#### Blockchain Abstraction

1. Strict ordering of messages
2. Rule-based write, global read
3. No message modification

**Compare: Execution, clearing, and settlement**

Traditional:

- For transfer of financial instruments
- Up to three days to complete (T+3)
- Many middlemen
- Fragmented records
- Difficult to audit

Blockchains are much faster and transparent.

### Bitcoin Basics

#### Transaction authentication

Passwords represent tradeoff

- Convenient but vulnerable
- Require a central to manage them

Alternative: Digital signatures

- Much stronger security
- The central can verify that I authorized a transaction without knowing my secrets

#### Digital Signature

Key Gen => Private key (SK) / Public key (PK)

(M, sig) + PK => Verify

Anyone can run software that executes **KeyGen, Sign, or Verify**, so:

- Any entity X can generate unique keypair ($SK_X$, $PK_X$)
- $X$ can sign using private key $SK_X$
- Anyone can verify $X$’s signatures against $PK_X$
- But PKX does not contain $X$’s real-world identity

Bitcoin uses **ECDSA** (Elliptic-Curve Digital Signature Algorithm). Private key SK is 256 bits (32 bytes); (compressed) public key PK is 33 bytes.

(More details in the later section.)

#### Cryptocurrency Case

For every account X, sign transactions using private key $SK_X$; Assume all public keys $PK_X$ known to the world.

Pros:

- No passwords to steal
- Universal access
- Fast transactions
- Central can’t falsify transactions

Cons:

- Users need to manage private keys (Can’t store in head like
password)
- Central needs to manage public keys
- Partial privacy: Pseudonymity only
- Central can still cheat
  - Can fail to process / post transactions
  - Can go back and erase transactions
  - Can show different subsets of transactions to different users

Private key SK = ownership leakage = theft

Improvement:

- Central periodically (every 10 minutes) digitally signs batches of transactions (linked to old batches)
- Central publishes batch for users to see and record

In this way, 

- Batches of transactions signed with $SK_{Central}$
- Present different batches/delete transactions will let central be caught

Ledger now constructed as **a chain of blocks**.

A block is **a batch of transactions + a digital signature**.

Whole chain contains all transactions over time.

Specifies complete system history.

BUT central can still **suppress transactions**:

- Refuse to process / post transactions
- If central cheats, there's no recourse (You catch it cheating, not much you can do)
- Trust resides in a single entity

Why not trust a central authority? (e.g. banks) -- Hyperinflation, Frozen assets

#### Consensus

For Bitcoin, similarly,

- Pseudonymous: User identities correspond to (SK, PK) key pairs
- Users digitally sign transactions to
authorize movement of money
- Transactions recorded in blockchain

And differently,

- The blockchain is fully decentralized
- Instead of one trusted entity, we rely on whole community

the problem of **consensus**: How does community agree on what transactions in ledger?

Naïve consensus idea: Everyone broadcasts to everyone else

*Problems*: Won’t work with thousands or millions of
participants (not always online); And even if it did, messages delivery times may vary -- ordering could affect validity

Improve: Vote

*Problems*: vote early, vote often (Sybil Problem)

Solution: **Bitcoin Mining**

- In Bitcoin, blocks of transactions made authoritative by **mining**
- Anyone in community can be a miner
- Miners all race to find community signature on current,
ordered batch of transactions
- Community signature discovered via **computationally costly process** (mining)
- (No private key for this signature type)
- Block then published with signature

#### Mining

Hash function (details in the next section)

To mine block, miner keeps trying different values of $N$ **until $H(B||N)$ has $k$ leading zeros**.

- $B$ block data including fresh transactions
- $N$ is a nonce
- $k$ is set by system
- Lots of 0s = lots of work!
- The faster your machine, the higher the probability that you get lucky and are the first to mine a block

To achieve decentralized / community signature (without identities or trusted hardware), this is **only known, fully battle-tested procedure**.

aka. **Proof of Work**

#### Acquire and Spend

- Anyone can create her own "address" / account X
  - Digital wallet (app) can do this for you
  - Creates cryptographic "key pair" ($SK_X$, $PK_X$)
- Secret key $SK_X$: to authorize use of your Bitcoin
- Public key $PK_X$: public identifier and to validate transactions
- Address comes from public key

Transaction lifecycle:

1. You scan QR code -- get address A of receiver.
2. You choose transfer amount.
3. Your wallet uses your private key (SK) to digitally sign
transaction.
4. Your wallet broadcasts transaction to cryptocurrency network / miners (I'm sending X coins to A).
5. Miners pick up your transaction.
6. A successful miner includes your transaction in a block.
7. Your transaction is on the blockchain!
8. The whole world knows you sent X coins to address A.

### Hash Function

A deterministic cryptographic function.

$H$ takes as input any desired bitstring / text B, outputs a random(-looking) fixed-size (256-bit) value digest $H(B)$.

*SHA-256^2 used in Bitcoin.*

Same input $B$ always produces same output, and any $B' ≠ B$ produces completely different, random-looking output $H(B')$.

Think of it as: A unique "fingerprint" of message B, and a very lossy compression of message B.

Common cryptographic hash function: MD5, SHA-1, SHA-256 (n = 256)

Two **key security properties** for a cryptographic hash
function $H$:

1. **preimage resistance**

- Image is any n-bit value $y$
- Given image $y$, a preimage is any $x$ s.t. $H(x) = y$
- **Given random $y$ (uniform over $\{0,1\}^n$), it's infeasible to find image $x$**.

Note, though, that for at least one image, there are infinitely many preimages. (?)

2. **Collision-resistance**

It is **hard** to find any pair of inputs $(w,x)$ such that
$H(w) = H(x) = y$.

#### Random Oracle Model

An ideal hash function case for conceptual simplicity, mathematically rigorous but simple proofs of security and understanding how to use hash functions.

*Question: Birthday Paradox* :birthday:

There are $N$ (=365) days in an (ordinary) year. Suppose there are $k$ people in the room. Assume (uniformly) randomly
distributed birthdays. How large must $k$ be for it to be likely (prob. ≥1/2) that two people share a birthday?

<details>
<summary>Click to see the answer</summary>

Prob ≈ 50.7% for k = 23.

For k = 120 ≈ 99.9999999+% chance.

For k = 120 ≈ 83% chance of three-way collision.
</details>

Birthday paradox illuminates collision-resistance.

**General rule of thumb in cryptography**: 128-bit security, meaning $2^{128}$ work for attacker, is "strong"

#### Blockchain Applications in Hashing

1. Addresses

    Public key (PK) in Bitcoin: 256-bit value

    Address in Bitcoin: base-58 string

    Address is hash of PK (plus other stuff)

2. Script hashing (?)

    P2SH-type transaction (Pay-to-Script-Hash)

    Special transaction: payment sent to "script hash" ('3' address), H(S) for "redeem script" S

    Redeem script S included in spending transaction

3. Transaction IDs

    Hash of transaction data is transaction identifier (TXID)

4. **Chaining Blockchain Blocks**

    Block header includes hash of previous block

5. Mining

    Special-purpose hardware is blazingly fast! ASIC (Application-Specific Integrated Circuit)

    Instead: **Memory-hard hash functions** (Make computation of H gobble up lots of fast memory)

    e.g. scrypt, Argon2, Balloon Hashing

    Meant to be ASIC-resistant (but with mixed success)

6. Commitment schemes

    **Commitment**: 
    
    Computational hiding.

    Suppose Alice (welcome our first character) chooses short message $m$, e.g. $m \in {0,1}$,Alice gives us $C = H(m)$.

    Easy to compute m from $C = H(m)$, like brute-force password cracking -- $C = H(0) or H(1)$?

    Can Alice somehow use $H$ to hide $m$?

    Answer: Alice chooses random, secret key $r$ and gives us $C = H(m||r)$, without $r$

    Collision resistance forbids Alice to cheats (change $m$)

    A good commitment scheme is:
    
    - Efficient: easy to compute $C$
    - Hiding: hard to compute $m$ from $C$
    - Binding: hard to change $m$ for given commitment $C$

7. Blockchain inclusion proofs

Use short “Root” to prove existence of a transaction: **Merkle Trees**

#### Coin-flip Protocol

In many blockchain protocols, "committee" must choose random
number.

Goal: Alice, Bob, and Charlie together generate a truly random bit $b$, assume at least one of them is honest

- Generate respective random bits $b_A$, $b_B$, $b_C$.
- Generate commitments $c_A$, $c_B$, and $c_C$ to their respective bits
- Post their commitments to the blockchain
- Compute $b$ = $b_A$ XOR $b_B$ XOR $b_C$

Deadline problem: the last person waits and decides whether to decommit

### Basic Crypto

#### Public Key Cryptography

Symmetric-key encryption

Symmetric = Classical (intuitive) encryption

Alice => Bob with $C=enc_K[M]$, where $K$ is a secret

Main challenge: How to share K safely to begin with?

Unfortunately, meeting under bridge :smiling_imp:	:bridge_at_night: at night not scalable for Internet

When logging on website, we have a secure channel to send things -- User and website somehow manage to choose and share a random, secret key K, despite:

- No prior communication about K
- Eavesdropper or malicious entity intercepting their communication

**Merkle puzzles**

Randomly selected secret keys and indices.

Alice sends => $P_1, P_2, ..., P_n$ where $P_j = (k_j, i_j)$

Bob returns => $i_r$

- $(k_j, i_j)$ are embedded in puzzle
- Constructing puzzle is easy (O(1) work)
- Solving puzzle is made moderately hard: brute force decryption requires O(m) work
- The evil Eve doesn’t know **which puzzle** Bob chose -- so she has to keep solving until she finds it.

Observe that:

- Alice does work O(n)
- Bob does work O(m)
- Eve must do work O(n × m) (much harder than Alice and Bob)

#### Diffie-Hellman key agreement

First practical public-key cryptosystem, and the simplest.

Scenario:

Alice and Bob want to share a secret key $K$, but:

- They've never met.
- They don’t want Eve, who’s eavesdropping, to learn $K$.

**Discrete log problem**

Given a group $G$ of order $q$ and the pair $(g,y)$, where
$g$ is a generator of G and $y = g^x$ for random $x \in [0,q-1]$, compute $x = log_gy$ .

Typical choices of G:

- $p = 2q+1$, for primes $p$ and $q$ (or $q ∣ p - 1$)
- Computation is performed mod $p$
- $g$ generates cyclic subgroup $G$ of order $q$
- So Alice’s public-key is $A = g^a \space mod \space p$, for $a \in R
[0,q-1]$
- Typical parameter choices: $p$ is a 2048-bit prime, $q$ is a 256-bit value

Random values in exponent space are hidden, e.g. $x$ is hidden in $y=g^x$. So we can "compute secretly" in the exponent space. (now omit mod p for visual clarity)

(Simplified) DH Key Agreement:

1. Key Generation

    Alice - Random private key $a$ and public key $A=g^a$

    Bob - Random private key $b$ and public key $B=g^b$

2. Public Key Exchange

3. Compute Secret, Shared Key

    Alice - $K' = B^a = (g^b)^a = g^{ba} = g^{ab}$

    Bob - $K' = A^b = (g^a)^b = g^{ab}$

Why can't Eve learn about $K$?

- Values $a$ and $b$ are in exponent space, so they remain hidden.
- Alice can multiply hidden value $b$ by known value $a$; vice versa for Bob.
- Eve doesn’t know $a$ or $b$, can't do secret multiplication (DH assumption).
- Eve can only compute, e.g. AB = $g^ag^b$ = $g^{a+b}$.
- $g^{ab}$ is hashed to obtain symmetric key, e.g., AES key

**RSA Encryption**: Uses modular exponentiation
(like D-H); Security related to hardness of factoring -- Given $pq$ for large primes $p$ and $q$, compute $p$ and $q$

How hard it is to break RSA?

• Best known general attack involves factoring $N = pq$
• Difficulty of best classical factoring algorithm (general number field sieve) grows super-polynomially (but sub-exponentially)

In fact...encryption uses *nowhere* in Bitcoin or basic Ethereum :upside_down_face:. But it's important to know it because:

- Important background for digital signatures
- Important **contrast** with digital signatures
- Used in advanced blockchain protocols

#### Digital Signatures

A metaphor story:

You have a special signet ring (rubber stamp ring? :wink:), everyone knows what your signature looks like (PK) but only the ring can produce it (SK).

Security property for digital signatures: Should *only* be possible to sign using SK, even though Alice's public key PK is published, i.e., known to world and anyone can verify using PK.

In encryption, public key used to **initiate algorithm** ( Encrypt under public key), with digital signatures, private
key used to initiate algorithm (Sign using private key) -- **"Opposite" workflows**

#### Schnorr Identification

A **interactive signature scheme**:

Goal: Alice identifies herself to Bob by proving knowledge of her private key.

Assume Alice's SK/PK: $(a,A=g^a)$ and Bob has $A$

- Bob sends Alice $c$
- Alice replies with $s=ca+r$
- Bob checks $g^{s} == RA^c$

Intuition: 

- Bob can verify that $a$ is properly "mixed in" -- so it’s really Alice.
- Alice removes $a$ from exponent space, reveals it in $s$, but, $r$ is a one-time value that blinds, i.e., conceals $a$.

Problems: 

- Requires interaction
- Bob can't prove to another person that Alice "signed" $c$

**Schnorr signature scheme**:

In addition to above, 

- Alice has a **one-time** (if not, look for "Sony PS3 Break" accident) private key $r$ and public key $R = g^r$. $C=H(R,m)$
- Signature: $(R, s = ca + r)$
- Verify: $g^s == RA^c$
- Key point: generate "challenge" c from m

#### ECDSA

Operates over subgroup on **elliptic curve**

- Subgroup of order $n$ generated by base point $g$
- Public / private key pair: $(g^a, a)$
- To sign $m$, compute $e = H(m)$
- Truncate $e$ to $⌈log2 n⌉$ bits (bit length of subgroup order)
- k ⇐$ [1,n-1]
- $(x,y) = g^k; x' = x$ mod $n$
• $s = k^1(e + x'a)$ mod $n$
• Signature is $(r,s)$

### UTXOs

Unspent Transaction Outputs.

The (intuitive) account model (used in Ethereum): 

*Block n*: Starting balances + TX (transactions) => *Block n+1*: New balances

*Addresses are actually for one-time use (more about this later)

But in Bitcoin... **No explicit balances, only a set of transactions.** Circulating money consists of Unspent Transaction Outputs (UTXOs).

A transaction contains **IN** and **OUT**, IN = OUT

Alice recovers unspent value by giving herself a partial refund.

What needs to be shown in [in] to prove legitimate use of [out]? [in] must:
- Include "unlocking code"
- With valid signature $sig$ for Transaction under
$SK_{Alice}$

Complicated scripts are usually P2SH (Pay to Script Hash).

Bitcoin scripting language not Turing complete; but still many possible scripts, e.g.

1. Multisig

    k-out-of-N sig -- Why not N-out-of-N?

    - Ensure no one steals money
    - Ensure collective agreement on which music videos to fund (or anything else in other cases)
    - And protect against loss of one key

    Application: Joint control, Escrow, 

    Case: Alice sells Bob a Lambo. Suppose Carol is a trusted third party, and can verify delivery of Lambo. To ensure that 
    - If Alice and Bob agree, Carol isn’t bothered.
    - If there’s a dispute, Carol can make sure money goes to right person.

    <details>
    <summary>Click to see solution</summary>

    Bob pays into 2-out-of-3 multisig, if Lambo not delivered or Bob cheats, Carol and honest player direct money.
    </details>

2. Payment channels

    Problem: On-chain Bitcoin transactions are expensive and slow
    
    Solution: Make Bitcoin payments (mostly) without using blockchain (:raised_eyebrow:) -- Mechanism called payment channel
    
    Main implementation: Lightning network

    Case: Alice wants to make multiple small payments to Bob (e.g. subscription) and prefunds channel


3. Cross-chain atomic swaps
4. Zero-Knowledge Contingent Payments

### Byzantine Agreement

The third person can't tell who's cheating (he said, she said).

#### Synchronous BA

There does not exist an n-party $(n > 3)$ Synchronous
Byzantine Agreement protocol $\Pi$ that is $(t;\epsilon)$-secure for $t ≥ ⌈n/3⌉$ and $\epsilon < 1/3$. (That is, secure w.p > 2/3 with 1 faulty player).

Idea: convert into 3-player protocol

#### Asynchronous BA

FLP (Fischer, Lynch, Patterson):


<br>
<br>







****

*Banner Image Ref:*

https://cointelegraph.com/bitcoin-for-beginners/what-are-cryptocurrencies
