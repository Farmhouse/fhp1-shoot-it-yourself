require "fileutils"
require "pp"

# move to gem, or at least into /lib
def sluggify(text, separator="-")
  text.downcase.gsub(/_|\s|\W/, separator).gsub(/-{2,}/, separator).gsub(/(-)+$/, "")
end

def meta(key=nil)
  require 'yaml'
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

    content << "# Table of Contents\n"

    File.open("_dev/pages.txt", "r").each_line do |line|
      page_title = line.chomp
      page_slug  = sluggify(page_title)

      content << "- [#{page_title}](/#{page_slug})"
    end

    content = content.join("\n") + "\n"

    FileUtils::mkdir_p "_includes"
    File.open("_includes/table-of-contents.md", 'w+') { |f| f.write(content) }
  end

  desc "Creates the pages for the book."
  task :pages do
    File.open("_dev/pages.txt", "r").each_line do |line|
      page_title = line.chomp
      page_slug  = sluggify(page_title)

      page_content = %Q(
---
layout: default
title: #{page_title} : #{meta("book_title")}, #{meta("book_author")}
---

# #{page_title}

![#{page_title}](#{meta("asset_path")}#{page_slug}-1.jpg)
)

      FileUtils::mkdir_p "#{page_slug}"
      File.open("#{page_slug}/index.md", 'w+') { |f| f.write(page_content) }
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
