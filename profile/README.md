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

![Screenshot from 2023-04-06 21-18-48](https://user-images.githubusercontent.com/57960301/230757730-ed3fddcb-6c71-4235-87c2-acef559fe3eb.png)
