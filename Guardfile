# A sample Guardfile
# More info at https://github.com/guard/guard#readme

## Uncomment and set this to only include directories you want to watch
# directories %w(app lib config test spec feature)

## Uncomment to clear the screen before every task
# clearing :on

## Make Guard exit when config is changed so it can be restarted
#
## Note: if you want Guard to automatically start up again, run guard in a
## shell loop, e.g.:
#
#  $ while bundle exec guard; do echo "Restarting Guard..."; done
#
## Note: if you are using the `directories` clause above and you are not
## watching the project directory ('.'), the you will want to move the Guardfile
## to a watched dir and symlink it back, e.g.
#
#  $ mkdir config
#  $ mv Guardfile config/
#  $ ln -s config/Guardfile .
#
# and, you'll have to watch "config/Guardfile" instead of "Guardfile"
#
watch 'Guardfile' do
  UI.info 'Exiting because Guard must be restarted for changes to take effect'
  exit 0
end


# shell commands
guard :shell do
  # test that the .gemspec still works
  watch %r{^.*\.gemspec$} do |m|
    cmd = "bundle exec gem build '#{m[0]}'"
    p "== #{cmd}"
    `#{cmd}`
  end
end


# Run rspec tests when things change
guard :rspec, cmd: 'bundle exec rspec ' do
  watch(%r{^spec/lib/.+_spec\.rb$})

  # when a file changes run the tests for that file
  watch(%r{^lib/(.+)\.rb$}) do |m|
    File.join( 'spec', 'lib', "#{m[1]}_spec.rb" )
  end

  watch('spec/spec_helper.rb')  { "spec" }
end


# tmux 1.7+ can send rspec results to the TMUX status pane.
notification( :tmux, {
  display_message: true,
  timeout: 5, # in seconds
  default_message_format: '%s >> %s',
  # the first %s will show the title, the second the message
  # Alternately you can also configure *success_message_format*,
  # *pending_message_format*, *failed_message_format*
  line_separator: ' > ', # since we are single line we need a separator
  color_location: 'status-left-bg', # to customize which tmux element will change color

  # Other options:
  default_message_color: 'black',
  success: 'colour150',
  failure: 'colour174',
  pending: 'colour179',
  # Notify on all tmux clients

  display_on_all_clients: false
}) if ( ENV.fetch( 'TMUX', false ) && (%x{tmux -V}.split(' ').last.split('.').last.to_i > 6 ))
# vim:set syntax=ruby:

