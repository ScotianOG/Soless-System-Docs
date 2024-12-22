# SOLess System Technical Architecture

## System Overview

```mermaid
graph TD
    subgraph "SOLess Ecosystem"
        A[SOLess DEX] --> D[Shared Token System]
        B[SOLspace] --> D
        C[SOLarium] --> D
        D --> E[Smart Contract Layer]
        E --> F[Solana Blockchain]
    end
```

## 1. SOLess DEX Architecture

### 1.1 Gasless Transaction System

```mermaid
sequenceDiagram
    participant User
    participant DEX
    participant Router
    participant LP
    participant Blockchain

    User->>DEX: Initiate Transaction
    DEX->>Router: Check Available Tokens
    Router->>LP: Query Token Balances
    LP->>Router: Return Token Values
    Router->>DEX: Calculate Fee in Selected Token
    DEX->>Blockchain: Execute Gasless Transaction
    Blockchain-->>User: Confirm Transaction
```

### 1.2 Smart Contract Infrastructure

```solidity
contract SOLessRouter {
    struct GaslessTransaction {
        address user;
        address tokenIn;
        address tokenOut;
        uint256 amountIn;
        uint256 amountOut;
        uint256 gasFee;
        address feeToken;
    }
    
    mapping(address => mapping(address => uint256)) public tokenAllowances;
    mapping(address => uint256) public userNonces;
    
    function executeGaslessSwap(
        GaslessTransaction memory transaction,
        bytes memory signature
    ) external returns (bool) {
        require(verifySignature(transaction, signature), "Invalid signature");
        // Swap implementation
    }
}
```

### 1.3 Liquidity Pool Structure

```mermaid
graph LR
    subgraph "Liquidity Pool System"
        A[Token Pairs] --> B[Automated Market Maker]
        B --> C[Price Oracle]
        C --> D[Gas Fee Calculator]
        D --> E[Fee Distribution]
        E --> F[Token Burns]
        E --> G[Treasury]
    end
```

## 2. SOLspace Architecture

### 2.1 Content Verification Flow

```mermaid
sequenceDiagram
    participant User
    participant ContentVerifier
    participant IPFS
    participant Blockchain
    participant NFTMinter

    User->>ContentVerifier: Submit Content
    ContentVerifier->>ContentVerifier: Generate Hash
    ContentVerifier->>IPFS: Store Media
    IPFS-->>ContentVerifier: Return CID
    ContentVerifier->>Blockchain: Record Verification
    ContentVerifier->>NFTMinter: Trigger NFT Creation
    NFTMinter-->>User: Return NFT
```

### 2.2 Storage Architecture

```mermaid
graph TD
    subgraph "Content Storage"
        A[Content Submission] --> B{Content Type}
        B -->|Text| C[On-Chain Storage]
        B -->|Media| D[IPFS]
        D --> E[Arweave]
        C --> F[Content Registry]
        E --> F
    end
```

### 2.3 Verification Smart Contract

```solidity
contract ContentVerification {
    struct Content {
        bytes32 contentHash;
        uint256 timestamp;
        address creator;
        bytes signature;
        bytes32[] parentRefs;
        ContentType contentType;
        VerificationStatus status;
    }
    
    enum ContentType {
        TEXT,
        IMAGE,
        VIDEO,
        AUDIO
    }
    
    mapping(bytes32 => Content) public verifiedContent;
    mapping(address => uint256) public creatorScores;
}
```

## 3. SOLarium Architecture

### 3.1 Floor Price Mechanism

```mermaid
graph TD
    subgraph "Floor Price System"
        A[NFT Mint] --> B[Token Allocation]
        B --> C[Vault Reserve]
        C --> D[Floor Price Calculator]
        D --> E[Buyback System]
        E --> F[Liquidity Pool]
    end
```

### 3.2 NFT Vault Structure

```solidity
contract SOLariumVault {
    struct NFTDetails {
        uint256 tokenId;
        uint256 floorPrice;
        uint256 lockedTokens;
        uint256 lastValuation;
        address creator;
    }
    
    mapping(uint256 => NFTDetails) public nftVault;
    mapping(address => uint256) public creatorVaults;
    
    function calculateFloorPrice(uint256 tokenId) public view returns (uint256) {
        // Floor price calculation logic
    }
}
```

### 3.3 Liquidity Process

```mermaid
sequenceDiagram
    participant NFTHolder
    participant Vault
    participant TokenPool
    participant Market

    NFTHolder->>Vault: Request Liquidation
    Vault->>TokenPool: Check Floor Price
    TokenPool->>Vault: Confirm Available Tokens
    Vault->>NFTHolder: Transfer Tokens
    NFTHolder->>Market: Optional Token Exchange
```

## 4. Integration Points

### 4.1 Cross-Platform Communication

```mermaid
graph TD
    subgraph "Platform Integration"
        A[SOLess DEX] -->|Token Transfer| B[SOLspace]
        B -->|NFT Creation| C[SOLarium]
        C -->|Floor Price| A
        A -->|Liquidity| C
        B -->|Content Value| C
    end
```
