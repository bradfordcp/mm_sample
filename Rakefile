desc 'Syncs the contents of our build/ directory out to S3'
task :sync do
  puts "####################################"
  puts "# Fingerprinting and Pushing Files #"
  puts "####################################"
  puts "Files: "
  require 'digest/md5'
  require 'aws-sdk'
  require 'mime-types'

  AWS.config({
    :s3_endpoint => "s3-us-west-2.amazonaws.com",
    :access_key_id => "SOME_ACCESS_KEY",
    :secret_access_key => "SOME_SECRET"
  })

  s3 = AWS::S3.new :s3_endpoint => "s3-us-west-2.amazonaws.com"
  bucket = s3.buckets["example.com"]

  Dir.glob "build/**/*" do |path|
    unless File.directory? path
      # Get the file's fingerprint
      fingerprint = Digest::MD5.file(path).to_s

      # Our paths on  S3 shouldn't start with build/
      s3_path = path[6..-1]

      # Ref to an object that may or may not exist. No requests are made at this point.
      object = bucket.objects[s3_path]

      # Flag used to determine if this file gets pushed up
      push_file = false

      if object.exists?
        # Push the file if the fingerprint doesn't match
        push_file = object.metadata["fingerprint"] != fingerprint
      else
        # Push the file if it doesn't exist
        push_file = true
      end

      if push_file
        # Push the object to S3
        object.write File.new(File.join(path)), :content_type => MIME::Types.type_for(File.basename(s3_path)).first, :metadata => {"fingerprint" => fingerprint}, :acl => :public_read
        puts "  - PUSH: #{s3_path}"
      else
        puts "  - SAME: #{s3_path}"
      end
    end
  end
end
