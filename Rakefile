# -*- encoding: utf-8 -*-
#
# Copyright (c) 2011 Sung Pae <self@sungpae.com>
# Distributed under the MIT license.
# http://www.opensource.org/licenses/mit-license.php

require 'shellwords'

task :default => :configure

desc 'Configure tmux'
task :configure do
  sh '/bin/sh autogen.sh' unless File.executable? 'configure'

  cmd = %W[
    ./configure
    --prefix=#{ENV['PREFIX'] || '/opt/tmux'}
  ]

  cmd << '--enable-debug' if ENV['DEBUG']

  sh *cmd
end

desc 'Create / update custom terminfo files'
task :terminfo do
  join = lambda { |es| es.map { |e| "\t#{e},\n" }.join }
  mkdir_p 'terminfo', :verbose => false

  Dir.chdir 'terminfo' do
    Dir['/usr/share/terminfo/**/screen*'].each do |file|
      # Write original terminfo to file
      name = File.basename file
      out = %x(infocmp #{name.shellescape})
      buf = out.lines.take(2).join
      buf.concat out.lines.drop(2).join.gsub /, /, ",\n\t"
      File.open(name, 'w') { |f| f.write buf }

      # Italics support (from FAQ)
      buf.sub! /^screen/, 'tmux'
      buf.sub! /\|screen/, '|tmux'
      buf.sub! /%\?%p1%t;3%/, '%?%p1%t;7%'
      buf.sub! /smso=[^,]*,/, 'smso=\\E[7m,'
      buf.sub! /rmso=[^,]*,/, 'rmso=\\E[27m,'
      buf.concat join.call(%w(sitm=\\E[3m ritm=\\E[23m))

      # Create tmux terminfo
      File.open(name.sub(/\Ascreen/, 'tmux'), 'w') { |f| f.write buf }
    end
  end
end
