desc 'delete and build'
task :generate do
  sh 'rm -rf _site'
  jekyll
end

def jekyll
  # time to give me generation times
  sh 'time jekyll'
end

desc 'create new post. args: title, future (# of days), slug'
# rake new future=0 title="New post title goes here" slug="slug-override-title"
task :new do
  require 'rubygems'
  require 'chronic'

  title = ENV["title"] || "New Title"
  future = ENV["future"] || 0
  slug = ENV["slug"] ? ENV["slug"].gsub(' ','-').downcase : title.gsub(' ','-').downcase

  if future.to_i < 3
    TARGET_DIR = "_posts"
  else
    TARGET_DIR = "_drafts"
  end

  if future.to_i.zero?
    filename = "#{Time.new.strftime('%Y-%m-%d')}-#{slug}.markdown"
  else
    stamp = Chronic.parse("in #{future} days").strftime('%Y-%m-%d')
    filename = "#{stamp}-#{slug}.markdown"
  end
  path = File.join(TARGET_DIR, filename)
  post = <<-HTML
---
layout: post
title: "TITLE"
---

HTML
  post.gsub!('TITLE', title)
  File.open(path, 'w') do |file|
    file.puts post
  end
  puts "new post generated in #{path}"
  system "open -a textmate #{path}"
end