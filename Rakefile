require "fileutils"
require "pp"
require 'yaml'

# move to gem, or at least into /lib
def sluggify(text, separator="-")
  text.downcase.gsub(/_|\s|\W/, separator).gsub(/-{2,}/, separator).gsub(/(-)+$/, "")
end

def meta(key=nil)
  meta = YAML.load_file('_config.yml')

  if key.nil?
    meta
  else
    meta[key]
  end
end



desc "Default task. Run with `rake`."
task default: ["book"]

desc "Builds the whole book."
task book: ["book:table_of_contents", "book:pages"]

namespace :book do
  desc "Creates the table of contents for the book."
  task :table_of_contents do
    content = []

    content << "---
layout: default
title: Table of Contents : Shoot It Yourself, Ignacio Galvez
---"
    content << "# Table of Contents\n"

    pages = YAML.load_file('_data/pages.yml')

    pages.each do |page|
      page_title = page["name"]
      page_slug  = sluggify(page_title)

      content << "- [#{page_title}](/pages/#{page_slug})"
    end

    content = content.join("\n") + "\n"

    FileUtils::mkdir_p "pages"
    File.open("pages/index.md", 'w+') { |f| f.write(content) }
  end

  desc "Creates the pages for the book."
  task :pages do
    pages = YAML.load_file('_data/pages.yml')

    pages.each do |page|
      page_title = page["name"]
      page_slug  = sluggify(page_title)
      photos     = page["photos"]

      page_content = %Q(---
layout: default
title: #{page_title} : #{meta("book_title")}, #{meta("book_author")}
---

# #{page_title}

)

      photos.times do |index|
        page_content << "![#{page_title}](#{meta("asset_path")}#{page_slug}-#{index + 1}.jpg)\n"
      end

      FileUtils::mkdir_p "pages/#{page_slug}"
      File.open("pages/#{page_slug}/index.md", 'w+') { |f| f.write(page_content) }
    end
  end

  desc "Prints variables for the book."
  task :vars do
    if meta.is_a?(Hash)
      pp meta
    else
      puts meta
    end
  end
end
