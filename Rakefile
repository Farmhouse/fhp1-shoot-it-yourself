require "fileutils"
require "pp"
require 'yaml'

# move to gem, or at least into /lib
def sluggify(text, separator="-")
  text.downcase.gsub(/_|\s|\W/, separator).gsub(/-{2,}/, separator).gsub(/(-)+$/, "")
end

def config(key=nil)
  config = YAML.load_file('_config.yml')

  if key.nil?
    config
  else
    config[key]
  end
end

def create_page(path, slug, title, photos)
  content = %Q(---
layout: default
title: #{title} - {{ site.book_title }}, {{ site.book_author }}
---

# #{title}

)

  photos.times do |index|
    content << "![#{title}]({{ site.book_image_path }}#{slug}-#{index + 1}.jpg)\n"
  end

  File.write path, content
end

desc "Default task. Run with `rake`."
task default: ["book"]

desc "Builds the whole book."
task book: ["book:table_of_contents", "book:pages"]

namespace :book do
  desc "Creates the table of contents for the book."
  task table_of_contents: %w[pages/index.md]

  desc "Creates the pages for the book."
  task :pages do
    pages = YAML.load_file('_data/pages.yml')

    pages.each do |page|
    end
  end

  desc "Prints variables for the book."
  task :config => '_config.yml' do
    if config.is_a?(Hash)
      pp config
    else
      puts config
    end
  end
end

directory 'pages'

file 'pages/index.md' => %w[pages _data/pages.yml] do
  content = []

  content << "---
layout: default
title: Table of Contents - {{ site.book_title }}, {{ site.book_author }}
---"
  content << "{% include table_of_contents_message.html %}\n"

  pages = YAML.load_file('_data/pages.yml')

  pages.each do |page|
    page_title = page["name"]
    page_slug  = sluggify(page_title)

    content << "1. [#{page_title}](/pages/#{page_slug})"
  end

  content = content.join("\n") + "\n"

  File.write("pages/index.md", content)
end

file '.depends.rf' => %w[Rakefile _data/pages.yml] do |t|
  pages = YAML.load_file('_data/pages.yml')

  open t.name, 'w' do |io|
    paths       = []
    directories = {}

    pages.each do |page|
      page_title = page["name"]
      photos     = page["photos"]

      page_slug  = sluggify(page_title)
      page_path  = "pages/#{page_slug}/index.md"

      paths << page_path
      directories["pages/#{page_slug}"] = true

      io.write <<-TASK
file #{page_path.dump} => %w[pages/#{page_slug} _data/pages.yml _config.yml] do |t|
  create_page t.name, #{page_slug.dump}, #{page_title.dump}, #{photos}
end

      TASK
    end

    directories.keys.each do |directory|
      io.write <<-DIRECTORIES
directory #{directory.dump}
      DIRECTORIES
    end

    io.write <<-WIRING

namespace :book do
  task :pages => %w[#{paths.join ' ' }]
end
    WIRING
  end
end

import '.depends.rf'

