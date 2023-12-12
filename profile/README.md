# Decentralization of Data Storage

## The Goal

Data storage in social networking applications can be quite costly,
and relying on true peer to peer networking models can be slow, part
of the reasong being that most peer to peer solutions tend to rely
on *UDP*, which is great for realtime communications, but when it
comes to reliable data propagation, fails miserably. Also a true
peer to peer network results in a complex and tedious amount of work
to find the appropriate path for data propagation. We provide a
partial peer to peer implementation to reduce the time required to
find such paths, and allow servers to store data on clients device,
reducing any form of redundancy and maintaining a data storage that
is cheap.

## The Idea

```mermaid
sequenceDiagram

actor C1
participant S as Server
participant B as Broadcast
actor C2

C1 -->> S: GET ws://server:port/proto/v1/ws
S ->> C1: { ..., connected, client_id }

C2 -->> S: GET ws://server:port/proto/v2/ws
S->> C2: { ..., connected, client_id }

C1 ->> C1: updates registry to post content
C2 ->> S: { ..., query, post_title }
S --) B: { sender: C2, ..., query, post_title }
B --) C1: repeat broadcast
B --) C2: repeat broadcast

C2 --) C2: skip broadcast since mesg.sender == self.id

alt if C1 has content matching required query
    C1 ->> S: { ..., response, { id, posted, title } }
end

loop collect message until pre-decided timeout
    S --> S: collect responses for C2
end

S ->> C2: { ..., response, vec[{ id, posted, title, from }] }
C2 ->> S: { ..., get, { target, post_id } }

note right of S: assuming target from previous get request is C1
S ->> C1: repeat get request
C1 ->> S: { ..., post, { ...post_header, content } }
S ->> C2: { sender: C1, ..., post, { ...post_header, content } }

C2 --) C2: update registry to contain post as long as it exists

C1 --x S: Disconnected
C2 --x S: Disconnected
```
