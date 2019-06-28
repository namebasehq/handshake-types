Handshake types
==
Helpful reference for the shape of Handshake objects, such as transactions.

```typescript
// @flow syntax

type NameHash = string; // 32 bytes
type Height = string; // 4 bytes
type Name = string; // Buffer.from(x: Name, 'hex').toString('ascii')
type Flags = string; // 1 byte
type Hash = string; // 32 bytes
type RecordData = string; // 512 bytes
type Nonce = string; // 32 bytes
type BlockHash = string; // 32 bytes
type Version = string; // 1 byte
type Address = string; // 2-40 bytes
type ClaimHeight = Height; // 4 bytes
type RenewalCount = string; // 4 bytes

export type NoneCovenant = {
  type: 0,
  action: 'NONE',
  items: [],
};
export type ClaimCovenant = {
  type: 1,
  action: 'CLAIM',
  items: [NameHash, Height, Name, Flags],
};
export type OpenCovenant = {
  type: 2,
  action: 'OPEN',
  items: [NameHash, Height, Name],
};
export type BidCovenant = {
  type: 3,
  action: 'BID',
  items: [NameHash, Height, Name, Hash],
};
export type RevealCovenant = {
  type: 4,
  action: 'REVEAL',
  items: [NameHash, Height, Nonce],
};
export type RedeemCovenant = {
  type: 5,
  action: 'REDEEM',
  items: [NameHash, Height],
};
export type RegisterCovenant = {
  type: 6,
  action: 'REGISTER',
  items: [NameHash, Height, RecordData, BlockHash],
};
export type UpdateCovenant = {
  type: 7,
  action: 'UPDATE',
  items: [NameHash, Height, RecordData],
};
export type RenewCovenant = {
  type: 8,
  action: 'RENEW',
  items: [NameHash, Height, BlockHash],
};
export type TransferCovenant = {
  type: 9,
  action: 'TRANSFER',
  items: [NameHash, Height, Version, Address],
};
export type FinalizeCovenant = {
  type: 10,
  action: 'FINALIZE',
  items: [NameHash, Height, Name, Flags, ClaimHeight, RenewalCount, BlockHash],
};
export type RevokeCovenant = {
  type: 11,
  action: 'REVOKE',
  items: [NameHash, Height],
};
export type NameCovenant =
  | ClaimCovenant
  | OpenCovenant
  | BidCovenant
  | RevealCovenant
  | RedeemCovenant
  | RegisterCovenant
  | UpdateCovenant
  | RenewCovenant
  | TransferCovenant
  | FinalizeCovenant
  | RevokeCovenant;
export type Covenant = NoneCovenant | NameCovenant;

type AbstractInput = {
  prevout: {
    hash: string,
    index: number,
  },
  witness: [any, any, any], // TODO
  sequence: number,
};
type CoinInput = AbstractInput & {
  coin: {
    version: number,
    height: number,
    value: number,
    address: string,
    covenant: Covenant,
    coinbase: boolean,
  },
};
type AddressInput = AbstractInput & {
  address: string | null,
};
export type Input = CoinInput | AddressInput;

export type Output = {
  value: number,
  address: Address,
  covenant: Covenant,
};

export type HandshakeTransaction = {
  hash: string,
  witnessHash: string,
  fee: number,
  rate: number,
  mtime: number,
  index: number,
  version: number,
  inputs: Input[],
  outputs: Output[],
  locktime: number,
  hex: string,
};

type FullNodeVersion = '0.0.0';
type HandshakeNetwork = 'main' | 'testnet' | 'simnet' | 'regtest';
type IpAddress = string;
type Port = number;
type Agent = '/hsd:0.0.0/';
export type FullNodeInfo = {
  version: FullNodeVersion,
  network: HandshakeNetwork,
  chain: {
    height: number,
    tip: Hash,
    treeRoot: Hash,
    progress: number,
    state: {
      tx: number,
      coin: number,
      value: number,
      burned: number,
    },
  },
  pool: {
    host: IpAddress,
    port: Port,
    agent: Agent,
    services: string,
    outbound: number,
    inbound: number,
  },
  mempool: {
    tx: number,
    size: number,
  },
  time: {
    uptime: number,
    system: number,
    adjusted: number,
    offset: number,
  },
  memory: {
    total: number,
    jsHeap: number,
    jsHeapTotal: number,
    nativeHeap: number,
    external: number,
  },
};
```
