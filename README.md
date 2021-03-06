Devise - PAM Authentication
===========================

devise\_pam\_authenticatable is a Devise (http://github.com/plataformatec/devise)
extension for authenticating using PAM (Pluggable Authentication Modulues)
via the rpam2 gem.

This allows you to authenticate against the local hosts authentication
system including local account usernames and passwords.

Or use LDAP and other PAM modules for LDAP authentication.

There are obvious security risks with using PAM authentication via a
web-based application. Make sure you at least use SSL to keep usernames and
passwords encrypted via HTTPS.

Installation
------------

In the Gemfile for your application:

    gem "devise_pam_authenticatable2"

Or, to use the latest from github:

    gem "devise_pam_authenticatable2", :git => "git://github.com/devkral/devise_pam_authenticatable2.git"

Important changes
-----------------

Versions before 4.0.0 are limitted compatible with database_authenticatable.
Some removed code may can clash in earlier versions.

Version 5-8 change method names. Check if everything is correct. Sorry for the quick release cycle.

In version 9 is_pam_account? is changed to pam_managed_user? because some linters hate the former name.

In version 9.2 pam_authentication must support a second argument (request). Only relevant if overwritten. Adds feature: pam gets now the remote ip address of the client as RHOST

Setup
-----

The devise_pam_authenticatable extension can use a username or extract the name from a special email address (suffix can be choosen)
username field and email field are configurable

In your Devise model, ensure the following is present:

    class User < ActiveRecord::Base

      devise :pam_authenticatable, pam_service: 'system-auth', pam_suffix: 'pamlogin'

      # in case there is no password set by other devise modules:
      attr_accessor :password
      @password = nil

      # in case other devise modules complain about missing password
      # and the password is not mirrored
      def password_required?
        false
      end

    end

pam_service: 'system-auth' is optional. By default the pam service specified in config.pam_default_service is used.

pam_suffix: 'pamlogin' is optional. By default the pam email extraction suffix specified in config.pam_default_suffix is used.

Options:

* config.pam_default_service = "rpam"
* config.pam_default_suffix = nil # extraction disabled by default
* config.pam_default_suffix = "pam" # username@pam = username
* config.emailfield = "email" # set emailfield, set to nil if not available
* config.usernamefield = "username" # set to nil to disable username (only email extraction)
* config.check_at_sign = false # detect if email field contains username by @ sign (make sure names cannot contain @ signs)

References
----------

* [Devise](http://github.com/plataformatec/devise)
* [Warden](http://github.com/hassox/warden)


Released under the MIT license
