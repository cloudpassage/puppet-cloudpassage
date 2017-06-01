require 'puppetlabs_spec_helper/rake_tasks'
gem 'test-kitchen', '~>1.15.0'
gem 'rspec', '>=3.3.0', '<3.5.0'
require 'rake'
require 'rspec'

namespace :spec do
  require 'rspec/core/rake_task'
  desc 'Run rspec tests'
  RSpec::Core::RakeTask.new(:spec)
end

namespace :integration do
  require 'kitchen/cli'
  require_relative 'libraries/s3.rb'
  ssh_key = S3.show_object_contents(ENV['S3_SSH_Key_Bucket'], ENV['S3_SSH_Key_Name'])
  File.write(ENV['S3_SSH_Key_Name'], ssh_key)
  File.chmod(0400, ENV['S3_SSH_Key_Name'], )
  puts `ls -la`
  File.write('manifests/site_linux.pp', "node default { class {'cloudpassage': agent_key => #{ENV['HALO_AGENT_KEY']} } }")
  File.write('manifests/site.pp', "node default { class { 'cloudpassage': agent_key => #{ENV['HALO_AGENT_KEY']}, package_file => 'cphalo-#{ENV['HALO_AGENT_VERSION']}-win64.exe', package_url => 'https://production.packages.cloudpassage.com/windows/cphalo-#{ENV['HALO_AGENT_VERSION']}-win64.exe', destination_dir => 'C:\\Users\Administrator\Downloads', server_label => 'puppet_windows' } }")

  task :ubuntu do
    desc 'Run ubuntu kitchen-test'
    ENV['KITCHEN_YAML'] = '.kitchen.ubuntu.yml'
    Kitchen::CLI.new([], concurrency: 2, destroy: 'always').test
  end

  task :rhel do
    desc 'Run rhel kitchen-test'
    ENV['KITCHEN_YAML'] = '.kitchen.rhel.yml'
    Kitchen::CLI.new([], concurrency: 2, destroy: 'always').test
  end

  task :amzn do
    desc 'Run amzn kitchen-test'
    ENV['KITCHEN_YAML'] = '.kitchen.amzn.yml'
    Kitchen::CLI.new([], concurrency: 2, destroy: 'always').test
  end

  task :centos do
    desc 'Run centos kitchen-test'
    ENV['KITCHEN_YAML'] = '.kitchen.centos.yml'
    Kitchen::CLI.new([], concurrency: 2, destroy: 'always').test
  end

  task :debian do
    desc 'Run debian kitchen-test'
    ENV['KITCHEN_YAML'] = '.kitchen.debian.yml'
    Kitchen::CLI.new([], concurrency: 2, destroy: 'always').test
  end

  task :oracle do
    desc 'Run oracle kitchen-test'
    ENV['KITCHEN_YAML'] = '.kitchen.oracle.yml'
    Kitchen::CLI.new([], concurrency: 2, destroy: 'always').test
  end

  task :windows do
    desc 'Run kitchen-windows tests'
    ENV['KITCHEN_YAML'] = '.kitchen.windows.yml'
    Kitchen::CLI.new([], concurrency: 4, destroy: 'always').test
  end
end

task travis: [:spec, :lint]