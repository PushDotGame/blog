---
layout: post
title: "SaySomething2.world - Reader A/B contract"
date: 2020-04-08
image: '/assets/img/'
description: 'Details about the reader A/B contract.'
tags:
- contract
- web3
- dapp
- ethereum
- blockchain
categories:
- Contract
---

# Reader A/B contract


## Address

- **Reader A**<br>
  `0x123A1eef39875D6009F66211d80E726067Ae74b6`
- **Reader B**<br>
  `0x123B2B91523F90F69C60a93E1161e7f1783C06a8`


## Open-sourced on Github.com

### Reader A

- reader_a.sol
  [https://github.com/PushDotGame/contracts/blob/master/reader_a.sol](https://github.com/PushDotGame/contracts/blob/master/reader_a.sol)
- reader_a.json - ABI
  [https://github.com/PushDotGame/contracts/blob/master/reader_a.json](https://github.com/PushDotGame/contracts/blob/master/reader_a.json)

### Reader B

- reader_b.sol
  [https://github.com/PushDotGame/contracts/blob/master/reader_b.sol](https://github.com/PushDotGame/contracts/blob/master/reader_b.sol)
- reader_b.json - ABI
  [https://github.com/PushDotGame/contracts/blob/master/reader_b.json](https://github.com/PushDotGame/contracts/blob/master/reader_b.json)



## Reader A: about game, exclude player

### Round

```javascript
/**
 * @dev Returns round info of `roundSerial`.
 *
 * roundSerial > 0: history
 * roundSerial == 0: latest
 */
function round(uint256 roundSerial)
    public
    view
    returns (
        uint256 serial,

        uint256 openedBlock,
        uint256 openedTimestamp,
        uint256 closingBlock,
        uint256 closingTimestamp,
        uint256 closedTimestamp,
        uint256 openingWinnerFund,
        uint256 closingWinnerFund,
        address opener,
        bytes memory openerName,
        uint256 openerBonusWeis
    )
```


### Round winners

```javascript
/**
 * @dev Returns round winners @ `roundSerial`.
 */
function roundWinners(uint256 roundSerial)
    public
    view
    returns (
        uint8[WINNERS_PER_ROUND] memory winnerSerials,
        uint256[WINNERS_PER_ROUND] memory messageSerials,
        address[WINNERS_PER_ROUND] memory players,
        bytes[WINNERS_PER_ROUND] memory playerNames,
        uint256[WINNERS_PER_ROUND] memory bonusWeis,
        bytes[WINNERS_PER_ROUND] memory texts,
        uint256[WINNERS_PER_ROUND] memory blockNumbers,
        uint256[WINNERS_PER_ROUND] memory timestamps
    )
```


### Message

```javascript
/**
 * @dev Returns a message info @ global `serial`.
 */
function message(uint256 serial)
    public
    view
    returns (
        address account,
        bytes memory name,
        bytes memory text,
        uint256 blockNumber,
        uint256 timestamp
    )
```


### Messages

```javascript
/**
 * @dev Returns game messages, `till` a serial.
 *
 * till > 0: history
 * till == 0: latest
 */
function messages(uint256 till)
    public
    view
    returns (
        uint256[LG_PAGE] memory serials,
        address[LG_PAGE] memory accounts,
        bytes[LG_PAGE] memory names,
        bytes[LG_PAGE] memory texts,
        uint256[LG_PAGE] memory blockNumbers,
        uint256[LG_PAGE] memory timestamps
    )
```


### Lucky cookies

```javascript
/**
 * @dev Returns game cookies, `till` a serial.
 *
 * till > 0: history
 * till == 0: latest
 */
function cookies(uint256 till)
    public
    view
    returns (
        uint256[LG_PAGE] memory serials,

        address[LG_PAGE] memory players,
        address[LG_PAGE] memory advisers,
        bytes[LG_PAGE] memory playerNames,
        bytes[LG_PAGE] memory adviserNames,
        uint256[LG_PAGE] memory playerWeis,
        uint256[LG_PAGE] memory adviserWeis,

        uint256[LG_PAGE] memory messageSerials,
        bytes[LG_PAGE] memory texts,
        uint256[LG_PAGE] memory blockNumbers,
        uint256[LG_PAGE] memory timestamps
    )
```


### Top-players

```javascript
/**
 * @dev Returns top-players.
 */
function topPlayers()
    public
    view
    returns (
        address payable[TOP_PLAYER_MAX_POSITION] memory accounts,
        bytes[TOP_PLAYER_MAX_POSITION] memory names,
        uint256[TOP_PLAYER_MAX_POSITION] memory messageSerials,
        uint256[TOP_PLAYER_MAX_POSITION] memory messageCounters,
        bytes[TOP_PLAYER_MAX_POSITION] memory texts,
        uint256[TOP_PLAYER_MAX_POSITION] memory blockNumbers,
        uint256[TOP_PLAYER_MAX_POSITION] memory timestamps
    )
```


### Shareholders

```javascript
/**
 * @dev Returns shareholders.
 */
function shareholders()
    public
    view
    returns (
        uint256 bidCounter,
    
        address payable[SHAREHOLDER_MAX_POSITION] memory accounts,
        bytes[SHAREHOLDER_MAX_POSITION] memory names,
        uint256[SHAREHOLDER_MAX_POSITION] memory stakingWeis,
        uint256[SHAREHOLDER_MAX_POSITION] memory profitWeis,
        uint256[SHAREHOLDER_MAX_POSITION] memory firstBlockNumbers,
        uint256[SHAREHOLDER_MAX_POSITION] memory firstTimestamps,

        uint256[SHAREHOLDER_MAX_POSITION] memory messageCounters,
        uint256[SHAREHOLDER_MAX_POSITION] memory messageSerials,
        bytes[SHAREHOLDER_MAX_POSITION] memory texts,
        uint256[SHAREHOLDER_MAX_POSITION] memory blockNumbers,
        uint256[SHAREHOLDER_MAX_POSITION] memory timestamps
    )
```

### Shareholders bids log

```javascript
/**
 * @dev Returns shareholder bid logs, `till` a serial.
 *
 * till > 0: history
 * till == 0: latest
 */
function shareholderBids(uint256 till)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,

        address[SM_PAGE] memory accounts,
        bytes[SM_PAGE] memory names,
        uint256[SM_PAGE] memory beforeWeis,
        uint256[SM_PAGE] memory afterWeis,
        uint256[SM_PAGE] memory blockNumbers,
        uint256[SM_PAGE] memory timestamps
    )
```


## Reader-B: only about player

### Player

```javascript
/**
 * @dev Returns player info of `account`.
 */
function player(address account)
    public
    view
    returns (
        uint256 serial,
        bytes memory name,
        bytes memory adviserName,
        address payable adviser,
        uint256 messageCounter,
        uint256 cookieCounter,
        uint256 followerCounter,
        uint256 followerMessageCounter,
        uint256 followerCookieCounter,
        uint256 bonusWeis,
        uint256 surpriseWeis,
        uint256 firstMessageTimestamp
    )
```

or by id:

```javascript
/**
 * @dev Returns player info @ `playerSerial`.
 */
function playerAt(uint256 playerSerial)
    public
    view
    returns (
        bytes memory name,
        bytes memory adviserName,
        address payable account,
        address payable adviser,
        uint256 messageCounter,
        uint256 cookieCounter,
        uint256 followerCounter,
        uint256 followerMessageCounter,
        uint256 followerCookieCounter,
        uint256 bonusWeis,
        uint256 surpriseWeis,
        uint256 firstMessageTimestamp
    )
```


### Player prime info

```javascript
/**
 * Return player top-player and shareholder position, of `account`.
 */
function playerPrime(address account)
    public
    view
    returns (
        uint256 pinnedMessageSerial,

        uint256 firstMessageBlockNumber,
        uint256 firstMessageTimestamp,

        uint8 topPlayerPosition,

        uint8 shareholderPosition,
        uint256 shareholderStakingWeis,
        uint256 shareholderProfitWeis,
        uint256 shareholderFirstBlockNumber,
        uint256 shareholderFirstTimestamp
    )
```


### Player messages

```javascript
/**
 * @dev Returns messages of `account`, `till` a serial.
 *
 * till > 0: history
 * till == 0: latest
 */
function playerMessages(address account, uint256 till)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,

        uint256[SM_PAGE] memory messageSerials,
        bytes[SM_PAGE] memory texts,
        uint256[SM_PAGE] memory blockNumbers,
        uint256[SM_PAGE] memory timestamps
    )
```


### Player lucky-cookies

```javascript
/**
 * @dev Returns cookies of `account`, `till` a serial.
 *
 * till > 0: history
 * till == 0: latest
 */
function playerCookies(address account, uint256 till)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory advisers,
        bytes[SM_PAGE] memory adviserNames,
        uint256[SM_PAGE] memory playerWeis,
        uint256[SM_PAGE] memory adviserWeis,

        uint256[SM_PAGE] memory messageSerials,
        bytes[SM_PAGE] memory texts,
        uint256[SM_PAGE] memory blockNumbers,
        uint256[SM_PAGE] memory timestamps
    )
```


### Player follower lucky-cookies

```javascript
/**
 * @dev Returns follower-cookies of `account`, `till` a serial.
 *
 * till > 0: history
 * till == 0: latest
 */
function playerFollowerCookies(address account, uint256 till)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory players,
        bytes[SM_PAGE] memory playerNames,
        uint256[SM_PAGE] memory playerWeis,
        uint256[SM_PAGE] memory adviserWeis,

        uint256[SM_PAGE] memory messageSerials,
        bytes[SM_PAGE] memory texts,
        uint256[SM_PAGE] memory blockNumbers,
        uint256[SM_PAGE] memory timestamps
    )
```


### Player followers

```javascript
/**
 * @dev Returns followers of `account`, `till` a serial.
 *
 * till > 0: history
 * till == 0: latest
 */
function playerFollowersA(address account, uint256 till)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory playerSerials,
        bytes[SM_PAGE] memory names,
        bytes[SM_PAGE] memory adviserNames,
        address[SM_PAGE] memory advisers,
        uint256[SM_PAGE] memory bonusWeis,
        uint256[SM_PAGE] memory surpriseWeis,

        uint256[SM_PAGE] memory messageSerials,
        uint256[SM_PAGE] memory messageBlockNumbers,
        uint256[SM_PAGE] memory messageTimestamps,
        bytes[SM_PAGE] memory messageTexts
    )
```

and part B:

```javascript
function playerFollowersB(address account, uint256 till)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,

        uint256[SM_PAGE] memory messageCounters,
        uint256[SM_PAGE] memory cookieCounters,
        uint256[SM_PAGE] memory followerCounters,
        uint256[SM_PAGE] memory followerMessageCounters,
        uint256[SM_PAGE] memory followerCookieCounters,

        uint256[SM_PAGE] memory firstMessageBlockNumbers,
        uint256[SM_PAGE] memory firstMessageTimestamps
    )
```

and part C:

```javascript
function playerFollowersC(address account, uint256 till)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory pinnedMessageSerials,

        uint8[SM_PAGE] memory topPlayerPositions,
        uint8[SM_PAGE] memory shareholderPositions,
        uint256[SM_PAGE] memory shareholderStakingWeis,
        uint256[SM_PAGE] memory shareholderProfitWeis,
        uint256[SM_PAGE] memory shareholderFirstBlockNumbers,
        uint256[SM_PAGE] memory shareholderFirstTimestamps
    )
```


### Player advisers

```javascript
/**
 * @dev Returns advisers of `account`.
 */
function playerAdvisersA(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory playerSerials,
        bytes[SM_PAGE] memory names,
        bytes[SM_PAGE] memory adviserNames,
        address[SM_PAGE] memory advisers,
        uint256[SM_PAGE] memory bonusWeis,
        uint256[SM_PAGE] memory surpriseWeis,

        uint256[SM_PAGE] memory messageSerials,
        uint256[SM_PAGE] memory messageBlockNumbers,
        uint256[SM_PAGE] memory messageTimestamps,
        bytes[SM_PAGE] memory messageTexts
    )
```

and part B:

```javascript
function playerAdvisersB(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,

        uint256[SM_PAGE] memory messageCounters,
        uint256[SM_PAGE] memory cookieCounters,
        uint256[SM_PAGE] memory followerCounters,
        uint256[SM_PAGE] memory followerMessageCounters,
        uint256[SM_PAGE] memory followerCookieCounters,

        uint256[SM_PAGE] memory firstMessageBlockNumbers,
        uint256[SM_PAGE] memory firstMessageTimestamps
    )
```

and part C:

```javascript
function playerAdvisersC(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory pinnedMessageSerials,

        uint8[SM_PAGE] memory topPlayerPositions,
        uint8[SM_PAGE] memory shareholderPositions,
        uint256[SM_PAGE] memory shareholderStakingWeis,
        uint256[SM_PAGE] memory shareholderProfitWeis,
        uint256[SM_PAGE] memory shareholderFirstBlockNumbers,
        uint256[SM_PAGE] memory shareholderFirstTimestamps
    )
```


### Player previous

```javascript
/**
 * @dev Returns previous of `account`.
 */
function playerPreviousA(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory playerSerials,
        bytes[SM_PAGE] memory names,
        bytes[SM_PAGE] memory adviserNames,
        address[SM_PAGE] memory advisers,
        uint256[SM_PAGE] memory bonusWeis,
        uint256[SM_PAGE] memory surpriseWeis,

        uint256[SM_PAGE] memory messageSerials,
        uint256[SM_PAGE] memory messageBlockNumbers,
        uint256[SM_PAGE] memory messageTimestamps,
        bytes[SM_PAGE] memory messageTexts
    )
```

and part B:

```javascript
function playerPreviousB(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,

        uint256[SM_PAGE] memory messageCounters,
        uint256[SM_PAGE] memory cookieCounters,
        uint256[SM_PAGE] memory followerCounters,
        uint256[SM_PAGE] memory followerMessageCounters,
        uint256[SM_PAGE] memory followerCookieCounters,

        uint256[SM_PAGE] memory firstMessageBlockNumbers,
        uint256[SM_PAGE] memory firstMessageTimestamps
    )
```

and part C:

```javascript
function playerPreviousC(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory pinnedMessageSerials,

        uint8[SM_PAGE] memory topPlayerPositions,
        uint8[SM_PAGE] memory shareholderPositions,
        uint256[SM_PAGE] memory shareholderStakingWeis,
        uint256[SM_PAGE] memory shareholderProfitWeis,
        uint256[SM_PAGE] memory shareholderFirstBlockNumbers,
        uint256[SM_PAGE] memory shareholderFirstTimestamps
    )
```

### Player next

```javascript
/**
 * @dev Returns next of `account`.
 */
function playerNextA(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory playerSerials,
        bytes[SM_PAGE] memory names,
        bytes[SM_PAGE] memory adviserNames,
        address[SM_PAGE] memory advisers,
        uint256[SM_PAGE] memory bonusWeis,
        uint256[SM_PAGE] memory surpriseWeis,

        uint256[SM_PAGE] memory messageSerials,
        uint256[SM_PAGE] memory messageBlockNumbers,
        uint256[SM_PAGE] memory messageTimestamps,
        bytes[SM_PAGE] memory messageTexts
    )
```

and part B:

```javascript
function playerNextB(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,

        uint256[SM_PAGE] memory messageCounters,
        uint256[SM_PAGE] memory cookieCounters,
        uint256[SM_PAGE] memory followerCounters,
        uint256[SM_PAGE] memory followerMessageCounters,
        uint256[SM_PAGE] memory followerCookieCounters,

        uint256[SM_PAGE] memory firstMessageBlockNumbers,
        uint256[SM_PAGE] memory firstMessageTimestamps
    )
```

and part C:

```javascript
function playerNextC(address account)
    public
    view
    returns (
        uint256[SM_PAGE] memory serials,
        address[SM_PAGE] memory accounts,
        
        uint256[SM_PAGE] memory pinnedMessageSerials,

        uint8[SM_PAGE] memory topPlayerPositions,
        uint8[SM_PAGE] memory shareholderPositions,
        uint256[SM_PAGE] memory shareholderStakingWeis,
        uint256[SM_PAGE] memory shareholderProfitWeis,
        uint256[SM_PAGE] memory shareholderFirstBlockNumbers,
        uint256[SM_PAGE] memory shareholderFirstTimestamps
    )
```
