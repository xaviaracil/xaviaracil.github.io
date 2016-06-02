task default: %w[serve]

task :tweet do
  ARGV.shift
  t update ARGV
  # By default, rake considers each 'argument' to be the name of an actual task.
  # It will try to invoke each one as a task.  By dynamically defining a dummy
  # task for every argument, we can prevent an exception from being thrown
  # when rake inevitably doesn't find a defined task with that name.
  ARGV.each do |arg|
    task arg.to_sym do ; end
  end
end

# Usage: rake notify
task :notify => ["notify:google", "notify:bing"]
desc "Notify various services that the site has been updated"
namespace :notify do
  desc "Notify Google of updated sitemap"
  task :google do
    begin
      require 'net/http'
      require 'uri'
      puts "* Notifying Google that the site has updated"
      Net::HTTP.get('www.google.com', '/webmasters/tools/ping?sitemap=' + URI.escape('//desarrolloenmac.com/sitemap.xml'))
    rescue LoadError
      puts "! Could not ping Google about our sitemap, because Net::HTTP or URI could not be found."
    end
  end

  desc "Notify Bing of updated sitemap"
  task :bing do
    begin
      require 'net/http'
      require 'uri'
      puts '* Notifying Bing that the site has updated'
      Net::HTTP.get('www.bing.com', '/webmaster/ping.aspx?siteMap=' + URI.escape('//desarrolloenmac.com/sitemap.xml'))
    rescue LoadError
      puts "! Could not ping Bing about our sitemap, because Net::HTTP or URI could not be found."
    end
  end
end


task :serve do
  bundle exec "jekyll serve --config _config.yml,_config.dev.yml"
end
