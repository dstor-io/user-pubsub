7/15/2023 WiP, testing and moving code from private repo to be held here moving forward.
# Arweave subscription and notification feature for Dstor.io

We will be moving the public facing code for this module here along with links for node setup.

## What it does

 * Enables ability to subscribe to another Arweave address and receive near-real-time notifications when the target user creates new transactions on the network.
 * Additional criteria for filtering the subscription can include: any traditional GraphQL filters, wildcards, topics and specific keywords within the transaction tags or within the tx data (if the data arweave.net/raw/txId is of type application/json or some form of text such as html, markdown, plain, etc.).

## What this repo includes

 * The React front-end ui for user creating a new subscription and editing existing subscriptions, authenticated via Arconnect or Arweave.app wallet.
 * Warp contract

## Process flow

 * User authenticates
 * User clicks button to create new subscription, optionally providing a name for it, providing target Arweave address, and if desired, clicking to add additional tx tags (key-value) or text filters.
 * User submits >> transaction Warp contract interaction
 * On-page notification ui elements pull any txIds from contract interactions created by the pubsub node
 ----
 * A node runs modified Arsyncer library to stream transactions in realtime, providing basic owner, tag-filtering as well as advanced text matching on each.
 * After the completion of each concurrency batch:
   * If any matches were found, a tx is stored on Arweave with the manifest of the users (subscribers), the active subscription with matches found in this batch, and the list of txIds
   * Matches also stored locally on node for N period of time (configurable)
   * The node fetches/revalidates the latest subscription's found in the contract interactions that include active tag.
   * An API endpoint (currently fastAPI) provides a lookup for individual users: ```https://{{server}}:8080/user/{{subscriberAddress}}``` that returns the same user notification data. (TO-DO, replace local caching with Warp DRE node functionality)
