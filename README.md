# Sending GET Requests

## Learning Goals

- Make a request to a remote resource
- Handle the response
- Parse JSON

## Introduction

Simple Ruby applications are capable of powerful feats. We can use them to
handle all sorts of routine tasks on our computer. We can use them to represent
real-world relationships and systems. It's like we've been given an infinite
multi-tool that can help us work all kinds of data and make it useful.

The limitation at this point, it seems, is really that we don't have enough data
to play with. Luckily, we have a massive source of data available to us: the
Internet!

We typically experience the Internet through webpages in a browser, but a lot of
information is accessible as pure data. If we know the tools to retrieve and
organize that data, we'll have access to a wealth of knowledge. In this lesson,
we're going to introduce some of the basic ways in which we can get remote data
using Ruby.

## Making an HTTP Request through the Internet

When we go to a website in the browser, we type in a URL and press enter. When
enter is pressed, the browser sends a request to that URL. The request travels
to a server somewhere in the world that is hosting the website we want. The
server sends back a response. This response includes the HTML code structure of
the website, which your browser uses to build the page for you in front of your
eyes.

This type of request is known as a HTTP GET request. We are simply retrieving
information at a specified location. There are [other types of requests][] for
sending, updating and deleting information that we explore in later lessons,
but for now, we will focus on the GET request. Don't worry though, the GET
request can... _get_ us quite far on its own!

[other types of requests]: https://www.w3schools.com/tags/ref_httpmethods.asp

In Ruby code, we don't have the luxurious graphical user interface of a browser,
but we can still send GET requests in a similar fashion using built-in Ruby
modules and classes.

## Sending an HTTP GET Request Using Ruby

To show how to make requests in Ruby, we'll first make an HTTP GET request in
IRB. To start, once IRB is open, we need to require [`open-uri`][]:

```ruby
require 'open-uri'
```

If loaded correctly, this should return `true`. Next, we will set a URL that we
want to get data from. For this example, we'll get data from a GitHub repository
that is hosting a webpage:
[https://learn-co-curriculum.github.io/json-site-example/][]. We can store this
URL in a variable:

```ruby
url = "https://learn-co-curriculum.github.io/json-site-example/"
#=> "https://learn-co-curriculum.github.io/json-site-example/"
```

The first thing we do is pass this `url` variable into a method called `parse`
that is part of the URI module we loaded with `require 'open-uri'`:

```ruby
uri = URI.parse(url)
#=> #<URI::HTTPS https://learn-co-curriculum.github.io/json-site-example/>
```

**Aside**: [URI][] stands for Universal Resource Identifier. URIs, in short, are
a standard way to name a resource. [URL][] stands for Universal Resource
Locator. They are a standard way to _locate_ something. **URLs also happen to
act as a standard name** (a web address acts as both the name of the website
_and_ the text you need to enter to visit the website). Therefore, generally
speaking, all URLs are considered a subset of URIs (but not all URIs are URLs).

By parsing our URL with `URI.parse`, we've stored the URL in a class object and
gained access to some powerful methods. The main one we want to use is
`open`:

```ruby
uri.open
#=> #<StringIO:0x00007fa84b13de90 @base_uri=#<URI::HTTPS https://learn-co-curriculum.github.io/json-site-example/>, @meta={"server"=>"GitHub.com", "content-type"=>"text/html; charset=utf-8", "last-modified"=>"Thu, 15 Aug 2019 23:05:32 GMT", "etag"=>"W/\"5d55e53c-284\"", "access-control-allow-origin"=>"*", "expires"=>"Thu, 22 Aug 2019 19:42:27 GMT", "cache-control"=>"max-age=600", "x-proxy-cache"=>"MISS", "x-github-request-id"=>"D49E:101C:257158:30F953:5D5EEDC6", "content-length"=>"341", "accept-ranges"=>"bytes", "date"=>"Thu, 22 Aug 2019 19:38:38 GMT", "via"=>"1.1 varnish", "age"=>"214", "connection"=>"keep-alive", "x-served-by"=>"cache-ewr18140-EWR", "x-cache"=>"HIT", "x-cache-hits"=>"1", "x-timer"=>"S1566502719.880905,VS0,VE1", "vary"=>"Accept-Encoding", "x-fastly-request-id"=>"bf2550e15c1927f0d65dc31f98747ad56e51d4db"}, @metas={"server"=>["GitHub.com"], "content-type"=>["text/html; charset=utf-8"], "last-modified"=>["Thu, 15 Aug 2019 23:05:32 GMT"], "etag"=>["W/\"5d55e53c-284\""], "access-control-allow-origin"=>["*"], "expires"=>["Thu, 22 Aug 2019 19:42:27 GMT"], "cache-control"=>["max-age=600"], "x-proxy-cache"=>["MISS"], "x-github-request-id"=>["D49E:101C:257158:30F953:5D5EEDC6"], "content-length"=>["341"], "accept-ranges"=>["bytes"], "date"=>["Thu, 22 Aug 2019 19:38:38 GMT"], "via"=>["1.1 varnish"], "age"=>["214"], "connection"=>["keep-alive"], "x-served-by"=>["cache-ewr18140-EWR"], "x-cache"=>["HIT"], "x-cache-hits"=>["1"], "x-timer"=>["S1566502719.880905,VS0,VE1"], "vary"=>["Accept-Encoding"], "x-fastly-request-id"=>["bf2550e15c1927f0d65dc31f98747ad56e51d4db"]}, @status=["200", "OK"]>
```

When we type `uri.open` in IRB, we get back an object _full of data!_ As it
turns out, we've actually _just made a GET request!_ The returned result, in
this case, is a [`StringIO`][] object, _which is not a `String`_, but can be
converted into one using a built-in `string` method. So we could write the
following:

```ruby
request_result = uri.open
request_result.string
```

Or just chain the methods:

```ruby
uri.open.string
```

The result? HTML as a String!

```ruby
"<!DOCTYPE html>\n<html lang=\"en\">\n<head>\n  <meta charset=\"UTF-8\">\n  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n  <meta http-equiv=\"X-UA-Compatible\" content=\"ie=edge\">\n  <title>JSON Example Site</title>\n</head>\n<body>\n  <h1>JSON Example Site</h1>\n\n  <h3>Endpoints available to visit:</h3>\n  <ul>\n    <li><a href=\"https://learn-co-curriculum.github.io/json-site-example/endpoints/locations.json\" alt=\"locations JSON\">endpoints/locations.json</a></li>\n    <li><a href=\"https://learn-co-curriculum.github.io/json-site-example/endpoints/people.json\" alt=\"people JSON\">endpoints/people.json</a></li>\n  </ul>\n</body>\n</html>"
```

We requested and received a _webpage_. While it isn't rendered nicely in the way
a browser would display this, it is a great start! And just to review, we did
this using only four lines of Ruby:

```ruby
require 'open-uri'
url = "https://learn-co-curriculum.github.io/json-site-example/"
uri = URI.parse(url)
uri.open.string
```

This is super cool, but working with `StringIO` can be a little limiting. To
stick with conventions, we're going to look at a slightly different approach
next.

## Reading the Body of a Response

Rather than use the `open` method, we're going to bring in another built-in Ruby
class, [`NET::HTTP`][]. `URI` (also known as `OpenURI`) is actually built using
`NET::HTTP`. `NET::HTTP` will allow us to get back an object closer to the
structure of the actual HTTP response being sent.

Starting from a new IRB session, we'll first require `open-uri` as before, but
we will also need to add a second require line:

```ruby
require 'open-uri'
require 'net/http'
```

Then, we'll keep the URL definition and URI parsing the same:

```ruby
url = "https://learn-co-curriculum.github.io/json-site-example/"
uri = URI.parse(url)
```

But instead of `uri.open` at the end, we'll enter the following:

```ruby
response = Net::HTTP.get_response(uri)
#=> #<Net::HTTPOK 200 OK readbody=true>
```

Here again we've sent a GET request, only this time, the return value
isn't a `StringIO` object, but a `Net::HTTPOK` object. This object
has a method, `body`, that should produce a familiar sight when used:

```ruby
response.body
#=> "<!DOCTYPE html>\n<html lang=\"en\">\n<head>\n  <meta charset=\"UTF-8\">\n  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">\n  <meta http-equiv=\"X-UA-Compatible\" content=\"ie=edge\">\n  <title>JSON Example Site</title>\n</head>\n<body>\n  <h1>JSON Example Site</h1>\n\n  <h3>Endpoints available to visit:</h3>\n  <ul>\n    <li><a href=\"https://learn-co-curriculum.github.io/json-site-example/endpoints/locations.json\" alt=\"locations JSON\">endpoints/locations.json</a></li>\n    <li><a href=\"https://learn-co-curriculum.github.io/json-site-example/endpoints/people.json\" alt=\"people JSON\">endpoints/people.json</a></li>\n  </ul>\n</body>\n</html>\n"
```

So, we've seen two ways to get the HTML code from a webpage. Although this is
cool, in its current state, this isn't very useful information. There are
actually tools designed specifically to take this raw HTML and turn it into
organized data for us in a process known as scraping, but we don't need to learn
that just yet. Plenty of data is already organized and made available to us
separate from HTML code and we can retrieve it the same way we send a GET
request to a website.

## Define JSON

JSON stands for JavaScript Object Notation and is a standard way store and
transfer nested data over the internet. The keyword here is **Notation**. Data
stored in JSON format is actually just data stored _as a string_, but structured
in a way that is easily converted into usable nested data. Ruby has a built-in
[`JSON`][] module that includes a `parse` method to take JSON formatted data and
turn it into an array or hash.

## Retrieving JSON Data

Just as with the website, we write in the _Universal Resource Locator_, convert
it to a `URI` object, then send a GET request. Previously, we sent requests to
[https://learn-co-curriculum.github.io/json-site-example/][]. The server, in
this case, had an HTML file to serve in response. If we request a slightly
different URL, we'll get a different response. This site has some example data
stored as JSON on it. By changing out the request to
[https://learn-co-curriculum.github.io/json-site-example/endpoints/locations.json][],
instead of responding with HTML, this time, the server will send the JSON data.

```ruby
require 'open-uri'
require 'net/http'
url = "https://learn-co-curriculum.github.io/json-site-example/endpoints/locations.json"
uri = URI.parse(url)
response = Net::HTTP.get_response(uri)
response.body
```

Running the above in in a fresh IRB session, you should see that the `response.body` now
returns a string of data:

```ruby
"[\n  {\n    \"name\": \"Flatiron School Manhattan\",\n    \"address\": \"11 Broadway, New York, NY 10004\",\n    \"coordinates\": {\n      \"latitude\": \"40.704521\",\n      \"longitude\": \"-74.012833\"\n    }\n  },\n  {\n    \"name\": \"Flatiron School Austin\",\n    \"address\": \"316 W 12th St, Austin, TX 78701\",\n    \"coordinates\": {\n      \"latitude\": \"30.275080\",\n      \"longitude\": \"-97.743700\"\n    }\n  },\n  {\n    \"name\": \"Flatiron School Denver\",\n    \"address\": \"3601 Walnut St 5th Floor, Denver, CO 80205\",\n    \"coordinates\": {\n      \"latitude\": \"39.743510\",\n      \"longitude\": \"-105.011360\"\n    }\n  },\n  {\n    \"name\": \"Flatiron School Seattle\",\n    \"address\": \"1411 4th Ave 13th Floor, Seattle, WA 98101\",\n    \"coordinates\": {\n      \"latitude\": \"47.684879\",\n      \"longitude\": \"-122.201363\"\n    }\n  },\n  {\n    \"name\": \"Flatiron School London\",\n    \"address\": \"131 Finsbury Pavement, Finsbury, London EC2A 1NT, UK\",\n    \"coordinates\": {\n      \"latitude\": \"51.520480\",\n      \"longitude\": \"-0.087190\"\n    }\n  }\n]"
```

This is JSON! It isn't very friendly to use yet, so we'll want to convert it.
First, we'll need to require the `json` module:

```ruby
require 'json'
```

Then we pass in `response.body` to the JSON parser:

```ruby
JSON.parse(response.body)
```

The result is an array containing five hashes, each with some data about a
Flatiron School campus:

```ruby
[
  {
    "name"=>"Flatiron School Manhattan",
    "address"=>"11 Broadway, New York, NY 10004",
    "coordinates"=>{"latitude"=>"40.704521", "longitude"=>"-74.012833"}
  },
  {
    "name"=>"Flatiron School Austin",
    "address"=>"316 W 12th St, Austin, TX 78701",
    "coordinates"=>{"latitude"=>"30.275080", "longitude"=>"-97.743700"}
  },
  {
    "name"=>"Flatiron School Denver",
    "address"=>"3601 Walnut St 5th Floor, Denver, CO 80205",
    "coordinates"=>{"latitude"=>"39.743510", "longitude"=>"-105.011360"}
  },
  {
    "name"=>"Flatiron School Seattle",
    "address"=>"1411 4th Ave 13th Floor, Seattle, WA 98101",
    "coordinates"=>{"latitude"=>"47.684879", "longitude"=>"-122.201363"}
  },
  {
    "name"=>"Flatiron School London",
    "address"=>"131 Finsbury Pavement, Finsbury, London EC2A 1NT, UK",
    "coordinates"=>{"latitude"=>"51.520480", "longitude"=>"-0.087190"}
  }
]
```

**Aside**: If you'd like a better view of this data, require `awesome_print` and
then try typing `ap JSON.parse(response.body)` to see a better output.

## Conclusion

If you've been following along, you've made three HTTP GET requests! We can make
these requests using a URL and some Ruby tools. Some data is readily available
as JSON, which we can retrieve and convert into Ruby data structures.

Having these tools unlocks access to _a lot of data_. Being able to communicate
with remote resources is also the cornerstone of web development!

[`json`]: https://ruby-doc.org/stdlib-2.6.3/libdoc/json/rdoc/JSON.html
[`net::http`]: https://ruby-doc.org/stdlib-2.6.3/libdoc/net/http/rdoc/Net/HTTP.html
[`stringio`]: https://ruby-doc.org/stdlib-2.5.1/libdoc/stringio/rdoc/StringIO.html
[uri]: https://en.wikipedia.org/wiki/Uniform_Resource_Identifier
[url]: https://en.wikipedia.org/wiki/URL
[https://learn-co-curriculum.github.io/json-site-example/endpoints/locations.json]: https://learn-co-curriculum.github.io/json-site-example/endpoints/locations.json

[https://learn-co-curriculum.github.io/json-site-example/]: [https://learn-co-curriculum.github.io/json-site-example/]
[`open-uri`]: https://ruby-doc.org/stdlib-2.6.3/libdoc/open-uri/rdoc/OpenURI.html
