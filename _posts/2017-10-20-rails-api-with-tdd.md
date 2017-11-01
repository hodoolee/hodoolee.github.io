---
layout: post
title: 레일즈 API와 TDD
comments: true
tags:
- Ruby on Rails
---

## Prerequisites

- Ruby >= 2.2.2
- Rails >= 5.0.0

## API Endpoints

| Endpoint | Functionality |
| ----- | ---- |
| GET /recipes | 모든 레시피를 가져온다 |
| POST /recipes | 레시피를 생성한다 |
| GET /recipes/:id | 해당 레시피의 정보를 가져온다 |
| PUT /recipes/:id | 해당 레시피를 갱신한다 |
| DELETE /recipes/:id | 해당 레시피를 삭제한다 |

## Setup

API 레일스 애플리케이션을 생성하려면 `--api` 옵션을 사용하여 시작 할 수 있다. 기본적로 프로젝트를 생성하면 테스트 프레임워크인 Minitest가 함께 생성된다. 개인적으로 테스트는 RSpec을 더 선호하기 때문에 `-T` 옵션을 사용하여 Minitest 대신 Rspec을 사용할 것이다.

```
$ rails new recipes-api --api -T
```

Test-drivien development(TDD)를 위해 몇가지 젬을 추가해보자.

```ruby
# Gemfile

...

group :development, :test do
  gem 'rspec-rails'
end

group :test do
  gem 'database_cleaner'
  gem 'shoulda-matchers'
  gem 'faker'
  gem 'factory_girl_rails'
end
```

이제 번들러를 사용하여 젬을 설치해보자.

```
$ bundle install
$ rails g rspec:install
  create  .rspec
  exist  spec
  create  spec/spec_helper.rb
  create  spec/rails_helper.rb
$ mkdir spec/factories
```

## Routes

```ruby
# config/routes.rb
Rails.application.routes.draw do
  resources :recipes
end
```

## Models

```
# 모델은 단수형이다.
$ rails g model Recipe name:string writer:string

# default가 string type이기 때문에 아래와 같이 실행해도 상관없다.
$ rails g model Recipe name writer
```

그럼 다음과 같은 마이그레이션 파일이 생성된다.

```ruby
# db/migrate/[timestamp]_create_recipes.rb
class CreateRecipes < ActiveRecord::Migration[5.1]
  def change
    create_table :Recipes do |t|
      t.string :name
      t.string :writer

      t.timestamps
    end
  end
end
```

마이그레이션을 실행하기 위해선 아래의 명령어를 실행한다. 

```
$ rails db:migrate
```

레시피 모델을 테스트 하기 위한 spec 파일은 아래와 같다.

```ruby
# spec/models/recipe_spec.rb
require 'rails_helper'

# 레시피 모델의 컬럼 존재 여부를 테스트 한다.
RSpec.describe Recipe, type: :model do
  it { should validate_presence_of(:name) }
  it { should validate_presence_of(:writer) }
end
```

Recipe 모델을 작성 한 후 테스트를 실행해보자.

```ruby
# app/models/recipe.rb
class Recipe < ApplicationRecord
  validates_presence_of :name, :writer
end
```

## Controllers

```
# 컨트롤러는 복수형이다.
$ rails g controller Recipes

# 테스트를 위한 파일을 생성한다.
$ mkdir spec/requests && touch spec/requests/recipes_spec.rb
$ touch spec/factories/recipes.rb
```

```ruby
# spec/factories/recipes.rb
FactoryGirl.define do
  factory :recipe do
    name { Faker::Lorem.word }
    writer { Faker::Number.number(10) }
  end
end
```

factory가 호출 될 떄 마다 faker는 같은 데이터가 아닌 다른 데이터가 생성 되도록 도와준다.

```ruby
# spec/requests/recipes_spec.rb
require 'rails_helper'

RSpec.describe 'Recipes API', type: :request do
  let!(:recipes) { create_list(:recipe, 10) }
  let(:recipe_id) { recipes.first.id }

  # Test GET /recipes
  describe 'GET /recipes' do
    before { get '/recipes' }

    it 'returns recipes' do
      # json은 JSON 응답을 parse 해주는 헬퍼 함수이다
      expect(json).not_to be_empty
      expect(json.size).to eq(10)
    end

    it 'returns a status code 200' do
      expect(response).to have_http_status(200)
    end
  end

  # Test GET /recipes/:id
  describe 'GET /recipes/:id' do
    before { get "/recipes/#{recipe_id}" }

    context 'when the record exists' do
      it 'returns the recipe record' do
        expect(json).not_to be_empty
        expect(json['id']).to eq(recipe_id)
      end

      it 'returns a status code 200' do
        expect(response).to have_http_status(200)
      end
    end

    context 'when the record does not exist' do
      let(:recipe_id) { 100 }

      it 'returns a status code 404' do
        expect(response).to have_http_status(404)
      end

      it 'returns a not found message' do
        expect(response.body).to match(/Couldn't find Recipe/)
      end
    end
  end

  # Test POST /recipes
  describe 'POST /recipes' do
    let(:valid_attributes) { { name: 'Afoogato', writer: '1' } }

    context 'when the request is valid' do
      before { post '/recipes', params: valid_attributes }

      it 'creates a recipe' do
        expect(json['name']).to eq('Afoogato')
      end

      it 'returns a status code 201' do
        expect(response).to have_http_status(201)
      end
    end

    context 'when the request is invalid' do
      before { post '/recipes', params: { name: 'Affogato' } }

      it 'returns a status code 422' do
        expect(response).to have_http_status(422)
      end

      it 'returns a failure message' do
        expect(response.body)
          .to match(/writer can't be blank/)
      end
    end
  end

  # Test PUT /recipes/:id
  describe 'PUT /recipes/:id' do
    let(:valid_attributes) { { name: 'Apple Pie' } }

    context 'when the record exists' do
      before { put "/recipes/#{recipe_id}", params: valid_attributes }

      it 'updates the record' do
        expect(response.body).to be_empty
      end

      it 'returns a status code 204' do
        expect(response).to have_http_status(204)
      end
    end
  end

  # Test DELETE /recipes/:id
  describe 'DELETE /recipes/:id' do
    before { delete "/recipes/#{recipe_id}" }

    it 'returns a status code 204' do
      expect(response).to have_http_status(204)
    end
  end
end
```

위에서 언급한 헬퍼 함수 json은 아래와 같이 작성한다.

```
$ mkdir spec/support && touch spec/support/request_spec_helper.rb
```

```ruby
# spec/support/request_spec_helper
module RequestSpecHelper
  # Parse JSON response to ruby hash
  def json
    JSON.parse(response.body)
  end
end

# spec/rails_helper.rb
# [...]
Dir[Rails.root.join('spec/support/**/*.rb')].each { |f| require f }
# [...]
RSpec.configuration do |config|
  # [...]
  config.include RequestSpecHelper, type: :request
  # [...]
end
```

완성된 컨트롤러는 아래와 같다.

```ruby
# app/controllers/recipes_controller.rb
class RecipesController < ApplicationController
  include Response
  include ExceptionHandler

  before_action :set_recipe, only: [:show, :update, :destroy]

  # GET /recipes
  def index
    @recipes = Recipe.all
    json_response(@recipes)
  end

  # POST /recipes
  def create
    @recipe = Recipe.create!(recipe_params)
    json_response(@recipe, :created)
  end

  # GET /recipes/:id
  def show
    json_response(@recipe)
  end

  # PUT /recipes/:id
  def update
    @recipe.update(recipe_params)
    head :no_content
  end

  # DELETE /recipes/:id
  def destroy
    @recipe.destroy
    head :no_content
  end

  private
  
    def recipe_params
      params.permit(:name, :created_by)
    end

    def set_recipe
      @recipe = recipe.find(params[:id])
    end
end

# app/controllers/concerns/exception_handler.rb
module ExceptionHandler
  extend ActiveSupport::Concern

  included do
    rescue_from ActiveRecord::RecordNotFound do |e|
      json_response({ message: e.message }, :not_found)
    end

    rescue_from ActiveRecord::RecordInvalid do |e|
      json_response({ message: e.message }, :unprocessable_entity)
    end
  end
end
```

```ruby
# app/controllers/concerns/response.rb
module Response
  def json_response(object, status = :ok)
    render json: object, status: status
  end
end
```

json_response 함수는 JSON과 HTTP 상태 코드로 응답할 수 있게 도와준다.  
마지막으로 테스트가 모두 패스하는지 확인해보자!
