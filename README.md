This is a fun project written in Python 3 that explores how BitTorrent works using asyncio, which is a library for asynchronous programming in Python.

So, what does this BitTorrent client actually do? Well, it's not a full-fledged one that you'd use every day. Instead, it's more of an experimental one made for learning purposes. It can do some basic things:

1. Download files from other users (called leeching).
2. Check in with the tracker regularly to find other users to connect to.
3. Share files with others once they've been downloaded (called seeding).
4. It can handle torrents with multiple files.
5. If you stop the download and come back later, it can pick up where it left off.

However, there are a few issues you might run into:

- Sometimes, when you start the client, it might hang, especially if you're trying to connect to lots of other users at once.

Now, let's dive a bit into how the code works:

- The main part of the program is called `TorrentClient`. It connects to the tracker to find out who else has the file you want to download, then it creates a list of those peers.
- It figures out which pieces of the file it should ask for first from these peers.
- Once it's downloaded everything, it shuts down.

The `PieceManager` is responsible for deciding which piece of the file to ask for next and putting the pieces together once they're downloaded. Right now, it uses a simple strategy.

One thing to note is that writing files happens one after the other, which could be improved for efficiency.

The actual BitTorrent stuff is handled in the `protocol` module. The `PeerConnection` class sets up a connection to another user, and it manages the messages they send back and forth.

Since BitTorrent is a binary protocol, decoding those messages is done using an async iterator called `PeerStreamIterator`. It keeps reading and understanding the data sent over the connection until it's closed.

Each type of message in BitTorrent has its own class with methods to encode and decode it. But because this client doesn't support sharing files yet, not all the message types are used in both directions.
