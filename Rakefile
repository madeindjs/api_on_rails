LANGS = %w[en fr].freeze

def parse_lang(args)
  lang = args[:lang]
  unless LANGS.include?(lang)
    msg = format('Lang not availaible. Select on of theses langs: %s', LANGS.join(', '))
    raise ArgumentError, msg
  end

  lang
end

def out_filename(lang, extension, version)
  "api_on_rails_#{version}-#{lang}.#{extension}"
end

namespace :rails5 do
  namespace :build do
    desc 'Build all versions'
    task :all, [:lang] do |_task, args|
      lang = parse_lang(args)
      Rake::Task['rails5:build:pdf'].invoke(lang)
      Rake::Task['rails5:build:epub'].invoke(lang)
      Rake::Task['rails5:build:mobi'].invoke(lang)
    end

    desc 'Build a PDF version'
    task :pdf, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'pdf', 5
      `asciidoctor-pdf rails5/#{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
      puts "Book compiled on build/#{filename}"
    end

    desc 'Build an HTML version'
    task :html, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'html', 5
      `asciidoctor rails5/#{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
      puts "Book compiled on build/#{filename}"
    end

    desc 'Build an EPUB version'
    task :epub, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'epub', 5
      `asciidoctor-epub3 rails5/#{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
      puts "Book compiled on build/#{filename}"
    end

    desc 'Build a MOBI version'
    task :mobi, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'mobi', 5
      `asciidoctor-epub3 rails5/#{lang}/api_on_rails.adoc --destination-dir build -a ebook-format=kf8 --out-file #{filename}`
      `rm build/api_on_rails-#{lang}-kf8.epub`
      puts "Book compiled on build/api_on_rails-#{lang}.mobi"
    end
  end
end

namespace :rails6 do
  namespace :build do
    desc 'Build all versions'
    task :all, [:lang] do |_task, args|
      lang = parse_lang(args)
      Rake::Task['rails5:build:pdf'].invoke(lang)
      Rake::Task['rails5:build:epub'].invoke(lang)
      Rake::Task['rails5:build:mobi'].invoke(lang)
    end

    desc 'Build a PDF version'
    task :pdf, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'pdf', 6
      `asciidoctor-pdf rails6/#{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
      puts "Book compiled on build/#{filename}"
    end

    desc 'Build an HTML version'
    task :html, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'html', 6
      `asciidoctor rails6/#{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
      puts "Book compiled on build/#{filename}"
    end

    desc 'Build an EPUB version'
    task :epub, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'epub', 6
      `asciidoctor-epub3 rails6/#{lang}/api_on_rails.adoc --destination-dir build --out-file #{filename}`
      puts "Book compiled on build/#{filename}"
    end

    desc 'Build a MOBI version'
    task :mobi, [:lang] do |_task, args|
      lang = parse_lang(args)
      filename = out_filename lang, 'mobi', 6
      `asciidoctor-epub3 rails6/#{lang}/api_on_rails.adoc --destination-dir build -a ebook-format=kf8 --out-file #{filename}`
      `rm build/api_on_rails-#{lang}-kf8.epub`
      puts "Book compiled on build/api_on_rails-#{lang}.mobi"
    end
  end
end
