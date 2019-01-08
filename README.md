<h1 align="center">
  <img src="logo.svg" alt="Api on Rails 5" />
</h1>

<noscript><a href="https://liberapay.com/alexandre_rousseau/donate"><img alt="Donate using Liberapay" src="https://liberapay.com/assets/widgets/donate.svg"></a></noscript>

Update & translation of the [API on Rails (EN)](http://apionrails.icalialabs.com/book) book. This book is written using [Asciidoctor](https://asciidoctor.org).

## Build book

~~~bash
$ git clone https://github.com/madeindjs/api_on_rails/
$ cd api_on_rails
$ bundle install
$ rake build:pdf[fr]
~~~

You can see all build available with `rake -T`

~~~bash
$ rake -T
rake build:epub[lang]  # Build an EPUB version
rake build:html[lang]  # Build an HTML version
rake build:mobi[lang]  # Build a MOBI version
rake build:pdf[lang]   # Build a PDF version
~~~

## License

This book is under [MIT license](https://opensource.org/licenses/MIT) and [Creative Common BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)
