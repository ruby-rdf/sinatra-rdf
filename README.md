# Linked Data Content Negotiation for Sinatra Applications

This is a [Sinatra][] extension that provides [Linked Data][] content
negotiation for Sinatra applications.

This version is based on [rack-linkeddata][] without the hard dependency on the [linkeddata][] gem, to allow applications to better manage their dependencies

* <http://github.com/datagraph/sinatra-rdf>

[![Gem Version](https://badge.fury.io/rb/sinatra-rdf.svg)](http://badge.fury.io/rb/sinatra-rdf)
[![Build Status](https://travis-ci.org/ruby-rdf/sinatra-rdf.svg?branch=master)](http://travis-ci.org/ruby-rdf/sinatra-rdf)

## Features

* Implements [HTTP content negotiation][conneg] for RDF content types using
  the [`Rack::RDF`][Rack::RDF] middleware.
* Supports all [RDF.rb][]-compatible serialization formats.
* Supports both classic and modular Sinatra applications.

## Examples

### Adding Linked Data content negotiation to a classic Sinatra application

```ruby
#!/usr/bin/env ruby -rubygems
require 'sinatra'
require 'sinatra/rdf'

get '/hello' do
  RDF::Graph.new do |graph|
    graph << [RDF::Node.new, RDF::DC.title, "Hello, world!"]
  end
end
```

### Adding Linked Data content negotiation to a modular Sinatra application

```ruby
#!/usr/bin/env ruby -rubygems
require 'sinatra/base'
require 'sinatra/rdf'

module My
  class Application < Sinatra::Base
    register Sinatra::RDF

    get '/hello' do
      RDF::Graph.new do |graph|
        graph << [RDF::Node.new, RDF::DC.title, "Hello, world!"]
      end
    end
  end
end

My::Application.run! :host => '127.0.0.1', :port => 4567
```

### Adding Linked Data content negotiation to a Rackup application

```ruby
#!/usr/bin/env rackup
require 'sinatra/base'
require 'sinatra/rdf'

module My
  class Application < Sinatra::Base
    register Sinatra::RDF

    get '/hello' do
      RDF::Graph.new do |graph|
        graph << [RDF::Node.new, RDF::DC.title, "Hello, world!"]
      end
    end
  end
end

run My::Application
```

### Testing Linked Data content negotiation using `rackup` and `curl`

    $ rackup doc/examples/config.ru
    
    $ curl -iH "Accept: application/n-triples" http://localhost:9292/hello
    $ curl -iH "Accept: application/turtle" http://localhost:9292/hello
    $ curl -iH "Accept: application/rdf+xml" http://localhost:9292/hello
    $ curl -iH "Accept: application/json" http://localhost:9292/hello
    $ curl -iH "Accept: application/trix" http://localhost:9292/hello
    $ curl -iH "Accept: */*" http://localhost:9292/hello

## Description

`Sinatra::RDF` is a thin Sinatra-specific wrapper around the
[`Rack::RDF`][Rack::RDF] middleware, which implements Linked
Data content negotiation for Rack applications.

At the moment the Sinatra extension simply corresponds
to doing the following manually in a Sinatra application:

```ruby
require 'rack/rdf'

module My
  class Application < Sinatra::Base
    use     Rack::RDF::ContentNegotiation
    helpers Sinatra::RDF::Helpers
    include ::RDF
  end
end
```

See the `Rack::RDF` documentation for more information on the
operation and details of the content negotiation.

## Documentation

* [Sinatra::RDF](https://www.rubydoc.info/github/ruby-rdf/sinatra-rdf/master)

## Dependencies

* [Sinatra](http://rubygems.org/gems/sinatra) (~> 2.0)
* [Rack::RDF](http://rubygems.org/gems/rack-rdf) (~> 3.1)

## Installation

The recommended installation method is via [RubyGems](http://rubygems.org/).
To install the latest official release of the gem, do:

    % [sudo] gem install sinatra-rdf

## Download

To get a local working copy of the development repository, do:

    % git clone git://github.com/ruby-rdf/sinatra-rdf.git

Alternatively, you can download the latest development version as a tarball
as follows:

    % wget http://github.com/ruby-rdf/sinatra-rdf/tarball/master

## References

* <http://www.w3.org/DesignIssues/LinkedData.html>
* <http://linkeddata.org/docs/how-to-publish>
* <http://linkeddata.org/conneg-303-redirect-code-samples>
* <http://www.w3.org/TR/cooluris/>
* <http://www.w3.org/TR/swbp-vocab-pub/>
* <http://patterns.dataincubator.org/book/publishing-patterns.html>

## Authors

* [Arto Bendiken](http://github.com/bendiken) - <http://ar.to/>

## License

`Sinatra::RDF` is free and unencumbered public domain software. For more
information, see <http://unlicense.org/> or the accompanying UNLICENSE file.

[Sinatra]:          http://www.sinatrarb.com/
[Rack]:             http://rack.github.com/
[RDF.rb]:           http://ruby-rdf.github.com/rdf/
[Rack::RDF]:        http://datagraph.rubyforge.org/rack-rdf/
[Linked Data]:      http://linkeddata.org/
[conneg]:           http://en.wikipedia.org/wiki/Content_negotiation
