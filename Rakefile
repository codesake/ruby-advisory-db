require 'yaml'
require 'codesake-dawn'

namespace :lint do
  begin
    gem 'rspec', '~> 2.4'
    require 'rspec/core/rake_task'

    RSpec::Core::RakeTask.new(:yaml)
  rescue LoadError => e
    task :spec do
      abort "Please run `gem install rspec` to install RSpec."
    end
  end

  task :dawn do

    Dir.glob('gems/*/*.yml') do |path|
      advisory = YAML.load_file(path)

      if advisory['cve']
        cve = "CVE-"+advisory['cve']
        puts "CVE #{cve} NOT in dawn v#{Codesake::Dawn::VERSION} knowledge base" unless Codesake::Dawn::KnowledgeBase.new.find(cve)
      end
    end

  end
  task :cve do
    Dir.glob('gems/*/*.yml') do |path|
      advisory = YAML.load_file(path)

      unless advisory['cve']
        puts "Missing CVE: #{path}"
      end
    end
  end

end

task :lint    => ['lint:yaml', 'lint:cve']
task :default => :lint
