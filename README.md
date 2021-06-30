# EVM js execution trace obtainer example

We have a siple contract called `StackPushPop.sol` under `contracts/`.
The plan is to obtain a full trace of the execution of it  step by step and
dump the output in a JSON or a similar format friendly with the specs
of the BUS-MAPPING found in [here](https://hackmd.io/Hy_nqH4yTOmjjS9nbOArgw#psudo-code).


To get the trace simply do:
```
npm i
npm test
```

If you want the full trace stored inside a file, dump stdout to a file:
`npm test &> trace.txt`

This will require you to have installed `solc` and `node.js`.

The trace actually looks like this for a single step of the execution of the `push_pop()` fn:
```
{
  pc: 31,
  gasLeft: <BN: 1e31ea>,
  gasRefund: <BN: 0>,
  opcode: { name: 'PUSH4', fee: 3, isAsync: false },
  stack: [ <BN: 94e9df35>, <BN: 94e9df35> ],
  returnStack: [],
  depth: 0,
  address: Address {
    buf: <Buffer 61 de 9d c6 f6 cf f1 df 28 09 48 08 82 cf d3 c2 36 4b 28 f7>
  },
  account: Account {
    nonce: <BN: 1>,
    balance: <BN: 0>,
    stateRoot: <Buffer 56 e8 1f 17 1b cc 55 a6 ff 83 45 e6 92 c0 f8 6e 5b 48 e0 1b 99 6c ad c0 01 62 2f b5 e3 63 b4 21>,
    codeHash: <Buffer d1 f6 bd e5 59 bc 14 d0 e6 96 70 f6 97 59 5e a2 dc 4b 0f 99 fe 5d 93 b9 48 d6 43 3d c2 83 8f d4>
  },
  stateManager: DefaultStateManager {
    _common: Common {
      _events: [Object: null prototype],
      _eventsCount: 1,
      _maxListeners: undefined,
      _supportedHardforks: [Array],
      _eips: [],
      _customChains: [],
      _chainParams: [Object],
      DEFAULT_HARDFORK: 'istanbul',
      _hardfork: 'istanbul',
      [Symbol(kCapture)]: false
    },
    _trie: SecureTrie {
      EMPTY_TRIE_ROOT: <Buffer 56 e8 1f 17 1b cc 55 a6 ff 83 45 e6 92 c0 f8 6e 5b 48 e0 1b 99 6c ad c0 01 62 2f b5 e3 63 b4 21>,
      lock: [Semaphore],
      db: [CheckpointDB],
      _root: <Buffer 91 db d4 a7 4f c1 82 09 91 28 d1 38 d2 53 93 bd 17 97 1b c0 17 e8 05 93 2d a3 7e b1 aa b1 a1 1a>,
      _deleteFromDB: false
    },
    _storageTries: { '61de9dc6f6cff1df2809480882cfd3c2364b28f7': [SecureTrie] },
    _cache: Cache {
      _cache: [RedBlackTree],
      _checkpoints: [Array],
      _trie: [SecureTrie]
    },
    _touched: Set {},
    _touchedStack: [ Set {}, Set {} ],
    _checkpointCount: 2,
    _originalStorageCache: Map(0) {},
    _accessedStorage: [ Map(0) {}, Map(0) {}, Map(0) {} ],
    _accessedStorageReverted: [ Map(0) {} ]
  },
  memory: <Buffer 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 ... 46 more bytes>,
  memoryWordCount: <BN: 3>,
  codeAddress: Address {
    buf: <Buffer 61 de 9d c6 f6 cf f1 df 28 09 48 08 82 cf d3 c2 36 4b 28 f7>
  }
}
```
And the goal should be now to transform this into a much better organized data set with only the important data.

