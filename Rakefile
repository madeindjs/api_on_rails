LANGS = %w[en fr].freeze

def parse_lang(args)
  lang = args[:lang]
  unless LANGS.include?(lang)
    msg = format('Lang not availaible. Select on of theses langs: %s', LANGS.join(', '))
    raise ArgumentError, msg
  end

  lang
end

def out_filename(lang, extension)
  "api_on_rails-#{lang}.#{extension}"
end

namespace :build do
  desc 'Build a PDF version'
  task :pdf, [:lang] do |_task, args|
    lang = parse_lang(args)
    filename = out_filename lang, 'pdf'
    `asciidoctor-pdf #{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
    puts "Book compiled on build/#{filename}"
  end

  desc 'Build an HTML version'
  task :html, [:lang] do |_task, args|
    lang = parse_lang(args)
    filename = out_filename lang, 'html'
    `asciidoctor #{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
    puts "Book compiled on build/#{filename}"
  end

  desc 'Build an EPUB version'
  task :epub, [:lang] do |_task, args|
    lang = parse_lang(args)
    filename = out_filename lang, 'epub'
    `asciidoctor-epub3 #{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
    puts "Book compiled on build/#{filename}"
  end

  desc 'Build a MOBI version'
  task :mobi, [:lang] do |_task, args|
    lang = parse_lang(args)
    filename = out_filename lang, 'mobi'
    `asciidoctor-epub3 #{lang}/api_on_rails.adoc --destination-dir build -a ebook-format=kf8 --out-file #{filename}`
    puts "Book compiled on build/api_on_rails-#{lang}-kf8.epub"
  end
end
