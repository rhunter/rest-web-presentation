!SLIDE 
# REST and the Web #

!SLIDE bullets incremental
# Bullet Points #

* first point
* second point
* third point

!SLIDE
I’m going to explain hypermedia types by looking at some examples that are probably already familiar.

## I’ll assume that you’re comfortable with: ##

 * Using the World-Wide Web
 * HTTP requests and responses
 * URLs (at least http:// URLs)
 * JSON and XML
 * Atom/RSS feeds (you'll get by without)

!SLIDE
The World-Wide Web is the biggest best example of hypermedia
# “Gosh, that works pretty well” #
* In use for about twenty years
* Lots of different clients over the years
* Lots of unrelated servers
* Capabilities have evolved pretty seriously (Images! Forms! Scripting! Styling!)

!SLIDE bullets
# “What was it that kept that working?” #
A set of architectural constraints
A set of known trade-offs (worse latency for better cacheability)

Roy Fielding’s thesis introduced the term “REST” to describe architectures that accepted these constraints.

!SLIDE bullets
# What REST is not #
* Not about nice, human-friendly URLs (they’re great, but irrelevant to REST)
* Not about CRUD operations with POST/GET/PUT/DELETE (great but not all of it)
* It’s not even necessarily over HTTP (it’s a great, established implementation option)
* Definitely not a silver bullet



!SLIDE
# But REST is awesome, shouldn’t we use it everywhere? #

!SLIDE
REST architectures accept a known set of trade-offs.
# Do those trade-offs make sense? #

!SLIDE
# HTTP API trade-offs #
* SOAP/WS-*
* RPC
* HTTP Type I
* HTTP Type II
* Full-blown REST

!SLIDE
# The Web has a REST architecture #

## Established specs: ##

* HTML (+ CSS + JavaScript)
* URIs
* HTTP
* TLS (SSL)
* TCP/IP

Limited operations (GET/POST) on unlimited resources

The browser doesn’t have any knowledge of what a resource “means” nor does it need to take apart URLs, nor make new URLs

!SLIDE
# HTTP is a pretty mature protocol #
* Ubiquitous - Library support everywhere
* Cache-friendly - multi-layer, transparent, advanced controls
* Troubleshooting-friendly - plain text, dates, user agents, proxyable, forwarded-for
* Layered - use whatever content types you want, HTTP doesn’t care

!SLIDE
# URIs are a pretty universal way of indicating a resource #
* Scheme (“what are we even talking about here?”)
* Authority (only this guy is allowed to muck with it)
* Path (hierarchical + non-heirarchical)
* Query
* Fragment


!SLIDE
Everyone knows regular Web pages, but you can universally locate:

* Files on an FTP server
* SSH-protocol files
* Git-protocol git repositories
* Even content-based addressing
  * data-uri for long
  * magnet links for short


!SLIDE

You can indicate resources that aren’t necessarily “located” anywhere:

* Email addresses
* Phone numbers
* Chat rooms
* Books (ISBN or ISSN)
* American social security numbers
* XML namespaces

…But often it’s more useful to talk about someone’s idea of it

* urn:isbn:991911919
vs
* http://amazon.com/book/isbn/991911919


!SLIDE
# Composable #

Just like relative URLs in web pages.
When you’re on
`http://example.com/houses/for-sale/1`

<table>
<tr><th>Then...</th><th>... goes to ...</th></tr>
<tr><td>/news</td><td>http://example.com/news</td></tr>
<tr><td>../for-rent</td><td>../for-rent</td></tr>
<tr><td>#agent</td><td>#agent</td></tr>
<tr><td>?detail=full</td><td>?detail=full</td></tr>
<tr><td>2</td><td>2</td></tr>
<tr><td>//other.exeample.com</td><td>http://other.example.com/</td></tr>
</table>

## Trivial with pretty much every library and platform ##

    @@@ruby

    #{current_uri + '/news'}

.notes …Except JavaScript in the browser.

!SLIDE
# Link Semantics #
But what’s at that URL? Why would I go there?

!SLIDE
# Relationships #

    @@@html
    <link rel=“stylesheet” href=“pretty.css”>

`pretty.css` is a _stylesheet_ for this document



    @@@html
    <link rel=“icon” href=“favicon.ico”>

`favicon.ico` is an _icon_ for this document



    @@@html
    <link rel=“alternate” href=“index.atom”
          type=“application/atom+xml”>

`index.atom` is an _alternative_ to this document

!SLIDE smbullets

_This thing_ has *this relationship* to _that thing_
(subject, relationship, object)
* An “RDF Triple” (Resource Description Format)

There are special databases called “triple stores” for storing and querying these networks

  * Semantic Web folks say triples are the essence of it all
  * RDF is an XML format to express triples
  * RDF is a big part of RSS
  * JSON-LD brings RDF-style triples to JSON

Master list of unqualified relationships maintained by IANA

It’s polite to submit to IANA or use a URL instead

… but humans prefer shorter names

… lots of people prefer to pretend their custom thing is global

… CURie tries to strike a balance


!SLIDE
# Media types #

Agreed beforehand, ideally in a spec, ideally with multiple independent implementations

Describe a processing model (“how do I do something with this?”)

May describe semantics (HTML) or not (PDF)

Kept to a minimum

!SLIDE
Armed with just HTML/CSS specs and your human mind, your browser supports:

  * Web searches
  * forum posts
  * Wikipedia
  * Internet banking

Web spiders can index lots of the Web without knowing the difference too

Web spiders armed with a bit more knowledge can extract more

… eg sitemap, the “review” microformat, RDF, OpenGraph (Facebook)

Only robots with very specific knowledge can interact with complex stateful workflows

… eg forum spam bots


!SLIDE
Armed with just RSS and Atom, your feed reader supports:

* Blogs
* Online Newspapers
* Podcasts
* Jenkins build status
* Twitter favourites

If all these things can be processed with just a couple of media types,
we can probably make do.


!SLIDE
# Media types on the World Wide Web #

* HTML (the big one)
- Defined in W3C standards
* Images (GIF, JPEG, PNG)
* CSS
* JavaScript
* Plain text, PostScript and PDF, Word documents

!SLIDE bullets
# Hypertext-friendly media types #

HTML (basically the canonical hypertext media type - has links, forms)

XHTML (same thing but has sockets for other things)

AtomPub (links)

Even PDF and Word documents

.notes But you wouldn’t consider them for most APIs

!SLIDE
# What about XML and JSON? #
XML is a markup language with no semantics of its own.
JSON is a notation with no semantics of its own.
*Not* plain old XML (no links)
*Not* plain old JSON (no links)

!SLIDE
# Atom is great #

A list of arbitrary “things” (HTML is popular)

* Supports links
* Specifies how to make and change “things”
* Specifies paging and archiving too (extension spec)
* Roy Fielding likes it
* Relatively popular library support
  * … but it’s XML and XML is out of fashion (fun fact: 4 in 5 rubyists are allergic to XML)
  * … but the libraries usually only support either reading or writing
  * … but the libraries seldom support paging and archiving

!SLIDE
# ATOM #

    @@@xml
    <feed
      xmlns:xn=“http://example.org/named-person”>
      <item>
        <xn:name>John Foo</xn:name>
      </item>
      <item>
        <xn:name>Barry Bar</xn:name>
      </item>
      <item>
        <xn:name>Bruce Baz</xn:name>
      </item>
    </feed>


!SLIDE smbullets
# HTML is great #

* Super well supported — everything works with it
* Really familiar model — everyone works with it
* Supports links
* Supports forms (limited but good enough)
* Supports “a list of things”
  * … But those things have to be more HTML
  * … XHTML
* Designed for humans first, machines second
  * … Often designed for sighted humans using a visual browser with a mouse and keyboard
* Better at marking up text than data structures
  * … Although there are ways to make it easier (microformats, XHTML)

!SLIDE
# HTML #

    @@@html
    <ol>
      <li>
        <a href=“/foo” class=“xn-name”>
          John Foo
        </a>
      </li>
      <li>
        <a href=“/foo” class=“xn-name”>
          Barry Bar
        </a>
      </li>
      <li>
        <a href=“/foo” class=“xn-name”>
          Bruce Baz
        </a>
      </li>
    </ol>
    <form action=“/people” method=“post”>
      <input name=“name” type=“text”>
    </form>

!SLIDE smbullets
# HAL is new #

* Like HTML but for computers first
* Has a JSON encoding and an XML encoding
* Supports links
  * … Including URI templating
  * … but no forms (separate spec, not finished yet)
* Tool support is present
  * … but not standardised yet
  * … Check out the HAL Browser

!SLIDE
# HAL #
    @@@javascript
    {
      people: [
    {_links: [{rel: “self”, href: “/foo”}], Name: “John Foo”},
    {_links: [{rel: “self”, href: “/bar”}], Name: “Barry Bar”}
    {_links: [{rel: “self”, href: “/baz”}], Name: “Bruce Baz”}
      ],
      _controls: [
      Rel: “create”,
      Href: “/people”
      Template: {
        Name: “”
      },
    _links: [{rel: “self”, href: “/people”}]
      ]

!SLIDE
# JSON-LD #

Like JSON but with explicit markers for “what does this mean”. 
Doesn’t have form semantics, although it does assume JSONSchema so it’s a natural leap to using them for POSTs.

!SLIDE

    @@@javascript
    {
    people: [
      {@Id: “xn:person”, Name: “John Foo”},
      {@Id: “xn:person”, Name: “Barry Bar”},
      {@Id: “xn:person”, Name: “Bruce Baz”},
    ]
    }

!SLIDE
# Other options #

RDF

* Make your own
  * (XML+XLink+XForms)
  * (JSON+JSONSchema+your own links+your own forms)
!SLIDE
# One format I've used #
## JSON + JSONSchema + your own links + your own forms ##

    @@@javascript
    {
      Items: [
    {Name: “John Foo”},
    {Name: “Barry Bar”},
    {Name: “Bruce Baz”}
      ]
      Links: [
      Rel: “create”,
      Form: {
        Type: “application/x-www-formencoding”,
        Inputs: [
          {Name: {type: “string”, mandatory: true}}
        ]
      }
      ]

!SLIDE
Questions? Observations?

!SLIDE
Thanks.

