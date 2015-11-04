#
#
require 'rubygems'
require 'bundler'

Bundler.require

def help
  puts "== Bump patch version =="
  puts "rake bump['--patch']"

  puts "== Bump minor version =="
  puts "rake bump['--minor']"

  puts "== Bump major version =="
  puts "rake bump['--major']"

  puts "== Upload to artifactry =="
  puts "rake upload['./app.zip', 'username', 'password', 'build_name', 'build_number']"
end

def tags(git_out)
  # See what tags are currently in use
  tags = git_out.split /\r?\n/
  # We only want tags that match the base
  tags = tags.select { |t| t.match /ctct_docker/}

  # Turn the tag into a hash
  def parse_tag(t)
    # Get the version out of the tag
    _v = t.split '-'
    v = _v.pop
    v = v.split '.'
    {
        :major => v[0].to_i,
        :minor => v[1].to_i,
        :patch => v[2].to_i
    }
  end

  # If we have no tags then we start a new history
  if tags.count == 0
    puts 'Creating new tag history'
    version =  {
        :major => 0,
        :minor => 0,
        :patch => 0
    }
  # Else we need to use the most recent tag
  else
    # we need to sort the tags and find the newest one
    _versions = tags.map { |t| parse_tag t}

    _smaller = -1
    _same = 0
    _bigger = 1

    _versions.sort! { |a, b|
      # p a, b
      # Compare Major Version
      if a[:major] < b[:major]
        _smaller
      elsif  a[:major] > b[:major]
        _bigger
      else
        # Compare Minor Version
        if a[:minor] < b[:minor]
          _smaller
        elsif a[:minor] > b[:minor]
          _bigger
        else
          # Compare Patch Version
          if a[:patch] < b[:patch]
            _smaller
          elsif a[:patch] > b[:patch]
            _bigger
          else
            _same
          end
        end
      end
    }
    version = _versions.pop
  end

  version
end

task :bump, [:target] do |t, args|
  tag_base = 'ctct_docker'
  out = `git tag`

  version = tags out
  # puts "Old Version - #{version}"
  target = args[:target]
  if target == nil or target == '--patch'
    version[:patch] += 1
  elsif target == '--minor'
    version[:minor] += 1
    version[:patch] = 0
  elsif target == '--major'
    version[:major] += 1
    version[:minor] = 0
    version[:patch] = 0
  end

  # puts "New Version - #{version}"

  # Generate the new tag
  new_tag = "#{tag_base}-#{version[:major]}.#{version[:minor]}.#{version[:patch]}"
  puts "New Tag - #{new_tag}"
  `git tag #{new_tag}`
  `git push --tags`
end

task :version do
  out = `git tag`
  version = tags out
  puts "#{version[:major]}.#{version[:minor]}.#{version[:patch]}"
end

include Artifactory::Resource
task :upload, [:artifact, :user, :pass, :build_name, :build_number] do |t, args|

  out = `git tag`

  version = tags out
  version = "#{version[:major]}.#{version[:minor]}.#{version[:patch]}"
  endpoint =  'https://artifactory.roving.com/artifactory'
  file = args[:artifact]
  repo_key = 'libs-release-local'
  remote_path = "/com/constantcontact/wmiller/ctct_docker/ctct_docker-#{version}.knob"

  puts "Uploading #{file} to #{endpoint}/#{repo_key}#{remote_path}"
  # p args

  Artifactory.configure do |config|
    # The endpoint for the Artifactory server. If you are running the "default"
    # Artifactory installation using tomcat, don't forget to include the
    # +/artifactoy+ part of the URL.
    config.endpoint = endpoint

    # The basic authentication information. Since this uses HTTP Basic Auth, it
    # is highly recommended that you run Artifactory over SSL.
    config.username = args[:user]
    config.password = args[:pass]

    # Speaking of SSL, you can specify the path to a pem file with your custom
    # certificates and the gem will wire it all up for you (NOTE: it must be a
    # valid PEM file).
    # config.ssl_pem_file = '/path/to/my.pem'

    # Or if you are feeling frisky, you can always disable SSL verification
    config.ssl_verify = false

    # You can specify any proxy information, including any authentication
    # information in the URL.
    # config.proxy_username = 'user'
    # config.proxy_password = 'password'
    # config.proxy_address  = 'my.proxy.server'
    # config.proxy_port     = '8080'
  end

  properties = {
      'build.name' => args[:build_name],
      'build.number' => args[:build_number],
      'build.version' => version
  }
  artifact = Artifact.new(local_path: "./#{file}")
  # Upload an artifact to a repository whose key is 'repo_key'
  res = artifact.upload repo_key, remote_path, properties
  p res
end

task default: [:bump]
