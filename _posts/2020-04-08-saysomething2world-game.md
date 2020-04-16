---
layout: post
title: "SaySomething2.world - Main contract"
date: 2020-04-08
image: '/assets/img/'
description: 'Details about the main contract.'
tags:
- contract
- web3
- dapp
- ethereum
- blockchain
categories:
- Contract
---

# Main contract

## 1. Address

`0x1234567B172f040f45D7e924C0a7d088016191A6`


## 2. Open-sourced on Github.com

- game.sol<br>
  [https://github.com/PushDotGame/contracts/blob/master/game.sol](https://github.com/PushDotGame/contracts/blob/master/game.sol)
- game.json - ABI<br>
  [https://github.com/PushDotGame/contracts/blob/master/game.json](https://github.com/PushDotGame/contracts/blob/master/game.json)



## 3. Functions for main interaction

### 3-1. Push first message

Push first message, with an adviser address, and become a new player.

```javascript
/**
 * @dev Push with adviser
 *     ____                __
 *    / __ \ __  __ _____ / /_
 *   / /_/ // / / // ___// __ \
 *  / ____// /_/ /(__  )/ / / /
 * /_/     \__,_//____//_/ /_/
 *             _  __   __                  __        _
 *  _      __ (_)/ /_ / /_     ____ _ ____/ /_   __ (_)_____ ___   _____
 * | | /| / // // __// __ \   / __ `// __  /| | / // // ___// _ \ / ___/
 * | |/ |/ // // /_ / / / /  / /_/ // /_/ / | |/ // /(__  )/  __// /
 * |__/|__//_/ \__//_/ /_/   \__,_/ \__,_/  |___//_//____/ \___//_/  >= `0.123`/`0.2` ETH
 */
function pushWithAdviser(bytes memory text, address payable adviser, bytes memory playerName)
    public
    payable
{
    address sender = msg.sender;

    // assert: ether and player
    require(msg.value >= PUSH_COMMAND, "Push: wrong ether amount");
    require(!_player[msg.sender].isPlayer(), "SignUP: sender is a player");
    require(sender.isNotContract(), "SignUP: sender is a contract");

    // auto adviser
    if (adviser == address(0) && msg.value >= AUTO_ADVISER_COMMAND)
    {
        adviser = _latestMessage().account;
    }
    require(_player[adviser].isPlayer(), "SignUP: adviser is not a player");

    block2timestamp[block.number] = block.timestamp;

    _setAdviser(msg.sender, adviser);
    _rename(msg.sender, playerName);

    GameLib.Message memory message = GameLib.Message(msg.sender, text, block.number);
    _doPush(message);
}
```


### 3-2. Only sign-up a new player

Call the `ERC20 - Transfer`

Just transfer 0.123 PUSH to a player's address (as an adviser).

- Add game address `0x1234567B172f040f45D7e924C0a7d088016191A6`
  as a token
- You will find 0.123 PUSH already in your wallet
- Then, just push
- It's free


```javascript
/**
 * @dev Moves `amount` game tokens from the caller's account to `recipient`.
 *
 * Returns a boolean value indicating whether the operation succeeded.
 *
 * Emits a {Transfer} event.
 */
function transfer(address recipient, uint256 amount)
    public
    returns (bool)
{
    _transfer(msg.sender, recipient, amount);
    return true;
}

/**
 * @dev Moves game tokens `amount` from `sender` to `recipient`.
 *
 * When the `amount` meets the `BALANCE_DEFAULT`, make `sender` as a new player.
 * Otherwise refund.
 *
 * Emits a {Transfer} event.
 *
 * Requirements:
 *
 * - `sender` cannot be the zero address.
 * - `recipient` cannot be the zero address.
 * - `sender` must have a balance of at least `amount`.
 */
function _transfer(address sender, address recipient, uint256 amount)
    internal
{
    require(sender != address(0), "ERC20: transfer from the zero address");
    require(recipient != address(0), "ERC20: transfer to the zero address");

    if (
            !_player[sender].isPlayer() &&
            amount == BALANCE_DEFAULT &&
            _player[recipient].isPlayer() &&
            sender.isNotContract()
        )
    {
        _setAdviser(sender.toPayable(), recipient.toPayable());
    }

    else
    {
        emit Transfer(sender, sender, amount);
    }
}

/**
 * @dev Returns the amount of game tokens owned by `account`.
 */
function balanceOf(address account)
    public
    view
    returns (uint256)
{
    if (_player[account].isPlayer())
    {
        return _player[account].messageIds.length.mul(1000);
    }
    return BALANCE_DEFAULT;
}
```

### 3-3. Via the default payable function

Multi-functions depends on the ether transferred.

Code first:

```javascript
/**
 * @dev Accept Ether and Push The Game.
 *     ___                             __     ______ __   __
 *    /   |  _____ _____ ___   ____   / /_   / ____// /_ / /_   ___   _____
 *   / /| | / ___// ___// _ \ / __ \ / __/  / __/  / __// __ \ / _ \ / ___/
 *  / ___ |/ /__ / /__ /  __// /_/ // /_   / /___ / /_ / / / //  __// /
 * /_/  |_|\___/ \___/ \___// .___/ \__/  /_____/ \__//_/ /_/ \___//_/  Push, rename or bid.
 *                         /_/
 */
function ()
    external
    payable
{
    block2timestamp[block.number] = block.timestamp;

    // player push
    if (_player[msg.sender].isPlayer() && msg.value == PUSH_COMMAND)
    {
        GameLib.Message memory message = GameLib.Message(msg.sender, msg.data, block.number);
        _doPush(message);
    }

    // rename
    else if (msg.value == RENAME_COMMAND)
    {
        _rename(msg.sender, msg.data);
    
        _cookieFund = _cookieFund.add(0.002 ether);
        _devFund.transfer(0.001 ether);
        _pushToShareholders(0.002 ether);
    }

    // shareholder add weis
    else if (_getShareholderPosition(msg.sender) > 0 && msg.value >= SHAREHOLDER_STEP)
    {
        _shareholderAddWeis();
    }

    // bid for shareholder
    else if (msg.value >= _shareholder[_shareholders[SHAREHOLDER_MAX_POSITION]].stakingWeis.add(SHAREHOLDER_STEP))
    {
        _bidForShareholder();
    }

    // sign-up with auto adviser, then push
    else if (!_player[msg.sender].isPlayer() && msg.value >= AUTO_ADVISER_COMMAND)
    {
        _setAdviser(msg.sender, _latestMessage().account);

        GameLib.Message memory message = GameLib.Message(msg.sender, msg.data, block.number);
        _doPush(message);
    }

    // other amount of ether
    else
    {
        revert("Push: wrong ether amount");
    }
}
```

#### 3-3-1. Push a message with 0.123 ETH

```javascript
// player push
if (_player[msg.sender].isPlayer() && msg.value == PUSH_COMMAND)
{
    GameLib.Message memory message = GameLib.Message(msg.sender, msg.data, block.number);
    _doPush(message);
}
```

#### 3-3-2. Rename with 0.01 ETH

```javascript
// rename
else if (msg.value == RENAME_COMMAND)
{
    _rename(msg.sender, msg.data);

    _cookieFund = _cookieFund.add(0.002 ether);
    _devFund.transfer(0.001 ether);
    _pushToShareholders(0.002 ether);
}
```

#### 3-3-3. Bid for a shareholder

```javascript
// shareholder add weis
else if (_getShareholderPosition(msg.sender) > 0 && msg.value >= SHAREHOLDER_STEP)
{
    _shareholderAddWeis();
}

// bid for shareholder
else if (msg.value >= _shareholder[_shareholders[SHAREHOLDER_MAX_POSITION]].stakingWeis.add(SHAREHOLDER_STEP))
{
    _bidForShareholder();
}
```

#### 3-3-4. Push message, with auto adviser

```javascript
// sign-up with auto adviser, then push
else if (!_player[msg.sender].isPlayer() && msg.value >= AUTO_ADVISER_COMMAND)
{
    _setAdviser(msg.sender, _latestMessage().account);

    GameLib.Message memory message = GameLib.Message(msg.sender, msg.data, block.number);
    _doPush(message);
}
```

### 3-4. Pin a message, or unpin

```javascript
/**
 * @dev Pin a message.
 *     ____   _           ___       __  ___
 *    / __ \ (_)____     /   |     /  |/  /___   _____ _____ ____ _ ____ _ ___
 *   / /_/ // // __ \   / /| |    / /|_/ // _ \ / ___// ___// __ `// __ `// _ \
 *  / ____// // / / /  / ___ |   / /  / //  __/(__  )(__  )/ /_/ // /_/ //  __/
 * /_/    /_//_/ /_/  /_/  |_|  /_/  /_/ \___//____//____/ \__,_/ \__, / \___/
 *                                                               /____/
 */
function pin(uint256 serial)
    public
{
    require(serial > 0, "Pin: message serial is zero");
    require(serial <= _message.length, "Pin: message serial number exceeded");
    require(_message[serial - 1].account == msg.sender, "Pin: not player's message");

    _player[msg.sender].pinnedMessageSerial = serial;
}

/**
 * @dev Unpin.
 */
function unpin()
    public
{
    delete _player[msg.sender].pinnedMessageSerial;
}
```


## 4. Functions for read

### 4-1. Get game status

Returns the basic status of the game

```javascript
/**
 * @dev Returns game status.
 */
function getStatus()
    public
    view
    returns (
        uint256 timer,
        uint256 roundCounter,
        uint256 playerCounter,
        uint256 messageCounter,
        uint256 cookieCounter,
        
        uint256 cookieFund,
        uint256 winnerFund,

        uint256 surpriseIssued,
        uint256 bonusIssued,
        uint256 cookieIssued,
        uint256 shareholderIssued
    )
```

