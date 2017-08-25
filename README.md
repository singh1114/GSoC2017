# Google Summer of Code 2017

## Paul Moritz Schaub - OMEMO Encrypted Jingle File Transfer in Smack

Welcome to the project page of my GSoC 2017 project!

The goal of my project was to implement Jingle File Transfer for [Smack](https://github.com/igniterealtime/Smack/). Smack is a client library for the [XMPP](https://xmpp.org/) protocol for Java and Android.

XMPP stands for `eXtensible Message and Presence Protocol`. The focus lays on _extensible_. Many features are not part of the core specification, but instead are defined in so called XEPs (XMPP Extension Protocols). XEPs are numbered, so every XEP has a unique number and a name to identify it. The goal of my project was to implement XEP-0234 - Jingle File Transfer. In order to achieve this goal, I had to implement a whole number of other XEPs, since there are some transitive dependencies (see below).

I documented my progress in weekly blog posts. Those can be found in my [blog](https://blogs.fsfe.org/vanitasvitae/category/xmpp/).

### XEP-0234 - Jingle File Transfer
This XEP describes how two entities can exchange files using the Jingle Protocol. Jingle itself is another XEP which I also had to implement (see <a href="#xep0166">below</a>), since Jingle File Transfer is defined on top of it. 
The specification for XEP-0234 can be found [here](https://xmpp.org/extensions/xep-0234.html). 
The code I wrote for that XEP is available in [this](https://github.com/vanitasvitae/Smack/tree/jingle3/smack-experimental/src/main/java/org/jivesoftware/smackx/jingle_filetransfer) package.

### XEP-0166 - Jingle <a name="xep0166"></a>
The Jingle XEP defines how two entities can negotiate bytestreams between each other. 
This involves, which transport methods are used (examples for that down below), which parameters are used for the application of the bytestream (eg. which audio codec in a voice chat) and so on. 
Also security plays a role in the negotiation. The Jingle protocol consists of three main components:

 - Description: Describes the application of the session (eg. file transfer - see XEP-0234)
 - Transport: The transport method used. This might be InBandBytestreams, SOCKS5 proxies...
 - Security: The encryption used. Describes how the bytestream is secured.
 
A _session_ consists of one or more _contents_, which in turn consist of a _description_, a _transport_ method and a _security_ component. Those components can be composed and replaced in arbitrary ways.

Implementing the Jingle XEP was the biggest part of my project. 
I worked using the waterfall model to learn from previous errors I made and it took me 3 iterations, until I was happy with my code. 
Now I have a very modular codeset which can be easily extended and composed in any way. 
This [blog post](https://blogs.fsfe.org/vanitasvitae/2017/07/26/gsoc-week-8-reworking/) gives an overview of the structure. 
The specification is available [here](https://xmpp.org/extensions/xep-0166.html), while my implementation can be found [here](https://github.com/vanitasvitae/Smack/tree/jingle3/smack-extensions/src/main/java/org/jivesoftware/smackx/jingle).

### XEP-0261 - Jingle InBandBytestream Transport
The easiest, most basic transport method is described in XEP-261. 
When data is sent using InBandBytestreams, the raw data is split into blocks of typically 4096 base64 encoded byte, which are sent one after another via the XMPP protocol itself. 
This method is occasionally very slow, but since it depends only on the XMPP protocol itself, it does not require additional server infrastructure. 
The XEP can be found [here](https://xmpp.org/extensions/xep-0261.html), my implementation resides in [this package](https://github.com/vanitasvitae/Smack/tree/jingle3/smack-extensions/src/main/java/org/jivesoftware/smackx/jingle/transport/jingle_ibb).

### XEP-0260 - Jingle SOCKS5Bytestream Transport

A more advanced transport method is described in Jingle SOCKS5Bytestream Transports (short Jingle S5B). Here SOCKS5 proxy servers are utilized to enabled clients behind firewalls or NATs to establish connections with each other. It is possible to use either external servers (eg. proxy components of the XMPP servers), or local servers for this purpose. 

Working on the implementation for that XEP was a pain. For half a week I was stuck at finding one little bug. My frustrated [blog post](https://blogs.fsfe.org/vanitasvitae/2017/08/07/gsoc-week-10-finding-that-damn-little-bug/) has more information about that. 
The XEP can be found [here](https://xmpp.org/extensions/xep-0260.html), my implementation [here](https://github.com/vanitasvitae/Smack/tree/jingle3/smack-extensions/src/main/java/org/jivesoftware/smackx/jingle/transport/jingle_s5b).

XEP-0261 and XEP-0260 are based on two older XEPs ([XEP-0047](https://xmpp.org/extensions/xep-0047.html) and [XEP-0065](https://xmpp.org/extensions/xep-0065.html)), so I could reuse some already existing code for those two transport methods.

### XEP-0300 - Use of Cryptographic Hash Functions
A Jingle File Transfer involves checksums to ensure the integrity of the sent file. Since computer science is a rapidly changing field, hash functions might quickly get obsolete (see SHA1 - [well done you](https://shattered.io/), Google :D), so it is desirable to be able to quickly replace hash functions. For that purpose XEP-0300 defines namespaces for hash functions, so that negotiators can use different algorithms as they wish. The specification can be found [here](https://xmpp.org/extensions/xep-0300.html), while my implementation lives [here](https://github.com/vanitasvitae/Smack/tree/jingle3/smack-experimental/src/main/java/org/jivesoftware/smackx/hashes).

### XEP-xxxx - Jingle Encrypted Transports (JET)
![JET Logo](https://github.com/vanitasvitae/GSOC2017/blob/master/logo.png?raw=true)
The most exciting part of my project was to create my own specification! With _JET_ I specified a way to utilize [OMEMO](https://conversations.im/omemo/xep-omemo.html) and [OX](https://xmpp.org/extensions/xep-0374.html) (OpenPGP) end-to-end encryption to secure Jingle transports. This way entities can transfer bytestreams end-to-end encrypted. The specification allows for easy extensibility with other encryption mechanisms additionally to OMEMO or OX. My plan is to propose the specification to the [XSF](https://xmpp.org/about/xmpp-standards-foundation.html) for approval, so that it might one day become an official XEP.

This [blog post](https://blogs.fsfe.org/vanitasvitae/2017/08/02/gsoc-week-9-bringing-it-back-to-life/) goes into more details on how the protocol works.

My draft specification can be found [here](https://geekplace.eu/xeps/xep-jet/xep-jet.html), while my prototype implementation can be found [here](https://github.com/vanitasvitae/Smack/tree/jingle3/smack-experimental/src/main/java/org/jivesoftware/smackx/jet) (only OMEMO encryption so far).

## Status
Not all of my code has been merged into Smacks [master branch](https://github.com/igniterealtime/Smack/) yet. This is partially due to experimental features, which need approval by the XSF.
Unmerged code can be found in [my repository](https://github.com/vanitasvitae/Smack/tree/jingle3).

**Merged**
 - XEP-0166 - Jingle
 - XEP-0260 - Jingle S5B
 - XEP-0261 - Jingle IBB
 - XEP-0300 - Hashes
 
**Pending**
 - XEP-0234 - Jingle File Transfer
 - XEP-xxxx - Jingle Encrypted Transports
 
## Interoperability
The whole point of XMPP is to avoid walled gardens, so there are many clients implementing many of the specified features. Ideally all clients are interoperable to one another (in reality it doesn't always go so well unfortunately).
This is why I'm very happy to announce that my file transfer code has been successfully tested against Gajim, a well established desktop XMPP client, as well as against Conversations, a popular XMPP client for Android.

## Demo Application
Since my works is mostly invisible library code, I wrote a small application to demonstrate some features.
_XMPP_SYNC_ is a command line application, which can be used to synchronize files between computers via (encrypted) Jingle File Transfer.

A blog post about _XMPP_SYNC_ can be found [here](https://blogs.fsfe.org/vanitasvitae/2017/08/14/149/) and the source code for it is available in [this 
repository](https://github.com/vanitasvitae/xmpp_sync).

## Feedback on XEPs

### Mailing List
During my time implementing the XEPs, I often gave feedback to the XSFs standards mailing list.

 - [Encrypted Jingle File Transfers](https://mail.jabber.org/pipermail/standards/2017-June/032871.html)
 - [Ciphers XEP?](https://mail.jabber.org/pipermail/standards/2017-July/033063.html)
 - [XEP-0166 - Missing unsuported-security action](https://mail.jabber.org/pipermail/standards/2017-July/033084.html)
 - [XEP-0166 - Multiple Contents in transport-info message](https://mail.jabber.org/pipermail/standards/2017-July/033075.html)
 - [XEP-0234 - NonNegativeInteger](https://mail.jabber.org/pipermail/standards/2017-July/033069.html)
 - [Jingle XEPs - Use of session-info vs. description-info](https://mail.jabber.org/pipermail/standards/2017-July/033079.html)
 - [XEP-0234 - Wrong date format](https://mail.jabber.org/pipermail/standards/2017-August/033095.html)
 - [XEP-0260 - Redundant DstAddr](https://mail.jabber.org/pipermail/standards/2017-August/033094.html)
 - [XEP-0234 - Add option to request hash of offered file](https://mail.jabber.org/pipermail/standards/2017-August/033112.html)
 - [XEP-0234 - Denote used hash function in session-initiate](https://mail.jabber.org/pipermail/standards/2017-August/033116.html)
 - [XEP-0234 - Missing error condition for checksum mismatch](https://mail.jabber.org/pipermail/standards/2017-August/033126.html)

### XEP PRs
Whenever I could, I created Pull Request to address errors I found in the XEPs.

 - [XEP-0065 - Remove duplicate example](https://github.com/xsf/xeps/pull/471)
 - [Proto-XEP JET](https://github.com/xsf/xeps/pull/475)
 - [XEP-0234 - Fix xml in examples](https://github.com/xsf/xeps/pull/478)
 - [XEP-0234 - Add missing length attribute to XML schemas](https://github.com/xsf/xeps/pull/489)
 - [XEP-0234 - Fix wrong date format to conform with XEP-0082](https://github.com/xsf/xeps/pull/491)
 - [XEP-0234, XEP-0300 - Add hash-used element](https://github.com/xsf/xeps/pull/497)
