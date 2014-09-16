#!/usr/bin/env rake
require 'json'
require 'erb'
require 'rake/clean'

DOWNLOADS = ["words.txt", "rejects.txt"]
WORK_PRODUCTS = DOWNLOADS + ["index.html"]
CLOBBER.include(WORK_PRODUCTS + Rake::FileList.new('*.txt'))

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

  @stats[:counts] = Hash[@wordlist['sources'].map do |src|
    [src.first, @wordlist[src.first].size]
  end]

  @stats[:word_lengths] = {}
  @stats[:initial] = {}

  DOWNLOADS.each do |download|
    @stats[:word_lengths][download] = Hash[@wordlist[download].group_by{|w| w.length}.map do |length, words|
      File.open("#{length}-#{download}", "w+") do |f|
        f.write(words.join("\n"))
      end

      [length, words.size]
    end]

    @stats[:initial][download] = Hash[@wordlist[download].group_by{|w| w[0]}.map do |initial, words|
      File.open("#{initial}-#{download}", "w+") do |f|
        f.write(words.join("\n"))
      end

      [initial, words.size]
    end]
  end

  # puts @stats
end
