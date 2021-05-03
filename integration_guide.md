# Proof of Personhood passport (PoPP) Integration Guide

## Table of Contents

- [Proof of Personhood passport (PoPP) Integration Guide](#proof-of-personhood-passport--popp--integration-guide)
  * [Preface](#preface)
  * [Why](#why)
  * [What](#what)
    + [PersonhoodScore](#personhoodscore)
    + [System Design](#system-design)
      - [PersonhoodScore](#personhoodscore-1)
      - [Privacy](#privacy)
      - [System Design](#system-design-1)
      - [Issuance Patterns](#issuance-patterns)
        * [NFTs](#nfts)
        * [DIDs](#dids)
      - [Consumption](#consumption)


## Preface

ALPHA WARNING - This project is in alpha, version 0.0.1. 

## Why

Let's start with why proof of personhood matters. 

Right now the crypto ecosystem is pretty centric around capital-intensive use cases.  dApps built around one-token-one-vote and one-cpu-one-vote favour those who already have capital.  This is a self-reinforcing cycle in which most dApps are speculative in nature, and that means labour (which may not have capital), cannot participate.

We want to start building a foundation for the ecosystem to prove personhood.  We think this is going to unlock a whole ecosystem of one-human-one-vote use cases. Stuff like

- quadratic funding
- quadratic voting
- UBI
- one-person-one-vote DAOs
- data collectives
- sybil resistent airdrops
- + other use cases we havent discovered yet!

## What

The team behind Gitcoin is building [PoPP](http://proofofpersonhood.com/) - a tool thats going to allow users to take their Gitcoin Grants sybil resistent identity across the dweb with them.  We think this could help unlock a whole ecosystem of one-human-one-vote use cases. 

We have put together an alpha implementation of this system which you can see here https://github.com/gitcoinco/pop_api  and https://github.com/gitcoinco/PersonhoodPassport .  

### PersonhoodScore

Basically the TLDR is that the Gitcoin system issues the user an SDK that allows them to issue a NFT (and soon an DID) to take their PersonhoodScore (PS) with them across the dweb.  The higher the personhoodscore, the higher the price of forgery for that identity.  A user with a $1 PS has an identity that costs $1 to forge.  A user with a $100 PS has an identity that costs $100 to forge, and so on.

Note: Proof of Personhood NFT is already deployed to rinkeby, and will be integrated into Gitcoin's site for alpha users soon, but it is recommended  for the build out of this site that you deploy your own version, which can easily be done with the setup instructions in https://github.com/gitcoinco/PersonhoodPassport .

### System Design

At a high level, we've discovered that a proof of personhood solution needs a few things.

1. it needs an economic incentive for people to participate.
2. it needs a solid architecture for seperating the bot-farms from the humans.

To solve for (1), we are leveraging Gitcoin Grants, an $1.5mm/quarter economic system that is dependant upon sybil resistence.

To solve for (2), Proof of Personhood passport's approach is to recognize that while each of us out there is either a binary - human-or-not, that with limited capital, we will never ever ever be 100% sure of proof of personhood. For this reason, personhood in the PoPP system is represented as a floating point number, not a binary. 

#### PersonhoodScore

We call this number the personhoodscore.  The higher the personhoodscore, the higher the price of forgery for that identity.  A user with a $1 PS has an identity that costs $1 to forge.  A user with a $100 PS has an identity that costs $100 to forge, and so on.

In order to increase Personhood Scores on PoPP, PoPP aggregates sybil resistence mechanisms across the space.  For example, a user who wishes to increase their PersonhoodScore, can verify their identity on:

- BrightID
- Idena Network
- POAP
- SMS
- Google
- Twitter
- Upala
- ENS
- Facebook

All sybil-resistent identity providers are not created equal, and it would be foolish to presume tha the cost of forgery for a twitter account == an Idena address.  For this reason, each identity provider has a PersonhoodScore Weight associated with it.  This weight is initially set as follws

- BrightID (25)
- Idena Network (25)
- POAP (10)
- SMS (2)
- Google (2)
- Twitter (5)
- Upala (0.1)
- ENS (0.1)
- Facebook (0.1)

We plan to eventually launch a governance process which can review attacks on the system + respond by adjusting the weights of each identity provider.  It is possible that we may have algorithmic weights too (e.g a twitter accounts weigh is `min(months_old, max(20, num_followers * 0.001))` instead of just `5` )

#### Privacy

Before we get further in the integration guide, let's talk privacy.

The goal of this project to empower internet citizens to better control their personal data. Data dignity means taking our social network data from site to site but also minimizing the risk of yet-another-privacy-destroying-data-leak.

For this reason, we have deliberately narrowed the scope of PoPP to ONLY contain information about PersonhoodScore, and have *no personally identifiable information available* to consumers of PoPP.

#### System Design

At it's highest level, the system 

1. integrates the identity providers listed above
2. computes a personhood score
3. issues credentials which users can take across the dWeb

<img src=img/design.jpg>

Below are the details of some of the ways PoPP is issued:

#### Issuance Patterns

##### NFTs

NFTs are issued one-per-address and one-per-account.  The token URI o of the NFT resolves to a JSON blob that looks like this:

```
{
        "name": "Personhood Passport",
        "description": "Proof of Personhood Passport (PoPP) is a transportable proof of personhood identity for the web3 space.",
        "image": image_url,
        'external_url': 'https://proofofpersonhood.com',
        'background_color': 'fbfbfb',
        "attributes": [
            {
                "trait_type": "personhood_score",
                "value": personhood_score,
            },
            {
                "trait_type": "cost_of_forgery",
                "value": cost_of_forgery,
            },
        ],
    }
```

The `personhood_score` attribute is the attribute that consumers of the NFT will should care about.


##### DIDs

Coming soon


#### Consumption


Consumption of this API is available in various API clients:

- [js/](js/)
- [python/](python/)


## Warnings, Roadmap, and todos

Right now this project is a proof of concept, and the following warnings should ube noted:

- Alpha Warning - This project is in alpha, version 0.0.1. 
- Execution Centralization Warning - Due to an Oracle problem with how the integrations are managed, the PersonhoodScore is issued by Gitcoin right now.  We do have plans to federate issuance of credentials down the line.
- Governance Centralization Warning - The PersonhoodScore weights right now are set by the Gitcoin team, but we do have plans to one day move them to a governance process.
- DIDs - We believe that the W3C compliant DIDs are the likely long-term best place for this information to live, but are providing NFTs as a recognition of where the ecosystem is today.  Depending upon consumer demand + governance, we may support either/both long term.


