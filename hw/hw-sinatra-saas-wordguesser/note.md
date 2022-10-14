# 使用Sinatra搭建WordGuesser应用

## Part 0 简单应用示例

版本控制：`git`

依赖管理：`bundler`， bundler是一个gem，在app根目录中有一个名为`Gemfile`的文件，包含需要的gem列表以及版本。

创建`Gemfile`

```ruby
source 'https://rubygems.org'
ruby '2.6.6'

gem 'sinatra', '>= 2.0.1'
```

运行`bundle`命令，将会查看`Gemfile`文件，并且尝试安装，创建一个`Gemfile.lock`文件

### 使用Sinatra创建简单SaaS应用

SaaS应用需要

1）一个Web服务器来获取HTTP请求，我们使用`webrick`，一个基于Ruby的简易Web服务器

2）一个应用服务器、将应用逻辑链接至Web服务器，我们使用`rack`

创建文件`app.rb`

```ruby
require 'sinatra'

class MyApp < Sinatra::Base
  get '/' do
    "<!DOCTYPE html><html><head></head><body><h1>Hello World</h1></body></html>"
  end
end
```

`Rack`应用服务器需要文件`config.ru`控制，如下

```ruby
require './app'
run MyApp
```

启动应用

```bash
bundle exec rackup --port 3000
```

需要自动重启服务，使用`rerun` gem，将其添加至开发环境中

```ruby
group :development do 
  gem 'rerun'
end
```

启动服务命令变为`bundle exec rerun -- rackup --port 3000`

## 部署到Heroku

heroku查看`Procfile`来执行进程类型

```
web: bundle exec rackup config.ru -p $PORT
```



## WordGuesser

测试驱动开发（TDD），使用`autotest`

查看`.rspec`文件，其中包含选项、指明我们使用RSpce测试框架，

```ruby
  describe 'new', :pending => true do
    it "takes a parameter and returns a WordGuesserGame object" do      
      @game = WordGuesserGame.new('glorp')
      expect(@game).to be_an_instance_of(WordGuesserGame)
      expect(@game.word).to eq('glorp')
      expect(@game.guesses).to eq('')
      expect(@game.wrong_guesses).to eq('')
    end
  end
```



## RESTful API

| Route and action | Resource operation                        | Web result                             |
| ---------------- | ----------------------------------------- | -------------------------------------- |
| `GET /show`      | show game state                           | display correct & wrong guesses so far |
| `POST /guess`    | update game state with new guessed letter | redirect to `show`                     |
| `POST /create`   | create new game                           | redirect to `show`                     |

为了开始一个新游戏，我们需要一个提供给用户一个方式来触发`POST /create`

| `GET /new` | give human user a chance to start new game | display a form that includes a "start new game" button |
| ---------- | ------------------------------------------ | ------------------------------------------------------ |
|            |                                            |                                                        |

游戏结束后show需要重定向至结束界面

| Show game state, allow player to enter guess; may redirect to Win or Lose | GET /show    |
| ------------------------------------------------------------ | ------------ |
| Display form that can generate `POST /create`                | GET /new     |
| Start new game; redirects to Show Game after changing state  | POST /create |
| Process guess; redirects to Show Game after changing state   | POST /guess  |
| Show "you win" page with button to start new game            | GET /win     |
| Show "you lose" page with button to start new game           | GET /lose    |



 ## 连接到Sinatra

`before do...end` 在每一个Saas请求之前执行

`after do...end` 在每一个Saas请求之后执行

`erb :`会使Sinatra去查看文件`views/`中的`.erb`文件，然后运行其中的`<%= like this %>`,替换结果。



我们可以整个游戏状态放入cookie中，任何放入session[]中的东西，都可以跨请求保存。

如果想要想玩家显示一条错误消息，需要在下一次重定向中显示，这就需要“跨”重定向获得消息，我们使用`sinatra-flash` gem, 	`Flash[]`是一个用于记忆短消息的hash, 他会一直保存到下一个请求然后被擦除。


## Cucumber

将cucumber配置为使用Capybara，这是一种基于ruby的浏览器模拟器，模拟浏览器操作并检查影响。

