#!/usr/bin/env ruby

desc 'Check _site/ for broken links and images (run jekyll build first)'
task :test do
  require 'html-proofer'
  HTMLProofer.check_directory('./_site', empty_alt_ignore: true, disable_external: true).run
end

task default: :test
