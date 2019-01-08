LANGS = %w[en fr].freeze

def parse_lang(args)
  lang = args[:lang]
  unless LANGS.include?(lang)
    msg = format('Lang not availaible. Select on of theses langs: %s', LANGS.join(', '))
    raise ArgumentError, msg
  end

  lang
end

namespace :build do
  desc 'Build a PDF version'
  task :pdf, [:lang] do |_task, args|
    `asciidoctor-pdf #{parse_lang(args)}/api_on_rails.adoc --destination-dir build`
    puts 'Book build on build/api_on_rails.pdf'
  end

  desc 'Build an HTML version'
  task :html, [:lang] do |_task, args|
    `asciidoctor #{parse_lang(args)}/api_on_rails.adoc --destination-dir build`
    puts 'Book build on build/api_on_rails.html'
  end

  desc 'Build an EPUB version'
  task :epub, [:lang] do |_task, args|
    `asciidoctor-epub3 #{parse_lang(args)}/api_on_rails.adoc --destination-dir build`
    puts 'Book build on build/api_on_rails.epub'
  end

  desc 'Build a MOBI version'
  task :mobi, [:lang] do |_task, args|
    `asciidoctor-epub3 #{parse_lang(args)}/api_on_rails.adoc --destination-dir build`
    puts 'Book build on build/api_on_rails.mobi'
  end
end
