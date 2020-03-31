<h1 align="center">
  <img src="rails6/fr/img/logo.svg" alt="Api on Rails 6" />
</h1>

Learn **best practices** to build an **API** using **Ruby on Rails** 5/6. The intention with this book it’s not only to teach you how to build an API with Rails. The purpose is also to teach you how to build **scalable** and **maintainable** API with Rails which means **improve** your current Rails knowledge. In this book you will learn to:

- Build JSON responses
- Use Git for version controlling
- Testing your endpoints
- Optimize and cache the API

This book is based on ["APIs on Rails: Building REST APIs with Rails"](http://apionrails.icalialabs.com/book/). It was initially published in 2014 by [Abraham Kuri](https://twitter.com/kurenn). Since the original work was not maintained, I wanted to update this excellent work. All the source code of this book is available in [Asciidoctor](https://asciidoctor.org/) format on this repository. So don’t hesitate to [fork the project](https://github.com/madeindjs/api_on_rails/fork) if you want to improve it or fix a mistake that I didn’t notice.

Update & translation of the [API on Rails (EN)](http://apionrails.icalialabs.com/book) book. This book is written using [Asciidoctor](https://asciidoctor.org).

## Support the project

As you may know this project take me some times. So if you want to support me you can buy a version on Leanpub:

- Rails 5
  - [English version](https://leanpub.com/apionrails5/)
  - [French version](https://leanpub.com/apionrails5-fr)
- Rails 6
  - [English version](https://leanpub.com/apionrails6/)
  - [French version](https://leanpub.com/apionrails6-fr)
  - [Spanish version](https://leanpub.com/apionrails6-es)

Or you can support me with Liberapay: <noscript><a href="https://liberapay.com/alexandre_rousseau/donate"><img alt="Donate using Liberapay" src="https://liberapay.com/assets/widgets/donate.svg"></a></noscript>

## Build book

~~~bash
$ git clone https://github.com/madeindjs/api_on_rails/
$ cd api_on_rails
$ bundle install
$ rake "build:pdf[6,fr]"
~~~

You can see all build available with `rake -T`

~~~bash
$ rake -T
rake "build:all[version,lang]"   # Build all versions
rake "build:epub[version,lang]"  # Build an EPUB version
rake "build:html[version,lang]"  # Build an HTML version
rake "build:mobi[version,lang]"  # Build a MOBI version
rake "build:pdf[version,lang]"   # Build a PDF version
~~~

## License

This book is under [MIT license](https://opensource.org/licenses/MIT) and [Creative Common BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

## Contributors

- [Oscar Téllez](https://github.com/oscartzgz) (spanish translation)
