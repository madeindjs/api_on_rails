require 'asciidoctor'
require 'asciidoctor-pdf'

LANGS = %w[en fr es].freeze
VERSIONS = %w[5 6].freeze
OUTPUT_DIR = File.join __dir__, 'build'
THEMES_DIR = File.join __dir__, 'themes'
FONTS_DIR = File.join __dir__, 'fonts'

def check_args(args)
  lang = args[:lang]
  unless LANGS.include?(lang)
    msg = format('Lang not availaible. Select on of theses langs: %s', LANGS.join(', '))
    raise ArgumentError, msg
  end

  version = args[:version]
  unless VERSIONS.include?(version)
    msg = format('Version not availaible. Select on of theses versions: %s', VERSIONS.join(', '))
    raise ArgumentError, msg
  end

  lang
end

def in_filename(args)
  format('rails%s/%s/api_on_rails.adoc', args[:version], args[:lang])
end

def out_filename(args, extension)
  format("api_on_rails_%s-%s.%s",args[:version], args[:lang], extension)
end

namespace :build do
  desc 'Build all versions'
  task :all, [:version, :lang] do |_task, args|
    check_args(args)
    Rake::Task['build:pdf'].invoke(args[:version], args[:lang])
    Rake::Task['build:epub'].invoke(args[:version], args[:lang])
    Rake::Task['build:mobi'].invoke(args[:version], args[:lang])
  end

  desc 'Build a PDF version'
  task :pdf, [:version, :lang] do |_task, args|
    check_args(args)

    input = in_filename args
    output = out_filename args, 'pdf'

    Asciidoctor.convert_file input,
                             safe: :unsafe,
                             backend: 'pdf',
                             to_dir: OUTPUT_DIR,
                             mkdirs: true,
                             to_file: output,
                             attributes: {
                               'pdf-stylesdir' => THEMES_DIR,
                               'pdf-style' => 'my',
                               'pdf-fontsdir' => FONTS_DIR,
                             }


    # `asciidoctor-pdf #{input} --destination-dir build --out-file #{output}`
    puts "Book compiled on build/#{output}"
  end

  desc 'Build an HTML version'
  task :html, [:version, :lang] do |_task, args|
    check_args(args)
    input = in_filename args
    output = out_filename args, 'html'
    `asciidoctor #{input} --destination-dir build --out-file #{output}`
    puts "Book compiled on build/#{output}"
  end

  desc 'Build an EPUB version'
  task :epub, [:version, :lang] do |_task, args|
    check_args(args)
    input = in_filename args
    output = out_filename args, 'epub'
    `asciidoctor-epub3 #{input} --destination-dir build --out-file #{output}`
    puts "Book compiled on build/#{output}"
  end

  desc 'Build a MOBI version'
  task :mobi, [:version, :lang] do |_task, args|
    check_args(args)
    input = in_filename args
    output = out_filename args, 'mobi'
    `asciidoctor-epub3 #{input} --destination-dir build -a ebook-format=kf8 --out-file #{output}`
    puts "Book compiled on build/#{output}"
  end
end
