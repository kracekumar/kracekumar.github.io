+++
date = "2016-11-13 07:25:37+00:00"
draft = false
tags = ["recursecenter", "rust"]
title = "My Experience With Rust"
url = "/post/153116176705/my-experience-with-rust"
+++
When I was about to leave to <a href="http://recurse.com" target="_blank">RC</a> in few weeks, wrote an E-mail to <a href="https://twitter.com/punchagan" target="_blank">Puneeth</a> asking for `` Do's and Don'ts at RC ``. One of the line in the mail said,

>
> Since you are a Python guy, don’t write any Python code while you are there. Do something completely different.
>

I contemplated which language to choose. Other than `` Python ``, I knew a decent amount of `` Go-lang `` and `` Javascript ``. I previously attempted to learn `` rust `` but never dived deep into it. I reconsidered learning it and came up with the project idea.

>
> “Capture all the internet packet and figure out how much each website is consuming the bandwidth.”
>

After spending three weeks at RC, I started to work on the project <a href="http://slides.com/kracekumarramaraju/who-is-eating-my-bandwidth" target="_blank">imon</a>. I was excited to work on the project for several reasons

*   Everyone around me spoke about Rust’s memory model and how well rust is a good candidate for writing safe system programming code.
*   I have never done any low-level networking projects, though I had built a real-time backend for a messaging app.
*   This project involved working with multi-threaded programming, coroutine, and async networking at the same time.

### First bummer - Lifetime

<a href="https://github.com/ebfull/pcap" target="_blank">pcap</a> is a rust crate to capture the packets from a device(`` Wifi ``, `` Ethernet port ``). To get any useful information from the packet, you need to decode the packet carrying data of various layers. So decode `` Ethernet packet `` to pull out the payload. The payload becomes the next layer packet. The step goes on till you find required information. In my case, I need to have `` TCP `` or `` UDP `` packet to pull out wanted data. Capturing and decoding packet in same thread will make the program drop a lot of frames. So having a separate thread to sniff and decode made the program performant. So how do you communicate between two threads? I choose message passing since I had experience with `` coroutines ``, and `` go channels ``.

While passing the message via the channel from `` sniffer `` thread to `` decoder `` thread, packet didn’t live long enough to go through the channel. Here is the <a href="https://github.com/ebfull/pcap/issues/58" target="_blank">GH issue</a>.

The first suggestion which came up when I asked around was `` clone `` the packet and send across the channel. After hours of debugging, I didn’t get the solutions. After few days, <a href="https://twitter.com/k4rtik" target="_blank">karthik</a> replied

>
> The problem is that you are trying to send a reference with a limited lifetime across a thread boundary, which is not allowed. The `` clone `` is for a slice (a reference to a byte array), you probably want to do a `` clone_from_slice `` as shown here into a static byte array, then you should be able to talk across a thread (though this technique requires using unsafe).
>

I had multiple encounters with a lifetime of `` strings ``, `` string slice ``. Still, I am facing life time issues. As name suggests, `` life time `` problem is for `` life time ``.

### Traits

To collate the traffic data, the inferred information from packets needs to reside somewhere. SQLite was my preferred choice for ease of installation - I <a href="https://gist.github.com/kracekumar/d58ec0beab1d2ea4b17dff77aab22a58" target="_blank">https://gist.github.com/kracekumar/d58ec0beab1d2ea4b17dff77aab22a58</a>

Using the above code to insert records failed because `` FromSql trait `` wasn’t satisfied for the field `` date ``.

Adding the following lines didn’t fix the problem either.

    impl FromSql for chrono::Date<utc> {
        fn column_result(value: u32){
             println!("{:?}", value);
        }
    }

The compiler threw up an another error

`` The compiler allows declaring trait for types defined in the current crate. Date<utc> in Chrono doesn't implement FromSql trait. ``

`` rusqlite `` included support for `` DateTime `` <a href="https://github.com/jgallagher/rusqlite/blob/master/src/types/chrono.rs" target="_blank">in separate file</a>. Importing `` use rusqlite::type::chrono::{DateTime<utc>} `` failed. For a couple of hours, I was puzzled, how come code in the project directory is unimportable? Reading the <a href="https://github.com/jgallagher/rusqlite#optional-features" target="_blank">README</a> file carefully revealed, the <a href="http://doc.crates.io/manifest.html#the-features-section" target="_blank">Cargo</a> supports optional features to be included along with core library. Changing the `` Cargo.toml `` to have `` features `` attribute to `` dependencies.rusqlite `` fixed the issue.

    [dependencies.rusqlite]
    version = "0.7.3"
    features = ["chrono"]

What this means, the additional features which are part of the code base is compiled with binary only when specified. The use case is similar to installing `` SQLAlchemy `` and installing `` psycopg2 `` driver to connect `` Postgres ``.

### Serialization

The program follows `` daemon `` and `` client `` architecture. When client queries `` daemon `` to send traffic data like `` ./binary site kracekumar.com ``, the daemon sends out all the traffic data associated with the domain. I choose <a href="https://docs.rs/rmp/0.7.5/rmp/" target="_blank">msgpack</a> format to communicate. The `` rmp `` crate can automatically encode/decode `` struct `` to `` msgpack `` as long as attribute `` #[derive(RustcDecodable, RustcEncodable)] `` is used.

<div class="gist"><a href="https://gist.github.com/kracekumar/91bbc05d4d8201c35db0547bb8db086b" target="_blank">https://gist.github.com/kracekumar/91bbc05d4d8201c35db0547bb8db086b</a></div>

If the struct field is declared in an another crate like `` rusqlite ``, the source crate needs to support `` serialization ``. `` rusqlite `` supports `` serde_json `` and not `` rustc_serialize ``. So I ended up using custom tuple for serialization. Probably a good candidate for PR to `` rmp ``!

### Signal Handler

Rust natively doesn’t support signal handler like `` SIGTERM ``, `` SIGINT(Ctrl - C) ``. This is a good and bad decision. When the `` SIGINT `` interrupt is received; the daemon could write all the cached DNS mapping to a file and read the file during startup. <a href="https://github.com/BurntSushi/chan-signal" target="_blank">chan-signal</a> provides a way to do this, but only works for threaded code. Even if the program is designed to run on a single thread, `` chan_select `` requires you to spin a new thread, when the new thread receives the interrupt, the main thread executes `` chan_select! `` and clean up is performed inside macro.

<div class="gist"><a href="https://gist.github.com/kracekumar/c4012062b73612b3699e1de03774d11e" target="_blank">https://gist.github.com/kracekumar/c4012062b73612b3699e1de03774d11e</a></div>

My program uses a `` HashMap,  Arc `` to share data between threads safely. Using `` chan_signal `` with existing code structure caused lifetime issues for `` hashmap ``. I tried for a couple of hours to get it working, and it looked complicated than I thought. I have left to figure this out for future. Do you know any multi-threaded rust program that handles signals?

### Tests

I enjoy writing tests. It spots design smell and gifts subtle hints on `` API flexibility ``. Rust follows a different convention for `` unit tests `` and `` integration tests ``. Unit tests reside in the same file where the function is defined marked by the attribute `` #[test] ``. Integration tests are set in `` tests `` directory at the same level as `` src ``. By default rust, compiler harnesses multi-threading. If you’re web developer, you can think of what can happen in integration tests :-)

Running tests in multiple threads saves a considerable amount of time for large test suites. But tearing down tables or database for every test causes race conditions in DB. Test case for `` create_or_update_record `` requires serial execution. Setting environment variable `` RUST_TEST_THREADS=1 `` runs all tests in serial mode. `` RUST_TEST_THREADS `` environment variable affects both `` unit `` and `` integration `` tests.

`` Cargo.toml `` supports `` [[test]] `` section. Test section looks like

    [[test]]
    name = "db_integration"
    harness = false
    test = true

`` name `` points to file inside `` tests/db_integration.rs ``. `` db_integration.rs `` should have a entry point i.e `` main function ``. Tests inside `` db_integration.rs `` doesn’t contain `` #[test] `` attribute. The code looks like

<div class="gist"><a href="https://gist.github.com/kracekumar/dd289aa669e434c302b4620f987eb99e" target="_blank">https://gist.github.com/kracekumar/dd289aa669e434c302b4620f987eb99e</a></div>

Integration tests can only access `` public module/struct ``in the project crate. To assert on a struct’s field, the field should be declared `` pub `` keyword like `` pub has_changed: bool `` which makes sense.

### Error Messages

Rust error message is clear, concise, colorful and comes with a error code most of the times. Error code gives detailed write up about the possible scenario with an example. Here is an error message

<div class="gist"><a href="https://gist.github.com/kracekumar/ad339891e0792229ebce93d5eea90dca" target="_blank">https://gist.github.com/kracekumar/ad339891e0792229ebce93d5eea90dca</a></div>

Cargo `` explain `` flag displays verbose information about the error and link to the RFC.

<div class="gist"><a href="https://gist.github.com/kracekumar/3800fe67395acc5b8794c89e1597d830" target="_blank">https://gist.github.com/kracekumar/3800fe67395acc5b8794c89e1597d830</a></div>

This kind of error message explanation kindles interest to learn more. Some error message comes with excellent apt suggestions to fix the error.

At one place, rust error message is confusing and annoying. Error message returned while `` unwrapping a None `` is confusing and doesn’t print line number where error occurred. Here is an example

<div class="gist"><a href="https://gist.github.com/kracekumar/df1d975acfae9bc2a40ba3732b8b3ccf" target="_blank">https://gist.github.com/kracekumar/df1d975acfae9bc2a40ba3732b8b3ccf</a></div>

The correct way to unwrap an `` option `` is `` match ``, `` try! ``, `` ? ``, operator which is available in 1.13 or `` unwrap_or_else ``. Once I mistakenly accused foreign crate as unstable for the error. This took many encounters for me to figure out, the problem is with `` unwrap ``.

Here is output after setting `` RUST_BACKTRACE ``.

<div class="gist"><a href="https://gist.github.com/kracekumar/ac29d0f89ed9dfbcdf03b32b0d8a511b" target="_blank">https://gist.github.com/kracekumar/ac29d0f89ed9dfbcdf03b32b0d8a511b</a></div>

Having some sort of way to highlight the current project code in the backtrace can be visually useful for bigger project and nested code path.

### `` lib.rs `` and `` main.rs ``.

Here is my `` src `` directory structure.

    user@user-ThinkPad-T400 ~/c/imon> ls src
    cli.rs*  decoder.rs    ipc.rs   main.rs
    db.rs    formatter.rs  lib.rs*  packet.rs

`` main.rs `` is the entry point for the `` daemon `` and `` client ``. The code looks like

    #[macro_use] extern crate log;
    extern crate env_logger;
    extern crate imon;
    fn main(){
        env_logger::init().unwrap();
        imon::cli::parse_arguments();
    }

`` lib.rs `` contains all the external crate imports and public modules. The public modules declared here are only importable in any foreign crate utilizing the project.

    #[macro_use] extern crate log;
    extern crate env_logger;
    use std::fmt;
    pub mod cli;
    pub mod decoder;
    pub mod packet;

To use a `` macro `` defined in the crate `` log `` in any rust file inside `` src `` except `` lib.rs `` no import is needed. All foreign crate are imported in `` lib.rs `` and another rust file can use importables like `` use foo; foo.method() `` but in `` main.rs `` needs `` extern crate log; `` statement and doesn’t look in `` lib.rs ``. The import distinction between `` lib.rs `` and `` main.rs `` is a gray area to understand since both the files reside in the same directory.

Currently, I feel comfortable with `` rust ``. I haven’t used most of the features in `` rust `` like `` mutex, macros `` or distributed binaries. I have lots to read about rust, writing idiomatic rust but I am confident of using `` rust `` in production. I am learning what pieces can fall apart. There has been an enormous amount of work gone into rust compiler and tooling especially `` cargo ``. If you’re aware of Python world, `` Cargo `` is a single crate which performs multiple Python packages work `` pip ``, `` virtualenv ``, `` pytest `` and `` cookiecutter ``.

I owe a big part to RC folks who patiently assisted during the learning curve. Thanks to <a href="http://github.com/caipre" target="_blank">Nick Platt</a>, <a href="https://github.com/kamalmarhubi" target="_blank">Kamal Marhubi</a>, Mike Nielsen and others.
