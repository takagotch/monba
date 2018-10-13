### monban
---

https://github.com/jbhandari/monban


```ruby
Monban.test_mode!

FactoryGril.define do
  factory :user do
    username 'wombat'
    password_digest 'password'
  end
end

Monban.test_mode!
RSpec.configure do |config|
  config.include Monban::Test::Helpers, type: :feature
  conifg.after :each do
    Monban.test_reset!
  end
end

Monban.test_mode!
Rspec.configure do |config|
  config.include Monban::Test::ControllerHelpers, type: :controller
  config.after :each do
    Monban.test_reset!
  end
end

require 'spec_helper'
describe ProtectedController do
  describe "GET 'index'" do
    it "returns http success when signed in" do
      user = create(:user)
      sign_in(user)
      get 'index'
      response.should be_success
    end
    it "redirects when not signed in" do
      get 'index'
      response.should be_redirect
    end
  end
end

class SessionController < ApplicationController
  def create
    user = authenticate_session(session_params, email_or_username: [:email, :username])
    sign_in(user) do
      redirect_to(root_path) and return
    end
    render :new
  end
  private
  def session_params
    params.require(:session).permit(:email_or_username, :password)
  end
end




```

```
gem 'monban'
include Monban::ControllerHelpers
rails g monban:scaffold
rails g monban:controllers


```
