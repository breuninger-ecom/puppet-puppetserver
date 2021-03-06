require 'puppetlabs_spec_helper/rake_tasks'
require 'puppet-lint/tasks/puppet-lint'

Rake::Task[:lint].clear
PuppetLint::RakeTask.new :lint do |config|
  config.fail_on_warnings = true
  config.log_format = '%{path}:%{linenumber}:%{check}:%{KIND}:%{message}'
  config.ignore_paths = [
    'spec/**/*.pp',
    'pkg/**/*.pp',
    'vendor/**/*.pp',
  ]
  config.disable_checks = [
    '80chars',
    'autoloader_layout',
  ]
end

PuppetSyntax.exclude_paths = ["spec/fixtures/**/*.pp", "vendor/**/*"]

desc "Validate manifests, templates, and ruby files"
task :validate do
  Dir['manifests/**/*.pp'].each do |manifest|
    sh "puppet parser validate --noop #{manifest}"
  end
  Dir['spec/**/*.rb','lib/**/*.rb'].each do |ruby_file|
    sh "ruby -c #{ruby_file}" unless ruby_file =~ /spec\/fixtures/ || ruby_file =~ /spec\/unit/
  end
  Dir['templates/**/*.erb'].each do |template|
    sh "erb -P -x -T '-' #{template} | ruby -c"
  end
end
