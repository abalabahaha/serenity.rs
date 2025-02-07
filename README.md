[![ci-badge][]][ci] [![crate-badge][]][crate] [![license-badge][]][license] [![docs-badge][]][docs] [![contribs-badge][]][contribs]

# serenity.rs

Serenity is an ergonomic and high-level Rust library for the Discord API.

View the [examples] on how to make and structure a bot.

Serenity supports both bot and user login via the use of [`Client::login_bot`]
and [`Client::login_user`].

You may also check your tokens prior to login via the use of
[`validate_token`].

Once logged in, you may add handlers to your client to dispatch [`Event`]s,
such as [`Client::on_message`]. This will cause your handler to be called
when a [`Event::MessageCreate`] is received. Each handler is given a
[`Context`], giving information about the event. See the
[client's module-level documentation].

The [`Shard`] is transparently handled by the library, removing
unnecessary complexity. Sharded connections are automatically handled for
you. See the [gateway's documentation][gateway docs] for more information.

A [`Cache`] is also provided for you. This will be updated automatically for
you as data is received from the Discord API via events. When calling a
method on a [`Context`], the cache will first be searched for relevant data
to avoid unnecessary HTTP requests to the Discord API. For more information,
see the [cache's module-level documentation][cache docs].

Note that - although this documentation will try to be as up-to-date and
accurate as possible - Discord hosts [official documentation][discord docs]. If
you need to be sure that some information piece is accurate, refer to their
docs.

# Example Bot

A basic ping-pong bot looks like:

```rust,no-run
extern crate serenity;

use serenity::client::{Client, Context};
use serenity::model::Message;
use std::env;

fn main() {
    // Login with a bot token from the environment
    let mut client = Client::login_bot(&env::var("DISCORD_TOKEN").expect("token"));
    client.with_framework(|f| f
        .configure(|c| c.prefix("~")) // set the bot's prefix to "~"
        .on("ping", ping));

    // start listening for events by starting a single shard
    let _ = client.start();
}

fn ping(_context: &Context, message: &Message, _args: Vec<String>) {
    let _ = message.reply("Pong!");
}
```

### Full Examples

Full examples, detailing and explaining usage of the basic functionality of the
library, can be found in the [`examples`] directory.

# Features

Features can be enabled or disabled by configuring the library through
Cargo.toml:

```toml
[dependencies.serenity]
git = "https://github.com/zeyla/serenity.rs.git"
default-features = false
features = ["pick", "your", "feature", "names", "here"]
```

The following is a full list of features:

- **cache**: The cache will store information about guilds, channels, users, and
other data, to avoid performing REST requests. If you are low on RAM, do not
enable this;
- **framework**: Enables the framework, which is a utility to allow simple
command parsing, before/after command execution, prefix setting, and more;
- **methods**: Enables compilation of extra methods on struct implementations,
such as `Message::delete()`, `Message::reply()`, `Guild::edit()`, and more.
Without this enabled, requests will need to go through the [`Context`] or
[`rest`] module, which are slightly less efficient from a development
standpoint, and do not automatically perform permission checking;
- **voice**: Enables compilation of voice support, so that voice channels can be
connected to and audio can be sent/received.

# Dependencies

Serenity requires the following dependencies:

- openssl

### Voice

The following dependencies all require the **voice** feature to be enabled in
your Cargo.toml:

- libsodium (Arch: `community/libsodium`)
- opus (Arch: `extra/opus`)

Voice+ffmpeg:

- ffmpeg (Arch: `extra/ffmpeg`)

Voice+youtube-dl:

- youtube-dl (Arch: `community/youtube-dl`)

[`Cache`]: https://serenity.zey.moe/serenity/ext/cache/struct.Cache.html
[`Client::login_bot`]: https://serenity.zey.moe/serenity/client/struct.Client.html#method.login_bot
[`Client::login_user`]: https://serenity.zey.moe/serenity/client/struct.Client.html#method.login_user
[`Client::on_message`]: https://serenity.zey.moe/serenity/client/struct.Client.html#method.on_message
[`Shard`]: https://serenity.zey.moe/serenity/client/gateway/struct.Shard.html
[`Context`]: https://serenity.zey.moe/serenity/client/struct.Context.html
[`Event`]: https://serenity.zey.moe/serenity/model/enum.Event.html
[`Event::MessageCreate`]: https://serenity.zey.moe/serenity/model/enum.Event.html#variant.MessageCreate
[`examples`]: https://github.com/zeyla/serenity.rs/blob/master/examples
[`rest`]: https://serenity.zey.moe/serenity/client/rest/index.html
[`validate_token`]: https://serenity.zey.moe/serenity/client/fn.validate_token.html
[cache docs]: https://serenity.zey.moe/serenity/ext/cache/index.html
[ci]: https://travis-ci.org/zeyla/serenity.rs
[ci-badge]: https://travis-ci.org/zeyla/serenity.rs.svg?branch=master
[contribs]: https://img.shields.io/github/contributors/zeyla/serenity.rs.svg
[contribs-badge]: https://img.shields.io/github/contributors/zeyla/serenity.rs.svg
[crate]: https://crates.io/crates/serenity
[crate-badge]: https://img.shields.io/crates/v/serenity.svg?maxAge=2592000
[client's module-level documentation]: https://serenity.zey.moe/serenity/client/index.html
[discord docs]: https://discordapp.com/developers/docs/intro
[docs]: https://serenity.zey.moe/
[docs-badge]: https://img.shields.io/badge/docs-online-5023dd.svg
[examples]: https://github.com/zeyla/serenity.rs/tree/master/examples
[gateway docs]: https://serenity.zey.moe/serenity/client/gateway/index.html
[license]: https://opensource.org/licenses/ISC
[license-badge]: https://img.shields.io/badge/license-ISC-blue.svg
