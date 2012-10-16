#!/usr/local/bin/ruby

require "rubygems"
require "mechanize"
require "yaml"

MOB = YAML.load_file("#{File.dirname(__FILE__)}/config.yml") unless defined? MOB

agent = Mechanize.new
agent.user_agent_alias = 'Mac Safari'
agent.verify_mode = OpenSSL::SSL::VERIFY_NONE

MOB[:accounts].each do |name,acc|
  op = MOB[:operator][acc[:operator].to_sym]

  page = agent.get(op[:login_url])
  login_form = page.forms[op[:form_id]]
  login_form[op[:field_login]] = acc[:login]
  login_form[op[:field_password]] = acc[:password]
  login_form[op[:field_custom]] = acc[:custom] if acc[:custom]
  page = agent.submit login_form

  page = agent.get(op[:custom_url]) if op[:custom_url]
  
  page.body.each do |html|
    if html =~ /#{op[:balans_regexp]}/
      puts "\e[33m#{name}\e[0m: #{acc[:login]} #{$1} руб."
    end
  end
end


