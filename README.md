# Siblings-Solidity-Styling-Guide
This is the styling guide used by Sibling Labs

## Introduction

The Sibling Labs Solidity Style guide is based on Ethereum's original solidity style guide: https://github.com/ethereum/solidity/blob/v0.5.1/docs/style-guide.rst. There is a significant overlap between conventions but with minor adjustments. 

This guide's intended purpose is to provide the convention for writing solidity code. This guide is an evolving document and will incorporate future advancements in the language, improvements to the readability of contracts, and phase out outdated conventions.

## Code Layout

#### Indentation
Use 1 tab space per indentation level.

#### Tabs or Spaces
Tabs are the preferred indentation method.
Mixing tabs and spaces should be avoided.

#### Multiple Contracts and Modules 
Each smart contract and/or Smart Module should be given it's own file.

#### Maximum Line Length
The maximum length of a line of code should be less than or equal to 120 characters, including indentations.
Child contracts that are overriding a parent function that exceeds the length limit should be reformatted to the standard of this document.

#### Comments
The maximum length of a line of comments should be less than or equal to 65 characters, including indentations.

Comments should use multi-line comment syntax if they span multiple lines.

NATSPEC comments should be indented when spanning multiple lines.
Example:

```
/**
 * @dev Begin the sale. The sale period will automatically
 *      elapse and conclude.
 */
function beginPrivSale() {
    ...
}
```

The purpose of the maximum line length rule is to make your code readable. Developers are encouraged to use their best judgement for line length, but try to adhere to this maximum.

#### Wrapped Lines
Wrapped lines should conform to the following guidelines.

1. The first argument should not be attached to the opening parenthesis.
2. One, and only one, indent should be used.
3. Each argument should fall on its own line.
4. The terminating element, );, should be placed on the final line by itself.

## Function Styling

#### Function Calls

Yes:
```
thisFunctionCallIsReallyLong(
    longArgument1,
    longArgument2,
    longArgument3
);

shortFunctionCall(arg1, arg2, arg3);
```

No:
```
thisFunctionCallIsReallyLong(longArgument1,
                              longArgument2,
                              longArgument3
);

thisFunctionCallIsReallyLong(longArgument1,
    longArgument2,
    longArgument3
);

thisFunctionCallIsReallyLong(
    longArgument1, longArgument2,
    longArgument3
);

thisFunctionCallIsReallyLong(
longArgument1,
longArgument2,
longArgument3
);

thisFunctionCallIsReallyLong(
    longArgument1,
    longArgument2,
    longArgument3);
```

#### Assignment Statements

Yes:

```
thisIsALongNestedMapping[being][set][toSomeValue] = someFunction(
    argument1,
    argument2,
    argument3,
    argument4
);
```

No:

```
thisIsALongNestedMapping[being][set][toSomeValue] = someFunction(argument1,
                                                                   argument2,
                                                                   argument3,
                                                                   argument4);
```

#### Event Definitions and Event Emitters

Yes:

```
event ShortOneArg(address _sender);

event LongAndLotsOfArgs(
    address _sender,
    address _recipient,
    uint256 _publicKey,
    uint256 _amount,
    bytes32[] _options
);

emit LongAndLotsOfArgs(
    sender,
    recipient,
    publicKey,
    amount,
    options
);

emit ShortOneArg(sender);
```

No:

```
event ShortOneArg(
    address _sender
);

event LongAndLotsOfArgs(address _sender,
                        address _recipient,
                        uint256 _publicKey,
                        uint256 _amount,
                        bytes32[] _options);

emit LongAndLotsOfArgs(sender,
                  recipient,
                  publicKey,
                  amount,
                  options);
```

## Source File Encoding
UTF-8 or ASCII encoding is preferred.

## SPDX License Identifier
The SPDX License Identifier should always be on the first line of the file. Nothing else should go on the first line.

Use the MIT License unless otherwise specified for a particular project.

## Pragma Statement
The pragma statement should always be on the second line of the file. Nothing else should go on the second line.

Use `pragma solidity ^0.8.0;` unless a higher version is required, or if otherwise specified.

## Imports
Import statements should be placed above the contracts and interfaces in a file, but below the SPDX License Identifier and pragma statement.

## Order of Functions
Functions must be grouped according to their visibility and ordered:

- constructor
- receive function (if exists)
- fallback function (if exists)
- external
- public
- internal
- private

Within a grouping, place the ```view``` and ```pure``` functions last.

Yes:

```
pragma solidity >=0.8.0 <0.9.0;

contract A
{
    uint256 someVariable;

    event SomeEvent(
        uint256 _arg1
    );

    constructor() public {
        // ...
    }

    modifier SomeModifier(uint256 _arg1) {
        // some check
        _;
    }

    function() external {
        // ...
    }

    // External functions
    // ...

    // External functions that are view
    // ...

    // External functions that are pure
    // ...

    // Public functions
    // ...

    // Internal functions
    // ...

    // Private functions
    // ...
}
```

No:

```
pragma solidity >=0.8.0 <0.9.0;

contract A
{

  // External functions
  // ...

  function() 
    external 
  {
      // ...
  }

  // Private functions
  // ...

  // Public functions
  // ...

  modifier SomeModifier(uint256 _arg1) {
    // some check
    _;
  }

  constructor()
    public 
  {
      // ...
  }

  // Internal functions
  // ...
}
```

In addition to the above rule, functions should be categorised and separated by subheadings within the contract.

Subheadings include (in this order):
- Admin functions
- Public functions
- Misc functions

Admin functions subheading:
Functions categorised here are functions which are public or external, and intended to be used by Admin users of the smart contract.
e.g. `pause`, `setPrice`, `beginSale`

Public functions subheading:
Functions categorised here are functions which are public or external, and intended to be used by end users of the smart contract (e.g. token minters).
e.g. `mint`, `burn`

Misc functions subheading:
Functions categorised here are functions which are internal or private, OR intended to be called by another smart contract, or external service (such as NFT marketplaces).
e.g. external functions such as `supportsInterface`, `tokenURI`, or internal functions such as `exists`, `getPackedOwnershipData`

These categories should be separated by a commented subheading in this format:

// ADMIN FUNCTIONS //

// PUBLIC FUNCTIONS //

// MISC FUNCTIONS //

Constructors, enums, global variables, and structs are excluded from these categories and go at the top of the contract above the subheadings.

## Whitespace in Expressions

Avoid extraneous whitespace in the following situations:

Yes:

```boil(egg[1], Egg({ name: "egg" }));```

No:

```boil( egg[ 1 ], Egg( { name: "egg" } ) );```

Immediately before a comma, semicolon:

Yes:

```function boil(uint256 i, Egg egg) public;```

No:

```function boil(uint256 i , Egg egg) public ;```

More than one space around an assignment or other operator to align with another:

Yes
```
x = 1;
y = 2;
long_variable = 3;
```

No
```
x             = 1;
y             = 2;
long_variable = 3;
```

## Control Structures

The braces denoting the body of a contract, library, functions and structs should:

- open on the same line as the declaration
- close on their own line at the same indentation level as the beginning of the declaration.
- The opening brace should be proceeded by a single space.

Yes:

```
contract Coin {
    struct Bank {
        address owner;
        uint256 balance;
    }
}
```

No:

```
contract Coin
{
    struct Bank {
        address owner;
        uint256 balance;
    }
}
```

No:

```
contract Coin
{
    struct Bank 
    {
        address owner;
        uint256 balance;
    }
}
```

When declaring short functions with a single statement, it is permissible to do it on a single line and have a single space separating the curly brackets when there is a semicolon present.

Permissible:

```function shortFunction() public { doSomething(); }```

These guidelines for function declarations are intended to improve readability. Authors should use their best judgement as this guide does not try to cover all possible permutations for function declarations.

The structure remains similar for the control structures ```if```, ```else```, ```while```, ```constructor```, and ```for```.

Additionally there should be a single space between the control structures if, while, and for and the parenthetic block representing the conditional, as well as a single space between the conditional parenthetic block and the opening brace.

Yes:
```
if (...) {
  ...
}

while(...){
}

for (...) {
  ...;}
```

No:
```
if (...) 
{
  ...
}

for (...) 
{
  ...
}
```

For control structures whose body contains a single statement, omitting the braces is acceptable if it improves the readability of the code.

Yes
```
if (x < 10) x += 1;

```
No
```
if (x < 10)
    x += 1;

if (x < 10)
    someArray.push(Coin({
        name: 'spam',
        value: 42
    }));
```

For if blocks which have an else or else if clause, the else must be placed on the same line as the if's closing brace. This is an exception compared to the rules of other block-like structures.

Yes
```
if (x < 3) {
    x += 1;
} else {
    x -= 1;
}

```

No
```
if (x < 3) 
{
    x += 1;
} 
else if (x > 7) 
{
    x -= 1;
} 
else 
{
    x = 5;
}

if (x < 3) 
{
    x += 1;
}
else 
{
    x -= 1;
}
```

## Function Declaration

For every function declarations, it is recommended to drop each argument onto it's own line at the same indentation level as the function body only if it exceeds the maximum line length. In this scenario, the closing parenthesis and opening bracket must be placed on their own line as well at the same indentation level as the function declaration.

Yes
```
function thisFunctionHasNoArguments() public {
  doSomething();
}

function thisFunctionHasAnArgument(address _a) public {
  doSomething();
}

function thisFunctionHasLotsOfArguments(
    address _a,
    address _b,
    address _c,
    address _d,
    address _e,
    address _f
)
    public
{
    doSomething();
}

function thisFunctionHasALotOfModifiersAndSpecifiers(address _a, address _b)
    adminOnly
    isActive
    external
{
    doSomething();
}
```

No
```
function thisFunctionHasNoArguments() public
{
    doSomething();
}

function thisFunctionHasAnArgument(address _a) public {
    doSomething();
}

function thisFunctionHasLotsOfArguments(address _a, address _b, address _c,
    address _d, address _e, address _f) public {
    doSomething();
}

function thisFunctionHasLotsOfArguments(address _a,
                                        address _b,
                                        address _c,
                                        address _d,
                                        address _e,
                                        address _f) public {
    doSomething();
}

function thisFunctionHasLotsOfArguments(
    address _a,
    address _b,
    address _c,
    address _d,
    address _e,
    address _f) public {
    doSomething();
}
```

Multiline output parameters and return statements must follow the same style.

Yes
```
function thisFunctionNameIsReallyLong(
    address _a,
    address _b,
    address _c
)
    public
    returns (
        address someAddressName,
        uint256 LongArgument,
        uint256 Argument
    )
{
    doSomething()

    return (
        veryLongReturnArg1,
        veryLongReturnArg2,
        veryLongReturnArg3
    );
}
```

No
```
function thisFunctionNameIsReallyLong(
    address _a,
    address _b,
    address _c
)
    public
    returns (address someAddressName,
             uint256 LongArgument,
             uint256 Argument)
{
    doSomething()

    return (veryLongReturnArg1,
            veryLongReturnArg1,
            veryLongReturnArg1);
}
```

For constructor functions on inherited contracts whose bases require arguments, it is recommended to drop the base constructors onto new lines in the same manner as modifiers if the function declaration is long or hard to read.

Yes:
```
pragma solidity >=0.8.0 <0.9.0;

// Base contracts just to make this compile
contract B {
    constructor(uint) public {}
}
contract C {
    constructor(uint, uint) public {}
}
contract D {
    constructor(uint) public {}
}

contract A is B, C, D {
    uint256 x;

    constructor(uint256 param1, uint256 param2, uint256 param3, uint256 param4, uint256 param5)
        B(param1)
        C(param2, param3)
        D(param4)
        public
    {
        // do something with param5
        x = param5;
    }
}
```

No:
```
pragma solidity >=0.8.0 <0.9.0;

// Base contracts just to make this compile
contract B {
    constructor(uint) public {
    }
}
contract C {
    constructor(uint, uint) public {
    }
}
contract D {
    constructor(uint) public {
    }
}

contract A is B, C, D {
    uint256 x;

    constructor(uint256 param1, uint256 param2, uint256 param3, uint256 param4, uint256 param5)
    B(param1)
    C(param2, param3)
    D(param4)
    public
    {
        x = param5;
    }
}

contract X is B, C, D {
    uint256 x;

    constructor(uint256 param1, uint256 param2, uint256 param3, uint256 param4, uint256 param5)
        B(param1)
        C(param2, param3)
        D(param4)
        public {
        x = param5;
    }
}
```

## Mappings
In variable declarations, do not separate the keyword ```mapping``` from its type by a space. Do not separate any nested ```mapping``` keyword from its type by whitespace.

Yes
```
mapping(uint256 => uint) map;
mapping(address => bool) registeredAddresses;
mapping(uint256 => mapping(bool => Data[])) public data;
mapping(uint256 => mapping(uint256 => s)) data;
```
No
```
mapping (uint256 => uint) map;
mapping( address => bool ) registeredAddresses;
mapping (uint256 => mapping (bool => Data[])) public data;
mapping(uint256 => mapping (uint256 => s)) data;
```

## Variable Declarations
Declarations of array variables must not have a space between the type and the brackets.

Yes
```uint[] x;```
No
```uint256 [] x;```

## Other Recommendations
Strings must be quoted with double-quotes instead of single-quotes.

Yes
```
str = "foo";
str = "Hamlet says, 'To be or not to be...'";
```
No
```
str = 'bar';
str = '"Be yourself; everyone else is already taken." -Oscar Wilde';
```

Surround operators with a single space on either side.


Yes
```
x = 3;
x = 100 / 10;
x += 3 + 4;
x |= y && z;
```
No
```
x=3;
x = 100/10;
x += 3+4;
x |= y&&z;
```

## Order of Layout
Layout contract elements in the following order:

- SPDX License Identifier
- Pragma statements
- ASCII Art
- Import statements
- Interfaces
- Libraries
- Contract

Inside each contract use the following order:

Library declarations (using statements)
Constant variables
Type declarations
State variables
Events
Modifiers
Functions

Note:
It might be clearer to declare types close to their use in events or state variables.

Naming Styles
To avoid confusion, the following names will be used to refer to different naming styles.

```b``` (single lowercase letter)
```B``` (single uppercase letter)
```lowercase```
```lower_case_with_underscores```
```UPPERCASE```
```UPPER_CASE_WITH_UNDERSCORES```
```CapitalizedWords``` (or CapWords)
```mixedCase``` (differs from CapitalizedWords by initial lowercase character!)
```Capitalized_Words_With_Underscores```
```_underscoreMixedCase```

When using initialisms in CapWords, capitalize all the letters of the initialisms. Thus ERC20Address is better than Erc20Adress similiarly to HTTPServerError compared to HttpServerError. When using initialisms is mixedCase, capitalize all the letters of the initialisms, except keep the first one lower case if it is the beginning of the name. Thus ashERC20Address is better than ASHERC20Address similiarly to xmlHTTPRequest to XMLHTTPRequest.

## Names to Avoid
```l``` - Lowercase letter el
```O``` - Uppercase letter oh
```I``` - Uppercase letter eye
Never use any of these for single letter variable names. They are often indistinguishable from the numerals one and zero.

## Contract and Library Names
- Contracts and libraries must be named using the CapWords style. Examples: ```SimpleToken```, ```SmartBank```, ```CertificateHashRepository```, ```Player```, ```Starlings```, ```Owned```.
As shown in the example below, if the contract name is ```Starlings``` and the library name is ```Owned```, then their associated filenames should be ```Starlings.sol``` and ```Owned.sol```.

Yes:

```
pragma solidity >=0.8.0 <0.9.0;

// Owned.sol
contract Owned {
     address public owner;

     constructor() public {
         owner = msg.sender;
     }

     modifier onlyOwner {
         require(msg.sender == owner);
         _;
     }

     function transferOwnership(address newOwner) public onlyOwner {
         owner = newOwner;
     }
}

// Starlings.sol
import "./Starlings.sol";

contract Starlings is Owned, TokenRecipient {
    //...
}
```

No:
```
pragma solidity >=0.4.0 <0.6.0;

// owned.sol
contract owned {
     address public owner;

     constructor() public {
         owner = msg.sender;
     }

     modifier onlyOwner {
         require(msg.sender == owner);
         _;
     }

     function transferOwnership(address newOwner) public onlyOwner {
         owner = newOwner;
     }
}

// Starlings.sol
import "./owned.sol";

contract Starlings is owned, tokenRecipient {
    //...
}
```
## uints
Always specify the type of ```unit``` that is being used. Examples ```uint256```, ```uint8```, etc.

## Struct Names
Structs must be named using the CapWords style. Examples: ```MyCoin```, ```Position```, ```PositionXY```.

## Event Names
Events must be named using the CapWords style. Examples: ```Deposit```, ```Transfer```, ```Approval```, ```BeforeTransfer```, ```AfterTransfer```.

## Function Names
Functions other than constructors must use mixedCase are public. Examples: ```safeMint```, ```getBalance```, ```transfer```, ```verifyOwner```, ```addMember```, ```changeOwner```. If a function is private or internal and has a namesake function it should use _underscoreMixedCase. Examples: ```burn / _burn``` , ```beforeTokenTransfer / _beforeTokenTransfer```.

Setter functions which update boolean variables should be named like this:
`toggleSaleState`, with ‘toggle’ as the prefix

Setter functions which update variables of any type other than boolean should be named like this:
`setPrices`, with ‘set’ as the prefix

## Function Argument Names
Function arguments should use _underscoreMixedCase if there is a similarly named, globally variable in the contract. Examples: ```_price```, ```_account```, ```_owner```.

## Local and State Variable Names
Use mixedCase. Examples: ```totalSupply```, ```remainingSupply```, ```balancesOf```, ```creatorAddress```, ```isPreSale```, ```tokenExchangeRate```.

## Constants
Constants must be named with all capital letters with underscores separating words. Examples: ```MAX_BLOCKS```, ```TOKEN_NAME```, ```PRICE```, ```ERC20_CONTRACT```,```CONTRACT_VERSION```.

## Modifier Names
Use mixedCase. Examples: ```adminOnly```, ```onlyAfter```, ```onlyDuringThePreSale```.

## Enums
Enums, in the style of simple type declarations, must be named using the CapWords style. Examples: ```TokenGroup```, ```Frame```, ```HashStyle```, ```CharacterLocation```.