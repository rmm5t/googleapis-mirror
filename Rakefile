task :default => :serve

IP_ADDRESS = "172.16.88.88"
HOSTNAME   = "ajax.googleapis.com"

desc "[run as sudo] Creates a virtual IP address, maps ajax.googleapis.com to that IP, and starts a web server to serve a local mirror of ajax.googleapis.com"
task :serve do
  prepare_for_exit
  start_virtual_server
end

desc "Downloads all libraries from ajax.googleapis.com that are listed in libraries.txt"
task :sync do
  File.foreach("libraries.txt") do |url|
    download(url)
  end
end

def safe_require(require_path, gem_name = require_path)
  require require_path
rescue LoadError
  puts "You are missing a required dependency. Please run:"
  puts "  gem install #{gem_name}"
  exit 1
end

require "webrick"
safe_require "ghost"

def prepare_for_exit
  trap(0)     { cleanup_and_exit }
  trap("INT") { cleanup_and_exit }
  puts "\nPress Ctrl-C at any time to shutdown\n"
end

def start_virtual_server
  create_virtual_ip
  map_hostname
  start_web_server
end

def stop_virtual_server
  stop_web_server
  unmap_hostname
  delete_virtual_ip
end

def cleanup_and_exit
  return if @cleaned
  puts
  stop_virtual_server
  puts "Bye."
  @cleaned = true
end

def create_virtual_ip
  sh("ifconfig lo0 alias #{IP_ADDRESS}")
end

def delete_virtual_ip
  sh("ifconfig lo0 -alias #{IP_ADDRESS}")
end

def map_hostname
  sh("ghost add #{HOSTNAME} #{IP_ADDRESS}")
end

def unmap_hostname
  sh("ghost delete #{HOSTNAME}")
end

def start_web_server
  @server = WEBrick::HTTPServer.new(:BindAddress => IP_ADDRESS, :Port => 80, :DocumentRoot => Dir.pwd)
  @server.start
end

def stop_web_server
  @server.shutdown if @server
end

def download(url)
  url, path = url_and_path(url)
  return unless url
  dir = File.dirname(path)
  FileUtils.mkdir_p(dir)
  if File.exists?(path)
    puts "#{path} already exists"
  else
    sh("curl '#{url}' > '#{path}'")
  end
end

def url_and_path(url)
  return if url.nil? || url.strip.empty?
  url.strip!
  path = url.gsub("http://#{HOSTNAME}/", "")
  [url, path]
end
