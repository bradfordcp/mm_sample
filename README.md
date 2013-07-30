Assumptions
============
1. Ruby 1.9.3 p327 is installed
2. RVM (for OS X)
3. bundler gem installed globally

Spin-Up Instructions
====================
Want to develop / run the static site locally? Follow along.
```bash
mkdir -p ~/src
cd ~/src
git clone git@github.com:bradfordcp/mm_sample.git
cd mm_sample
# Accept RVM warning
bundle install
bundle exec middleman server
```

Building for Production
=======================
The following commands build the site and push everything out to S3.
```bash
bundle exec middleman build
bundle exec rake sync
```
_Yeah it's that easy_

Documentation
=============
http://middlemanapp.com/getting-started/
