<p align="center">
    <a href="https://github.com/MarshalX/atproto">
        <img alt="Logo of atproto SDK for Python by Midjourney'" src="https://github.com/MarshalX/atproto/raw/main/.github/images/logo.png">
    </a>
    <br>
    <b>Autogenerated from lexicons, well type hinted, documented, sync and async SDK for Python</b>
    <br>
    <a href="https://github.com/MarshalX/atproto/tree/main/examples">
        Examples
    </a>
    •
    <a href="https://atproto.blue">
        Documentation
    </a>
    •
    <a href="https://discord.gg/PCyVJXU9jN">
        Discord Bluesky API
    </a>
</p>

## The AT Protocol SDK

> ⚠️ Under construction. Until the 1.0.0 release compatibility between versions is not guaranteed. 

Code snippet:

```python
from atproto import Client, client_utils


def main():
    client = Client()
    profile = client.login('my-handle', 'my-password')
    print('Welcome,', profile.display_name)
    
    text = client_utils.TextBuilder().text('Hello World from ').link('Python SDK', 'https://atproto.blue')
    post = client.send_post(text)
    client.like(post.uri, post.cid)


if __name__ == '__main__':
    main()

```

<details>
  <summary>Code snippet of async version</summary>

```python
import asyncio

from atproto import AsyncClient, client_utils


async def main():
    client = AsyncClient()
    profile = await client.login('my-handle', 'my-password')
    print('Welcome,', profile.display_name)

    text = client_utils.TextBuilder().text('Hello World from ').link('Python SDK', 'https://atproto.blue')
    post = await client.send_post(text)
    await client.like(post.uri, post.cid)

    
if __name__ == '__main__':
    # use run() for a higher Python version
    asyncio.get_event_loop().run_until_complete(main())

```
</details>

💬 [Direct Messages (Chats)](https://atproto.blue/en/latest/dm.html)

🍿 [Example project with custom feed generator](https://github.com/MarshalX/bluesky-feed-generator)

🔥 [Firehose data streaming is available](https://atproto.blue/en/latest/atproto_firehose/index.html)

🌐 [Identity resolvers for DID and Handle](https://atproto.blue/en/latest/atproto_identity/index.html)

### Introduction

This SDK attempts to implement everything that provides ATProto. There is support for Lexicon Schemes, XRPC clients, Firehose, Identity, DID keys, signatures, and more. All models, queries, and procedures are generated automatically. The main focus is on the lexicons of atproto.com and bsky.app, but it doesn't have a vendor lock on it. Feel free to use the code generator for your own lexicon schemes. SDK also provides utilities to work with CID, NSID, AT URI Schemes, DAG-CBOR, CAR files, DID Documents and more.

### Requirements

- Python 3.8 or higher.

### Installing

``` bash
pip install atproto
```

### Quick start

First of all, you need to create the instance of the XRPC Client. To do so, you have two major options: asynchronous, and synchronous. The difference only in import and how you call the methods. If you are not familiar with async, use sync instead.

For sync:

```python
from atproto import Client

client = Client()
# By default, it uses the server of bsky.app. To change this behavior, pass the base api URL to constructor
# Client('https://example.com')
```

For async:

```python
from atproto import AsyncClient

client = AsyncClient()
# By default, it uses the server of bsky.app. To change this behavior, pass the base api URL to constructor
# AsyncClient('https://example.com')
```

In the snippets below, only the sync version will be presented.

Right after the creation of the Client instance, you probably want to access the full API and perform actions by profile. To achieve this, you should log in to the network using your handle and password. The password could be app-specific.

```python
from atproto import Client

client = Client()
client.login('my-username', 'my-password')
```

You are awesome! Now you feel to pick any high-level method that you want and perform it!

Code to send post:

```python
from atproto import Client

client = Client()
client.login('my-username', 'my-password')
client.send_post(text='Hello World!')
```

Useful links to continue:

- [List of all methods with documentation](https://atproto.readthedocs.io/en/latest/atproto_client/index.html).
- [Examples of using the methods](https://github.com/MarshalX/atproto/tree/main/examples).

### SDK structure

The SDK is built upon the following components:

| Package            | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| `atproto`          | Import shortcuts to other packages.                                         |
| `atproto_cli`      | CLI tool to generate code.                                                  |
| `atproto_client`   | XRPC Client, data models, and utils like rich text helper.                  |
| `atproto_codegen`  | Code generator of models, clients, and namespaces.                          |
| `atproto_core`     | Tools to work with NSID, AT URI Schemes, CID, CAR files, and DID Documents. |
| `atproto_crypto`   | Crypto utils like multibase, signature verification, work with DID keys.    |
| `atproto_firehose` | Firehose (data streaming) client and models.                                |
| `atproto_identity` | Identity resolvers for DID, Handle, AT Protocol data, signing keys.         |
| `atproto_lexicon`  | Lexicon parser.                                                             |
| `atproto_server`   | Server-side utils like JWT.                                                 |

I highly recommend you to use the `atproto` package to import everything that you need. 
It contains shortcuts to all other packages.

### Documentation

The documentation is live at [atproto.blue](https://atproto.blue/).

### Getting help

You can get help in several ways:
- Report bugs, request new features by [creating an issue](https://github.com/MarshalX/atproto/issues/new).
- Ask questions by [starting a discussion](https://github.com/MarshalX/atproto/discussions/new).
- Ask questions in [Discord server](https://discord.gg/PCyVJXU9jN).

### Advanced usage

I'll be honest. The high-level Client that was shown in the "Quick Start" section is not a real ATProto API. This is syntax sugar built upon the real XRPC methods! The high-level methods do not cover the full needs of developers. To be able to do anything that you want, you should know how to work with low-level API. Let's dive into it!

The basics:
- Namespaces – classes that group sub-namespaces and the XRPC queries and procedures. Built upon NSID ATProto semantic.
- Model – dataclasses for input, output, and params of the methods from namespaces. Models describe Record and all other types in the Lexicon Schemes.

#### Namespaces

The client contains references to the root of all namespaces. It's `com` and `app` for now.

```python
from atproto import Client

Client().com
Client().app
```

To dive deeper, you can navigate using hints from your IDE. Thanks to well-type hinted SDK, it's much easier.

```python
from atproto import Client

Client().com.atproto.server.create_session(...)
Client().com.atproto.sync.get_blob(...)
Client().app.bsky.feed.get_likes(...)
Client().app.bsky.graph.get_follows(...)
```

The endpoint of the path is always the method that you want to call. The method presents a query or procedure in XRPC. You should not care about it much. The only thing you need to know is that the procedures required data objects. Queries could be called with or without params.

#### Records

In some sub-namespaces, you can find records. Such record classes provide a syntax sugar not defined in the lexicon scheme. This sugar provides a more convenient way to work with repository operations. Such as creating a record, deleting a record, and so on.

Here are some available records of Bluesky records:

```python
from atproto import Client

Client().app.bsky.feed.post
Client().app.bsky.feed.like
Client().app.bsky.graph.follow
Client().app.bsky.graph.block
Client().app.bsky.actor.profile
# ... more
```

Usage example with the `post` record:

```python
from atproto import AtUri, Client, models

client = Client()
client.login('my-username', 'my-password')

posts = client.app.bsky.feed.post.list(client.me.did, limit=10)
for uri, post in posts.records.items():
    print(uri, post.text)

post = client.app.bsky.feed.post.get(client.me.did, AtUri.from_str(uri).rkey)
print(post.value.text)

post_record = models.AppBskyFeedPost.Record(text='test record namespaces', created_at=client.get_current_time_iso())
new_post = client.app.bsky.feed.post.create(client.me.did, post_record)
print(new_post)

deleted_post = client.app.bsky.feed.post.delete(client.me.did, AtUri.from_str(new_post.uri).rkey)
print(deleted_post)
```

Please note that not all repository operations are covered by these syntax sugars. You can always use the low-level methods to perform any desired action. One such action is updating a record.

#### Models

To deal with methods, we need to deal with models! Models are available in the `models` module and have NSID-based aliases. Let's take a look at it.

```python
from atproto import models

models.ComAtprotoIdentityResolveHandle
models.AppBskyFeedPost
models.AppBskyActorGetProfile
# 90+ more...
```

The model classes in the "models" aliases could be:

- Data model
- Params model
- Response model
- Sugar response model
- Record model
- Type model

The only thing you need to know is how to create instances of models. Not with all models, you will work as model-creator. For example, SDK will create Response models for you.

There are a few ways how to create the instance of a model:

- Dict-based
- Class-based

The instances of data and params models should be passed as arguments to the methods that were described above.

Dict-based:

```python
from atproto import Client

client = Client()
client.login('my-username', 'my-password')
# The params model will be created automatically internally for you!
print(client.com.atproto.identity.resolve_handle({'handle': 'marshal.dev'}))
```

Class-based:

```python
from atproto import Client, models

client = Client()
client.login('my-username', 'my-password')
params = models.ComAtprotoIdentityResolveHandle.Params(handle='marshal.dev')
print(client.com.atproto.identity.resolve_handle(params))
```

Tip: look at typehint of the method to figure out the name and the path to the input/data model!

Pro Tip: use IDE autocompletion to find necessary models! Just start typing the method name right after the dot (`models.{type method name in camel case`).

Models could be nested as hell. Be ready for it!

This is how we can send a post with the image using low-level XRPC Client:

```python
from atproto import Client, models

client = Client()
client.login('my-username', 'my-password')

with open('cat.jpg', 'rb') as f:
    img_data = f.read()

    upload = client.upload_blob(img_data)
    images = [models.AppBskyEmbedImages.Image(alt='Img alt', image=upload.blob)]
    embed = models.AppBskyEmbedImages.Main(images=images)

    client.com.atproto.repo.create_record(
        models.ComAtprotoRepoCreateRecord.Data(
            repo=client.me.did,
            collection=models.ids.AppBskyFeedPost,
            record=models.AppBskyFeedPost.Record(
                created_at=client.get_current_time_iso(), text='Text of the post', embed=embed
            ),
        )
    )

    # of course, you can use the syntax sugar here instead
    post = models.AppBskyFeedPost.Record(text='Text of the post', embed=embed, created_at=client.get_current_time_iso())
    client.app.bsky.feed.post.create(client.me.did, post)
    # or even high-level client
    client.send_image(text='Text of the post', image=img_data, image_alt='Img alt')
    # these three methods are equivalent
```

I hope you are not scared. May the Force be with you. Good luck!

### Change log

The full change log is available in [CHANGES.md](https://github.com/MarshalX/atproto/blob/main/CHANGES.md).

### Contributing

Contributions of all sizes are welcome. The contribution guidelines will be presented later.

### License

MIT
