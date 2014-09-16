#!/usr/bin/env rake
require 'json'
require 'erb'
require 'rake/clean'

DOWNLOADS = ["words.txt", "rejects.txt"]
WORK_PRODUCTS = DOWNLOADS + ["index.html"]
CLOBBER.include(WORK_PRODUCTS)

task :default => WORK_PRODUCTS

desc 'Build words.txt'
file "words.txt" => :json do
  File.open("words.txt", 'w') {|f| f.puts(@wordlist['words.txt']) }
end

desc 'Build rejects.txt'
file "rejects.txt" => :json do
  File.open("rejects.txt", 'w') {|f| f.puts(@wordlist['rejects.txt']) }
end

desc "Build HTML file"
file "index.html" => :json do
  File.open("index.html", "w+") do |f|
    f.write(ERB.new(File.read('src/template.html.erb')).result(binding))
  end
end

task :json => 'src/wordlist.json' do
  @wordlist = JSON.parse(File.read('src/wordlist.json'))

  @stats = {}

  @stats[:counts] = @wordlist['sources'].map do |src|
    "#{@wordlist[src.first].size} words in #{src.first}"
  end.join(', ')

  puts @stats
end
