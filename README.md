# pieces

An experimental BitTorrent client implemented in Python 3 using asyncio.

The client is not a practical BitTorrent client, it lacks too many
features to really be useful. It was implemented for fun in order to
learn more about BitTorrent as well as Python's asyncio library.

The client currently only support downloading of data.

Current features:

- Download pieces (leeching)
- Contact tracker periodically
- Seed (upload) pieces
- Support multi-file torrents
- Resume a download

Known issues:

* Sometimes the client hangs at startup. It seems to relate to the
  number of concurrent peer connections.

### Code walkthrough

The `pieces.client.TorrentClient` is the center piece, it:

* Connects to the tracker in order to receive the peers to connect to.

* Based on that result, creates a Queue of peers that can be connected
  to.

* Determine the order in which the pieces should be requested from the
  remote peers.

* Shuts down the client once the download is complete.


The strategy on which piece to request next and the assembly of
retrieved pieces is implemented in the `pieces.client.PieceManager`. As
previously stated, the strategy implemented is the simplest one
possible.

Notice, the file writing is synchronous something that could be
improved.

The BitTorrent specifics is implemented in the `pieces.protocol` module
where the `pieces.protocol.PeerConnection` sets up a connection to one
of the remote peers retrieved from the tracker. This class handles the
control flow of messages between the two peers.

BitTorrent is a binary protocol, and all decoding of messages is
implemented as a _async iterator_ under he name
`pieces.protocol.PeerStreamIterator`. The async part is that this
iterator will keep reading and parsing the raw data received from the
socket until the connection is closed.

Each of BitTorrents messages is implemented as separate classes, each
with a `encode` and a `decode` method. However, since this client
currently does not support seeding - not all of the messages goes in
both ways.


