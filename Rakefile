require 'rake/gempackagetask'
require 'rubygems/specification'
require 'spec/rake/spectask'
require 'date'
require 'bundler'

Bundler.setup(:runtime, :test)
Bundler.require(:runtime, :test)

require 'lib/rack/hoptoad'

GEM = "rack_hoptoad"
GEM_VERSION = Rack::Hoptoad::VERSION
AUTHOR = "Corey Donohoe"
EMAIL = "atmos@atmos.org"
HOMEPAGE = "http://github.com/atmos/rack_hoptoad"
SUMMARY = "A gem that provides hoptoad notifications from rack"

spec = Gem::Specification.new do |s|
  s.name             = GEM
  s.version          = GEM_VERSION
  s.platform         = Gem::Platform::RUBY
  s.has_rdoc         = true
  s.extra_rdoc_files = ["LICENSE", 'TODO']
  s.summary          = SUMMARY
  s.description      = s.summary
  s.author           = AUTHOR
  s.email            = EMAIL
  s.homepage         = HOMEPAGE

  bundle = Bundler::Definition.from_gemfile("Gemfile")
  bundle.dependencies.
    select { |d| d.groups.include?(:runtime) }.
    each   { |d| s.add_dependency(d.name, d.version_requirements.to_s)  }

  s.require_path = 'lib'
  s.files = %w(LICENSE README.md Rakefile TODO) + Dir.glob("{lib,specs}/**/*")
end

Rake::GemPackageTask.new(spec) do |pkg|
  pkg.gem_spec = spec
end

desc "create a gemspec file"
task :make_spec do
  File.open("#{GEM}.gemspec", "w") do |file|
    file.puts spec.to_ruby
  end
end

task :default => 'rack_hoptoad:spec'

namespace :rack_hoptoad do
  desc "Run unit specifications"
  Spec::Rake::SpecTask.new(:spec) do |t|
    t.spec_opts << %w(-fs --color)
    t.spec_opts << '--loadby' << 'random'
    t.spec_files = Dir["spec/*_spec.rb"]

    t.rcov_opts << '--exclude' << 'spec,.bundle'
    t.rcov = ENV.has_key?('NO_RCOV') ? ENV['NO_RCOV'] != 'true' : true
    t.rcov_opts << '--text-summary'
    t.rcov_opts << '--sort' << 'coverage' << '--sort-reverse'
  end
end
