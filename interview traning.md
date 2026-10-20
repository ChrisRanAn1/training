# L1 Code Training Methodology

A systematic training approach for blockchain L1 underlying code and compiler source code, designed to improve code comprehension, problem decomposition, assumption verification, and reasoning articulation skills.

## Training Objectives

Core Competencies:

1. **Code Comprehension**: Quickly understand the architecture and logic of unfamiliar code
2. **Problem Decomposition**: Break down complex problems into verifiable sub-problems
3. **assumption Verification**: Propose assumptions → Design verification method → Draw conclusions
4. **Communication Skills**: Clearly articulate reasoning processes

## Question Design Methodology

### Code Source Definition

Select code from the following categories (300-2000 line scope):

**1. L1 Underlying Code**

**Solana**: `solana-labs/solana`

- Consensus layer: `consensus/` (Tower BFT, fork choice)
- Block production: `poh/`, `banking-stage/`
- Transaction processing: `runtime/src/bank.rs`
- Ledger storage: `ledger/` (blockstore, shreds)
- Network layer: `turbine/`, `gossip/`

**Ethereum**: `ethereum/go-ethereum`

- Consensus engine: `consensus/` (beacon, clique, ethash)
- EVM execution: `core/vm/` (interpreter, jump table, opcodes)
- State management: `core/state/` (state DB, trie)
- Block validation: `core/blockchain.go`, `core/state_processor.go`
- Transaction pool: `core/txpool/`
- P2P network: `p2p/`, `eth/protocols/`

**2. Compiler Source Code**

**Solidity**: `ethereum/solidity`

- Optimizer: `libyul/optimiser/`
- Code generation: `libsolidity/codegen/`
- AST: `libsolidity/ast/`

**RBPF**: `anza-xyz/agave`

- JIT compilation: `program-runtime/src/invoke_context.rs`
- Verifier: `programs/bpf_loader/`

### Question Quality Control

**Standards for High-Quality Questions**

1. **Depth**: Seemingly simple code details have complex design/security considerations behind them
2. **Non-obvious**: Answers cannot be directly observed from the surface of the code; require reasoning
3. **Multi-step Reasoning**: Requires reasoning to reach conclusions
4. **Verifiability**: Can be verified through code/tests/documentation
5. **Practical Significance**: Real engineering considerations, not tricky brain teasers
6. **Domain Knowledge Required**: Understanding of underlying mechanisms (consensus/execution/compilation principles, etc.)

**AI Question Self-Check Process**:

```plain
1. Select code segment
   - Locate specific file and function
   - Confirm scope is within 300-2000 lines. Can span 2-3 files, not too many.
   
2. Identify key details
   - Find a seemingly simple but critical implementation detail
   - This detail must:
     * Be inconspicuous (easily overlooked)
     * Have deep underlying reasons (security/correctness)
     * Allow the counter-question "What if it weren't done this way?"
   
3. Design the question
   - Focus on that specific detail
   - Cannot directly ask "What does this do?" (too shallow)
   - Should ask "Why?" or "What would happen if changed?"
   
4. Self-verification (AI must answer first)
   - Does the answer require reasoning?
   - Does it require examining surrounding code to understand?
   - Does it involve edge cases/state dependencies/concurrency issues?
   - If all answers are "yes", this is a good question
   
   

```

### Question Types (5 Types)

1. **Security Analysis**
   - Question format: "Does this code have security issues? Where? Why?"
   - Focus on: Edge cases, permission checks, state consistency, race conditions

2. **Behavior Prediction**
   - Question format: "What would happen if X is removed/modified? Why?"
   - Focus on: Understanding side effects, execution order, state dependencies

3. **Design Reasoning**
   - Question format: "Why is it implemented this way? What would happen without it? What are the tradeoffs?"
   - Focus on: Architecture understanding, design decisions, tradeoff analysis

4. **Bug Locating**
   - Question format: Given a code version with a real bug, "Where is the problem? How to trigger it?"
   - Focus on: Logic flaws, concurrency issues, edge cases

5. **Optimization Analysis**
   - Question format: "Why is this optimization/transformation correct? Does it change semantics?"
   - Focus on: Equivalence analysis, side effect judgment

### Question Format

```plain
Just give me the specific question description and GitHub code link - concise and efficient
Output questions in English, one at a time
Not too large - cannot exceed 2000 lines
Question markers must be accurate - verify yourself first
Only ask one question at a time; when I answer, only go deep on that question, don't raise new ones.

Example: please explain the security implications of the iteration over function arguments in reverse order here: https://github.com/ethereum/solidity/blob/e0ab0f28206dcd1dfc3bb5e4a54bb6ef929caec5/libyul/optimiser/FullInliner.cpp#L315C98-L315C111. 
    
To be precise, there are security implications if the optimiser can change the execution result if the reverse is removed.
    
If you don't think there are any security implications, explain why.
```
