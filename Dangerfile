#! /usr/local/rvm/rubies/ruby-2.4.4/bin/ruby

(git.added_files + git.modified_files).each do |file|
  if file.end_with?('.rst')
    rst_file = file
    html_file = "#{ENV['JENKINS_URL']}job/#{ENV['JOB_NAME']}/lastSuccessfulBuild/artifact/_build/html/#{file.sub(/rst$/, 'html')}"
    message("[#{rst_file}](#{html_file})")
  end
end
