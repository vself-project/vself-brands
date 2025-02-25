<p align="center">
  <img src="brands.png" alt="Vself Ninja"/>
</p>


# vSelf for brands

## Overview 

Current repository contains source code of vSelf web application compatible with [Camino Blockchain](https://camino.network/). 

The available fuctionality is to set up custom SBT giveaway, distribute SBT reward using claim link or QR code, check claim analytics.   

This application supports [MetaMask](https://metamask.io/) authorization and [Columbus](https://docs.camino.network/about/columbus-testnet) testnet.

## Deployment

- Staging version of [web application](https://brands.vself.app/) is available here. 
- Smart contract address is [0x37d0924289a66687d7624963aa4f4bc2da1af36e](https://suite.camino.network/explorer/columbus/c-chain/address/0x37d0924289a66687d7624963aa4f4bc2da1af36e).

## User flow 
![](https://github.com/vself-project/vself-brands/blob/main/309.png)

## Documentation

Here is the description of vSelf [SBT collection toolkit](https://vself-project.gitbook.io/vself-project-documentation/sbt-collection-toolkit) functionality.

## SBT smart contract

### Synopsis

[This smart contract](https://github.com/vself-project/vself-brands/blob/main/contracts/contracts/Events.sol) allows to set up giveaway with non-transferable NFT (Soul Bound Token), mints SBT reward to recipient account on successful checkin via claim link, and stores giveaway metadata and mint history. 

Each giveaway is uniquely identified by `eventId` and contains the single SBT available to claim.

### Data structure

- `Quest` contains SBT reward data: name, description and link to source image.

```
struct Quest {
      string rewardTitle;
      string rewardDescription;
      string rewardUri;
}
```

- `EventData` contains giveaway data: unique identifier, name, description, duration, wallets of owner and claimers.
```
struct EventData {
        uint256 eventId;
        uint256 startTime;
        uint256 finishTime;
        uint256 totalUsers;
        address eventOwner;
        string eventName;
        string eventDescription;
        bool isAvailable;
}
```

- `EventStats` contains set up and claim history and claim analytics.
```
struct EventStats {
        uint256 totalActions;
        uint256 totalRewards;
        uint256 totalUsers;
        uint256 createdAt;
        uint256 stoppedAt;
        address[] participants;
    }
```

- `Event` consists of all metadata described above.
```
struct Event {
        EventData eventData;
        EventStats eventStats;
        Quest quest;
    }
```
### External functions
- `startEvent()` adds new giveaway with given data and sets up new `eventId` for it.
```
startEvent(
        string memory _eventName,
        string memory _eventDescription,
        uint256 _startTime,
        uint256 _finishTime,
        Quest memory _quest
    );
```

- `checkin(uint56 _eventId, address _recipient)` mints SBT reward from giveaway with `_eventId` for `_recipient` wallet.
- `stopEvent(uint256 _eventId)` is available to call for `eventOwner` only, and it stops giveaway with given `_eventId`. 

### External view functions
- `getOngoingEventDatas(uint256 _fromIndex, uint256 _limit)` returns `EventData` for currently ongoing giveaways.
- `getOngoingEvents(uint256 _fromIndex, uint256 _limit)` returns all metadata for currently ongoing giveaways.
- `getEventData(uint256 _eventId)` returns `EventData` for the giveaway with `_eventId`.
- `getEvent(uint256 _eventId)` returns all metadata for the giveaway with `_eventId`.
- `getEventActions(uint256 _eventId, uint256 _fromIndex, uint256 _limit)` returns claim history for the giveaway with `_eventId`.

## Folder structure

```

├── **tests**              # Tests folder
├── components             # Components folder
├── constants              # Constants folder
├── contexts               # Contexts HOCs folder
├── features               # Features used in app's pages
│   ├── claims
│   ├── dashboard
│   ├── event-form
│   ├── events-details
│   ├── landing
│   └── profile
├── hooks
├── mockData               # Mocked data for development and test purposes
├── models                 # Types and interfaces
├── pages                  # Main pages of the App
│   ├── about
│   ├── academy
│   ├── add
│   ├── api
│   ├── app
│   ├── auth
│   ├── blog
│   ├── claim
│   ├── contact
│   ├── dashboard
│   ├── docs
│   ├── event
│   ├── faq
│   ├── nft-list
│   ├── onboard
│   ├── terms
│   ├── vranda
|   ├── vselfnfts
|   └── vstudio
├── styles                  # Base CSS styles folder
├── public                  # Public folder for storing fonts, images and other public files
├── utils                   # Utils functions folder
├── next-i18next.config.js  # i18n config file
├── next.config.js          # Next JS config file
├── tailwind.config.js      # Tailwind CSS config file
└── tsconfig.json           # Typescript config file
```
