#require "bundler/gem_tasks"
require 'rake'
require 'rake/testtask'
require 'rspec/core/rake_task'
require 'rdoc/task'

task :default => [:rdoc]

desc "Run RSpec unit tests"
RSpec::Core::RakeTask.new(:spec) do |t|
  t.pattern = "./spec/*/*_spec.rb" # don't need this, it's default.
  # make sure ruby can find the superclass and dependencies
  t.ruby_opts = "-I spec/lib -I ../../../common/lib -I ../../../controller/lib -I lib"
end

desc "Generate RDoc output"
Rake::RDocTask.new do |rd|
  rd.main = "README.rdoc"
  rd.rdoc_dir = "doc"
  rd.rdoc_files.include("README.rdoc", "lib/**/*.rb")
end
