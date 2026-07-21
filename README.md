# Q Primitives

Q Primitives is the catalog of the Q_ types, the small set of primitive types that the Quanta language and the QVM share. Each type is defined so that a value of it can only be produced in the one sanctioned way, which is what turns a class of contract bugs into something that cannot be written. The normative definition of every type lives in SPEC-primitives in the Quantova-Specs repository, and this repository is the catalog's home in the stack.

Quantova is a sovereign post quantum Layer 1 built from scratch, sharing no code, no wire, and no trust assumption with any other chain. It is post quantum end to end and not a classical chain with a post quantum signature bolted on, built on NIST standardized schemes alone with no classical escape hatch anywhere. Consensus is QORUS, the virtual machine is the QVM running compiled containers, addresses are Q1 bech32m, and the asset is QTOV with its base unit the Quon and TQTOV on the testnet.

## The types

- Q_Address names an account or a contract. It carries the account payload, renders in the Q1 format, and is compared by value.
- Q_Sig binds a signed message to the party that signed it. A value of this type can only be produced by the machine verifying an ML DSA signature over exactly that message, so a contract cannot construct one any other way and an unchecked authority cannot be expressed.
- Q_Asset carries an amount of a declared asset kind and is linear, meaning it must be used exactly once. Copying it or dropping it is a compile error, which is what makes a double spend and a lost balance impossible to write. An asset can be split, merged, and sent. Some asset kinds are origin tagged with the foreign chain they were bridged from, and an origin tagged asset is not valid as validator stake.
- Q_Commit binds a hidden value to a public digest with SHA3, so a party can commit now and reveal later without being able to change the value.
- Q_Rand is a verified output of the verifiable random function. It can only be produced by the machine verifying a random output and its proof, so a contract cannot forge randomness.
- Q_Sealed is confidential in the mempool. It travels under ML KEM key encapsulation and is opened only at execution, which gives protection against front running without breaking any conservation or audit.
- Q_Key holds a public key together with its scheme identifier byte, so the machine knows which scheme to verify under. The secret half is never a value inside a contract.

## Why a value cannot be forged

A theme runs through the catalog. Authority, ownership, randomness, and confidentiality are not fields a contract fills in. They are types the machine alone can produce, by verifying a signature, by moving a linear asset, by checking a random proof, or by opening a sealed value at execution. A contract can hold these values and pass them on, and it cannot fabricate them, which is why the Quanta compiler can reject a forged authority or a conjured balance before a container exists.

## Cryptography

Every primitive that touches cryptography reduces to a NIST standardized post quantum scheme and nothing else. Signatures are ML DSA from FIPS 204, key encapsulation is ML KEM from FIPS 203, and hashing and commitment are SHA3 from FIPS 202. There is no elliptic curve anywhere, and the banned classical crates are refused across the repository by cargo deny.

## Status and honesty

Quantova is at the testnet stage. The cryptography the catalog rests on is a from scratch reference implementation validated against the NIST test vectors and has not been independently audited. Nothing here is audited, unbreakable, or production secure.

## Governance and license

The crypto policy in the Quantova-Specs repository is the supreme law of the stack and governs this repository. Dual licensed under Apache 2.0 and MIT.
