---
gists:
  - id: 352998a34a8d4e0fa0ded91a9fab0b0a
    url: 'https://gist.github.com/starch0/352998a34a8d4e0fa0ded91a9fab0b0a'
    createdAt: '2025-08-17T00:10:48Z'
    updatedAt: '2025-08-17T00:15:21Z'
    filename: STUN and TURN in WebRTC.md
    isPublic: true
    baseUrl: 'https://api.github.com'
---


## 1 Quick context, why do STUN/TURN exist?

Most users are behind NATs and firewalls. **WebRTC** needs to establish a peer-to-peer (P2P) path between clients. That’s where STUN and TURN come in:

- **STUN (Session Traversal Utilities for NAT):** allows a client to discover its **public-facing address** (IP:port) as seen from the internet. This creates **server-reflexive candidates** (`srflx`).
    
- **TURN (Traversal Using Relays around NAT):** if direct paths fail, traffic is **relayed through a TURN server**. These candidates are called **relay**.
    

The process is managed by **ICE (Interactive Connectivity Establishment)**. Each peer gathers connection candidates (`host`, `srflx`, `relay`, `prflx`) and performs connectivity checks until a working pair is found.

**Mental shortcut:**

- Try `host` (local LAN) → if blocked, try `srflx` (STUN) → if blocked, fallback to `relay` (TURN).

- TURN is the **plan C**: more expensive and with higher latency, but highly reliable.

---

## 2 When you want to use STUN vs. TURN?

- **STUN** is almost always required. It’s lightweight, public (many providers host free STUN servers), and solves most scenarios.
    
- **TURN** is needed when:
    
    - Corporate firewalls block UDP or random ports.
        
    - Symmetric NATs prevent predictable port mapping.
        
    - You want to **force relaying** (`iceTransportPolicy: "relay"`) for compliance or predictable routing.
        

> Tip: Always run **at least one TURN server** (e.g., `coturn`) as a fallback in production.

---

## 3 Configuring ICE servers in the browser

```ts
// src/webrtc/ice.ts
export type IceUrl = `stun:${string}` | `turn:${string}` | `turns:${string}`

export type IceServer = Readonly<{
  urls: ReadonlyArray<IceUrl> | IceUrl
  username?: string
  credential?: string
}>

export type PcConfig = Readonly<{
  iceServers: ReadonlyArray<IceServer>
  iceTransportPolicy?: RTCIceTransportPolicy // 'all' | 'relay'
}>

export const defaultStun: IceServer = {
  urls: [
    'stun:stun.l.google.com:19302',
    'stun:global.stun.twilio.com:3478'
  ]
}

export const makePc = (cfg: PcConfig) => new RTCPeerConnection(cfg)
```

### Forcing TURN-only mode (useful for restrictive networks)

```ts
// src/webrtc/turn-only.ts
import { makePc, type IceServer } from './ice'

const turn: IceServer = {
  urls: ['turns:turn.example.com:5349'],
  username: 'user',
  credential: 'pass',
}

export const pcRelayOnly = () =>
  makePc({ iceServers: [turn], iceTransportPolicy: 'relay' })
```

---

## 4 Pipeline for _trickle ICE_

With _Trickle ICE_, you send candidates as soon as they are discovered, without waiting for full gathering. Here’s a minimal, immutable, composable setup:

```ts
// src/webrtc/trickle.ts
export type OnCandidate = (c: RTCIceCandidate) => void
export type OnState = (s: RTCPeerConnectionState) => void

export const setupTrickle = (
  pc: RTCPeerConnection,
  onCandidate: OnCandidate,
  onState: OnState,
): void => {
  pc.onicecandidate = e => e.candidate && onCandidate(e.candidate)
  pc.onconnectionstatechange = () => onState(pc.connectionState)
}

// Example usage:
// const pc = makePc({ iceServers: [defaultStun] })
// setupTrickle(pc, sendCandidateOverSignal, console.log)
```

### Functional serialization of candidates for the signaling channel

```ts
// src/signal/candidate.ts
export type WireCandidate = Readonly<{ sdpMid?: string; sdpMLineIndex?: number; candidate: string }>

export const toWire = (c: RTCIceCandidate): WireCandidate => ({
  sdpMid: c.sdpMid ?? undefined,
  sdpMLineIndex: c.sdpMLineIndex ?? undefined,
  candidate: c.candidate,
})

export const fromWire = (w: WireCandidate) => new RTCIceCandidate(w)
```

---

## 5 Discovering and debugging ICE candidates

A common educational step is to **observe which candidates are discovered**. This helps you understand whether your STUN/TURN servers are configured properly.

```ts
// src/debug/ice-log.ts
export const logCandidates = (pc: RTCPeerConnection): void => {
  pc.onicecandidate = e => {
    if (e.candidate) {
      console.log('Discovered candidate:', e.candidate.candidate)
    } else {
      console.log('All ICE candidates have been gathered.')
    }
  }
}
```

### Example usage

```ts
import { makePc, defaultStun } from '../webrtc/ice'
import { logCandidates } from '../debug/ice-log'

const pc = makePc({ iceServers: [defaultStun] })
logCandidates(pc)

// Trigger candidate gathering by creating an offer
pc.createDataChannel('test')
await pc.setLocalDescription(await pc.createOffer())
```

This will print lines like:

```
Discovered candidate: candidate:842163049 1 udp 1677729535 192.168.0.5 57321 typ host
Discovered candidate: candidate:842163049 1 udp 1677729535 203.0.113.10 3478 typ srflx
```

---

## 6 Educational tips

- **Check candidate types**: `host`, `srflx`, and `relay` tell you how connectivity was established.
    
- **Simulate failures**: block UDP traffic or restrict firewall settings to see when TURN is triggered.
    
- **Always monitor connection state**: listen to `pc.connectionState` for transitions (`new` → `checking` → `connected` → `disconnected`/`failed`).
    

---

## 7 Wrapping up

STUN and TURN are the unsung heroes of WebRTC. STUN gives you public reflexive addresses, TURN guarantees connectivity when all else fails. By combining them thoughtfully and logging candidate discovery, you can:

- Build resilient P2P applications.
    
- Understand NAT/firewall behavior.
    
- Debug connectivity issues with confidence.
    

> Next step: Deploy your own **coturn** server, configure TLS (for `turns:`), and test your app under real-world network restrictions.
