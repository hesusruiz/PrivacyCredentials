# Smart Contracts

The Trust Framework is implemented by the Smart Contracts described below.

## The main registry

**ENSRegistry.sol**
:   Implements the main registry and is the first contract that should be deployed.

**ENS.sol**
:   Defines the interfaces and is included by ``ENSRegistry`` and other contracts.


## The resolver contracts

These contracts implement resolution of different types of information that can be registered in the blockchain.

**PublicResolver.sol**
:   This is the main contract and the only one that must be compiled, because all others are included.

**ResolverBase.sol**
:   A utility contract used by all other contracts specialised in resolving different types of information.

Inside the ''profiles'' directory we can find the specialised contracts, all of them are included by the
``PublicResolver.sol`` contract.

**AlaDIDPublicEntityResolver.sol**
:   Registers and resolves the identities of juridical persons (businesses, institutions, etc.). It keeps the DID
    and information required to build the associated DID Document. This contract registers and resolves the first
    public key associated to the DID. Additional public keys are registed using the ``AlaDIDPubkeyResolver`` contract.

**AlaDIDPubkeyResolver.sol**
:   Registers and resolves the public keys associated to the DID. In reality, it is used only if the DID has more
    than one public key. In the simple case where the DID has only one public key, it is registered and resolved
    directly by the ``AlaDIDPublicEntityResolver`` contract.

**AlaPublicCredentialResolver.sol**
:   Registers and resolves metadata about public credentials associated to the DID. In the case of juridical persons,
    they may want to register publicly available credentials (e.g. self-declarations). In general, the bulk of the
    credential data should be stored off-chain, but this contract allows registering critical credential metadata,
    including a tamper-resistant pointer to the actual location of the credential details.

**AlaTSPResolver.sol**
:   Registers and resolves special entities. It is a specialisation of ``AlaDIDPublicEntityResolver`` in the sense
    that it is intended for registration of the Trust Service Providers (TSPs), implementing the EU TSP List of Lists
    on the blockchian. It contains the associated X509 certificates for each TSP, making it very efficient to verify
    digital signatures existing off-chain.

**NameResolver.sol**
:   It performs reverse resolution, from domain name to blockchain address. Each entity in the Trust Framework has
    a domain name assigned at the moment of registration, much in the same way as it happens with standard domain
    names. This contract provides a secure way to determine from the domain name the blockchain address associated
    to it and then retrieve all additional information.

**ContentHashResolver.sol**
:   Registers and resolves Content-addressable Identifiers. It provides the ability for a DID to register pointers
    to additional information that they want to make available to the public.

**TextResolver.sol**
:   A general registration and resolution of any text that the DID wants to store associated to its DID. The content
    is free-format and can be used for almost any purpose that the DID wishes. However, being free-format means that
    the actual format and semantics may not be understood by everybody.


## Deployment of the Smart Contracts

### Compilation

Only two contracts have to be compiled: ``ENSRegistry.sol`` and ``PublicResolver.sol``. All the others are included by these two.

### Deployment

The deployment has to be performed in strict order: first ``ENSRegistry.sol`` and then ``PublicResolver.sol``.

The constructor of ``PublicResolver.sol`` requires the deployed address of ``ENSRegistry.sol``, so it can invoke its
functions when registering/resolving information.