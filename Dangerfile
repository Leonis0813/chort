#! /usr/local/rvm/rubies/ruby-2.5.5/bin/ruby

BASE_URL = "#{ENV['JENKINS_URL']}job/#{ENV['JOB_NAME']}/lastSuccessfulBuild/artifact/_build/html"

(git.added_files + git.modified_files).each do |file|
  if file.end_with?('.rst')
    rst_file = file
    html_file = File.join(BASE_URL, file.sub(/rst$/, 'html'))
    message("[#{rst_file}](#{html_file})")
  end
end
