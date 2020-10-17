# require 'pry'
# require 'rlayout'
task :default => :say_hi 

task :say_hi do
  puts "Hello gitbub-action is workinh Horay!!!!"
end


# task :default => :update_section_pdf
source_files  = FileList["**/*.md", "**/*.markdown"]
puts "+++++++++++ source_files: ++++++++++"
# puts source_files

story_pdfs    = source_files.ext(".pdf")
puts "+++++++++++ story_pdfs: ++++++++++"
section_pdfs   = FileList["**/section.pdf"]
puts "+++++++++++ section_pdfs +++++++++++ "
# puts section_pdfs

task :pdf => story_pdfs
rule ".pdf" => ".md" do |t|
  sh "touch #{t.name}"
  RLayout::NewsBoxMaker.new(story_path: t.source)
  # sh "/Applications/newsman.app/Contents/MacOS/newsman news_article #{t.source}"
end

# rule ".pdf" => ".markdown" do |t|
#   sh "/Applications/newsman.app/Contents/MacOS/newsman news_article ##{t.source}"
# end

# file 'section.pdf' => source_files.ext(".pdf") do |t|
#   sh "/Applications/newsman.app/Contents/MacOS/newsman section_pdf #{pwd}"
# end

# task :section do
#   sh "/Applications/newsman.app/Contents/MacOS/newsman section_pdf ."
# end

# def section_pdf_file_for(story_pdf)
#   puts story_pdf
#   if File.exist?(File.dirname(File.dirname(story_pdf)) + "/section.pdf")
#     return File.dirname(File.dirname(story_pdf)) + "/section.pdf"
#   elsif File.exist?(File.dirname(File.dirname(File.dirname(story_pdf))) + "/section.pdf")
#     return File.dirname(File.dirname(File.dirname(story_pdf))) + "/section.pdf"
#   elsif File.exist?(File.dirname(File.dirname(File.dirname(File.dirname(story_pdf)))) + "/section.pdf")
#     return File.dirname(File.dirname(File.dirname(File.dirname(story_pdf)))) + "/section.pdf"    
#   else
#     nil
#   end
# end

def story_pdf_files_in_same_page(sectionn_pdf)
  page_folder = File.dirname(sectionn_pdf)
  FileList["#{page_folder}/*/story.pdf", "**/*/story.pdf"]
end

# this is prefered way 
# 1.first update all story.pdf with rule
# 2. check if section.pdf needs updating from story.pdf in same page folders
task :update_section_pdf => :pdf do
  section_pdfs.each do |section|
    story_pdf_files = story_pdf_files_in_same_page(section)
    story_pdf_files.each do |story_pdf|
      page_file = File.expand_path(section)
      story_file = File.expand_path(story_pdf)
      if File.mtime(page_file) < File.mtime(story_file)
        puts story_file
        puts "need updating #{page_file}"
        RLayout::NewsPage.new(section_path: File.dirname(page_file), relayout:true)
        break
      end
    end
  end
end




task :update_page_pdf do
  puts "update page pdf"
  story_pdfs.each do |s|
    story_file = File.expand_path(s)
    page_file = section_pdf_file_for(story_file)
    next unless page_file
    if File.mtime(page_file) < File.mtime(story_file)
      puts "updating #{page_file}..."
      RLayout::NewsPage.new(section_path: page_file, relayout:true)

    end
  end
end
