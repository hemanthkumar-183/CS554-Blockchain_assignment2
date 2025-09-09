# CoinToss

## Overview
Single-contract commit–reveal coin toss for India vs England captains with a referee. Commitments hide choices; reveals open them. Optional stake and timeouts included.

## Files
- CoinToss.sol

## Steps (Referee)
1. Deploy contract with referee address.
2. Call `startRound(indiaAddr, englandAddr, commitWindowSec, revealWindowSec, stakeWei)`.

## Steps (Each Captain)
1. Generate salt (32 bytes) and pick choice 0 or 1.
2. Compute commitment: `calcCommitment(myAddress, choice, salt)`.
3. Call `commit(commitment)` before `commitDeadline` (send stake if required).
4. After both committed, call `reveal(choice, salt)` before `revealDeadline`.

## Timeouts and recovery
- If only one committed by commit deadline: committer calls `refundUnmatchedCommit()`.
- If only one revealed by reveal deadline: call `claimTimeoutWin()` to award revealer.
- Use `withdraw()` to pull any pending payouts.

## Notes
- `outcome = indiaChoice XOR englandChoice` (0 => India, 1 => England).
- Use wei for stake and payouts.
