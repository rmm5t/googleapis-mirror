task :default => :serve

HOSTNAME = "ajax.googleapis.com"

task :serve do
  puts virtual_host_message
  sh("ghost add #{HOSTNAME}")
  trap("INT") do
    puts
    sh("ghost delete #{HOSTNAME}")
    puts "Bye."
    exit 0
  end
  puts "\nPress Ctrl-C to unmap #{HOSTNAME}"
  sleep(1000) while true
end

task :download do
  File.foreach("libraries.txt") do |url|
    next if url.nil? || url.strip.empty?
    url.strip!
    path = url.gsub("http://#{HOSTNAME}/", "")
    dir = File.dirname(path)
    FileUtils.mkdir_p(dir)
    sh("curl '#{url}' > '#{path}'")
  end
end

def safe_require(require_path, gem_name = require_path)
  require require_path
rescue LoadError
  puts "You are missing a required dependency. Please run:"
  puts "  gem install #{gem_name}"
  exit 1
end

safe_require "directory_watcher"
safe_require "ghost"

def virtual_host_message
  <<-EOM

Don't forget to setup a virtual host on a local web server.
Here's an Apache example:

    <VirtualHost *:80>
      ServerName #{HOSTNAME}
      DocumentRoot "/path/to/googleapis"
      <Directory "/path/to/googleapis">
        Options Indexes
        Order allow,deny
        Allow from all
      </Directory>
    </VirtualHost>

EOM
end

