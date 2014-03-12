desc "Default task. Run with `rake`"
task default: ["book"]

desc "Builds the whole book"
task book: ["book:table_of_contents", "book:pages"]

namespace :book do
  desc "Creates the table of contents for the book"
  task :table_of_contents do
    puts "book:table_of_contents - creates the table of contents for the book"
    # File.open(local_filename, 'w') {|f| f.write(doc) }
  end

  desc "Creates the pages for the book"
  task :pages do
    puts "book:pages - creates the pages for the book"
    # File.open(local_filename, 'w') {|f| f.write(doc) }
  end
end



