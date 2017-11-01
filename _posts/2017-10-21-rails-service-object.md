---
layout: post
title: (번역) 레일즈 서비스 
comments: true
tags:
- Ruby on Rails
---

이 글은 Rob Race의 [Service Objects in Ruby on Rails…and you](https://hackernoon.com/service-objects-in-ruby-on-rails-and-you-79ca8a1c946e)를 번역한 글입니다.

## Let's begin!

Rails는 기본적으로 서비스 폴더를 만들어 주지 않기 때문에 우리가 직접 만들어야 하는 번거로움이 있습니다. 물론 만들고자 하는 앱의 크기나 복잡한 비지니스 로직으로 인해 서비스 객체는 다양해 질 수 있습니다. 이 게시물의 목적을 위해 app 폴더에 services 폴더를 만들겠습니다. `mkdir app/services`

이제 우리는 사용자 등록 서비스를 위한 `new_registration_service.rb`을 생성하고 비즈니스 로직을 써내려 갈 수 있습니다.

```ruby
class NewRegistrationService
  def initialize(params)
    @user = params[:user]
    @organization = params[:organization]
  end

  def perform
    organization_create
    send_welcome_email
    notify_slack
  end
  
  private

  def organization_create
      post_organization_setup if @organization.save
  end

  def post_organization_setup
      @user.organization_id = @organization.id
      @user.save
      @user.add_role :admin, @organization
  end

  def send_welcome_email
      WelcomeEmailMailer.welcome_email(@user).deliver_later
  end

  def notify_slack
      notifier = Slack::Notifier.new "https://hooks.slack.com/services/89ypfhuiwquhfwfwef908wefoij"
      notifier.ping "A New User has appeared! #{@organization.name} -   #{@user.name} || ENV: #{Rails.env}"
  end
end
```

이제 천천히 서비스 객체에 대해 살펴보겠습니다.

```ruby
class NewRegistrationService
  def initialize(params)
    @user = params[:user]
    @organization = params[:organization]
  end
end
```

여기에서는 클래스를 생성 한 다음 initialize 함수를 추가했습니다. `user` 각체와 `organization` 객체를 받아 각각의 변수에 저장합니다.

```ruby
def perform
  organization_create
  send_welcome_email
  notify_slack
end
```

개인적으로, 전체 비즈니스 로직을 하나의 함수인 `perform`에 묶어두는 것을 좋아합니다.  

```ruby
def organization_create
  post_organization_setup if @organization.save
end

def post_organization_setup
  @user.organization_id = @organization.id
  @user.save
  @user.add_role :admin, @organization
end
```

```ruby
def send_welcome_email
  WelcomeEmailMailer.welcome_email(@user).deliver_later
end
```

`user` 객체를 ActionMailer로 호출하기 위한 빠른 방법은...

```ruby
def notify_slack
  notifier = Slack::Notifier.new("https://hooks.slack.com/services/89ypfhuiwquhfwfwef908wefoij") 
  
  notifier.ping("A New User has appeared! #{@organization.name} -   #{@user.name} || ENV: #{Rails.env}")
end
```

마지막으로, Slack Webhook을 이용하여 알림을 보냅니다.

이제 여기서 하나 의문점이 생깁니다. "어떻게 그리고 어디에서 서비스 객체를 호출하나요?"
다음은 사인업 폼을 받은 Devise Registration 컨트롤러에서 추출한 코드입니다.

```ruby
NewRegistrationService.new({
  user: resource, 
  organization: @org
}).perform
```

이전에 비지니스 로직을 `perform` 함수에 묶어두는 것을 선호 한다고 했는데 그 이유 중에 하나는 아래와 같이 에러를 처리 할 수 있기 때문입니다.

```ruby
class NewRegistrationService
  def initialize(params)
    @user = params[:user]
    @organization = params[:organization]
  end

  def organization_create
    begin
      post_organization_setup if @organization.save
    rescue
      false
    end
  end
end
```

그리고 컨트롤러는 아래와 같습니다.

```ruby
if NewRegistrationService.new({user: resource, organization: @org}).organization_create
  # success logic 
else
  redirect_to(last_path, notice: 'Error saving record') 
end
```

서비스 객체의 구조와 상관없이 컨트롤러의 비지니스 로직을 서비스 객체로 옮겨 온다면 스키니한 컨트롤러와 간견할 코드의 앱을 구현할 수 있습니다.
