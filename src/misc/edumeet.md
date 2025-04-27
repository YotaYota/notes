# eduMEET

## Installation

[edumeet/edument](https://github.com/edumeet/edumeet) is simply a deployment repo.

do `yarn start` in edumeet-client.

bare bones is:

- [edumeet-client](https://github.com/edumeet/edumeet-client)
- [edumeet-room-server](https://github.com/edumeet/edumeet-room-server/)
- [edumeet-media-node](https://github.com/edumeet/edumeet-media-node)

optionally use [edumeet-management-server](https://github.com/edumeet/edumeet-management-server) repo (uses JWT).

develop on develop branch.

## edumeet-room-server

middlewares are the capabilities of a room. They are like express middleware (not react/redux middleware like in client).

a Peer object represents one edumeet-client out there.

A room server has access to 1 or more media nodes.

## edumeet-media-node

Media router. A hub that all connect to. A Producer, eg a video stream, has several Consumers (people recieving the video).

pipeProducers are for using more cores.

## eduMEET-client

### Configuration

Default state is in *src/utils/edumeetConfig.tsx*.

Configuration can be changed in *config/config.js*.

The configuration is loaded in *index.html* through a `<script>` tag.

---

hooks and limited redux.

"thunk actions" eg sendMessage.

dispatch state runs through all the middlewares (which intercepts them and may take actions). cf dispatch action.

useContext

---

navigate by starting in views/x and you can find all the things related to it.

---

services:

the biggest 2 are

- mediaService
- signalingService

---

slices:

builds up the state.

---

middlewares:

biggest is mediaMiddleware.

## Running EduMEET Client

In each repository:

- Check `node --version` is at least 20.
- Check `yarn --version` is version 1.

**Note**: Prepend `yarn start` with `DEBUG=<something>` to show logs.

### 1. Start a room-server

Go to _./config/config.json_ and remove `managementService`.

```bash
yarn install
```

```bash
yarn start
```

and it should be listening on port 8443 (`ss -nltp | grep edumeet`).

Note `mediaNodes[0].secret` in _config/config.json_ - this secret needs to be provided by media server when started.

### 2. Start media-node

```bash
yarn install
```

Copy the secret from previous step

```bash
yarn start --ip 127.0.0.1 --secret <secret string>
```

### 3. Start edumeet-clientinstall

Go to _./config/config.json_ and remove everything except

```js
var config = {
    loginEnabled: false,
    developmentPort. 8443,
    productionPort: 443
}
```

# Docker Installation

- Clone (edumeet-docker)[https://github.com/edumeet/edumeet-docker] via HTTPS (better to not have a git user on host)
- install dependencies
- create a user
- install docker and add user to docker group (to avoid using sudo) then relogin to take effect
- `./run-me-first.sh`
    - create user on emil format
    - `perl: warning: Setting locale failed.`
- `.env` file
- `MEDIA_SECRET`

- `kc/realms/{realm}/.well-known/openid-configuration` to get configuration for 
