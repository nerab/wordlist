#!/usr/bin/env rake
require 'rake/clean'
require 'json'
require 'erb'

SITE = '.'
CLOBBER.include("#{SITE}/*")

task :default => ["#{SITE}/words.txt", "#{SITE}/rejects.txt", "#{SITE}/index.html"]

desc 'Build words.txt'
file "#{SITE}/words.txt" => :json do
  File.open("#{SITE}/words.txt", 'w') {|f| f.puts(@wordlist['words.txt']) }
end

desc 'Build rejects.txt'
file "#{SITE}/rejects.txt" => :json do
  File.open("#{SITE}/rejects.txt", 'w') {|f| f.puts(@wordlist['rejects.txt']) }
end

desc "Build HTML file"
file "#{SITE}/index.html" => :json do
  File.open("#{SITE}/index.html", "w+") do |f|
    f.write(ERB.new(File.read('src/template.html.erb')).result(binding))
  end
end

task :json => 'src/wordlist.json' do
  @wordlist = JSON.parse(File.read('src/wordlist.json'))
  @downloads = []
end
