### Day1 for Rails

## Rails 是什么？
Rails 是使用 Ruby 语言编写的 Web 应用开发框架，目的是通过解决快速开发中的共通问题，简化 Web 应用的开发。与其他编程语言和框架相比，使用 Rails 只需编写更少代码就能实现更多功能。

Rails 哲学包含两大指导思想：
* 不要自我重复（DRY）： DRY 是软件开发中的一个原则，意思是“系统中的每个功能都要具有单一、准确、可信的实现。”。不重复表述同一件事，写出的代码才更易维护、更具扩展性，也更不容易出问题。
* 多约定，少配置： Rails 为 Web 应用的大多数需求都提供了最好的解决方法，并且默认使用这些约定，而不是在长长的配置文件中设置每个细节。

## MVC控制模式
在Rails里面我们主要使用MVC：Model-View-Controller的控制模式：
* Model：模型负责数据的存贮
* View： 视图负责页面的显示与交互，是工作中的大头
* Controller： 负责数据的获取，处理，包括页面交互背后的逻辑

在Rails里面，这三个模块都在root/app/文件里面。

## Routes 路由
占坑，不懂

## 建立一个欢迎页"Hello Rails!"需要什么操作 ？
只是编写一个显示页面，我们需要创建控制器和视图。控制器接受向应用发起的特定访问请求，视图负责显示，而背后的路由负责决定哪些访问请求被哪些控制器接受。 一个控制器会对应多个路由，不同路由对应不同动作，不同动作负责搜集数据并把数据提供给视图。

首先需要创建一个控制器来负责显示欢迎页“Hello Rails！”, 这个名为Welcome的控制器包含一个叫**index**的动作。
```ruby
rails generate controller Welcome index
```
同时这行命令会产生一个路由, 告诉Rails这行命令的前缀是welcome_index，路由动作是GET，对应的URI资源是/welcome/index(.:format),而对应的控制器是welcome，对应的控制器方法是**index**。 
路由文件位于root/config/routes.rb。
```
Prefix        Verb   URI Pattern                 Controller#Action
welcome_index GET    /welcome/index(.:format)    welcome#index
```
生成的控制器文件位于root/app/controllers/welcome_controller.rb，视图文件位于root/app/views/welcome/index.html.erb。我注意到之前Welcome的大写会变成小写，rails似乎会“自作主张”地对命名作出一些自己的推断和更改。这里控制器文件.rb负责定义动作，必须定义有名为**index**的方法，即使是空的；而视图文件负责编辑显示页面，使用HTML或者haml都可以。

现在我们需要告知Rails何时需要显示“Hello Rails！”，假设我们需要在网站的主页即根目录显示，那么我们需要更改路由routes.rb来告诉Rails现在的主页在哪里。
```ruby
Rails.application.routes.draw do
  get 'welcome/index'
  root 'welcome/index'
  # For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
end
```
get那一行是创建控制器时自动创建的，而root这一行是新加入的，表示这是网站的根目录。保存运行之后路由里会多出一行，这行命令代表的含义是对根路径的访问请求应该发往welcome控制器的index动作。而之前get那一行的命令意思是对/welcome/index的访问应该发往welcome控制器的index动作：
```
Prefix        Verb   URI Pattern          Controller#Action
root          GET    /                    welcome#index
```

综上所述我们需要
1. 创建一个相应的控制器，会生成控制器文件和视图文件
2. 控制器文件创建**空的**index动作方法
3. 视图文件编写显示“Hello Rails!”
4. 修改路由如果对显示有特殊要求
 
##  建立一个表单需要什么操作？

###从对象（资源）的建立开始
这次我们要创建一个资源（resource）。资源是Rails中的一个术语，表示一系列类似对象的集合，这个集合一般称为CRUD，我们在路由文件中声明一个名为：articles的资源（使用ruby的符号）,Rails将会自动为我们创建多个路由命令：
```ruby
Rails.application.routes.draw do
  get 'welcome/index'
  resources :articles
  root 'welcome#index'
  
$ rake routes
      Prefix Verb   URI Pattern                  Controller#Action
    articles GET    /articles(.:format)          articles#index
             POST   /articles(.:format)          articles#create
 new_article GET    /articles/new(.:format)      articles#new
edit_article GET    /articles/:id/edit(.:format) articles#edit
     article GET    /articles/:id(.:format)      articles#show
             PATCH  /articles/:id(.:format)      articles#update
             PUT    /articles/:id(.:format)      articles#update
             DELETE /articles/:id(.:format)      articles#destroy
        root GET    /      
```
### 表单需要页面
任何应用都需要一个页面去显示和交互，我们选择articles/new作为表单提交的页面。有了页面，联想上一节更改欢迎页面，我们还需要控制器：
```ruby
rails generate controller Articles
```
我们需要在这个新建控制器里面定义控制器的动作，也就是CRUD。既然我们已经选定了articles/new作为显示页面，那么我们就需要在控制器里面定义**new**动作。有了**new**方法，我们还需要对应的视图，由于我们在创建控制器的时候没有使用**new**这个参数，我们需要自己手动建立app/views/articles/new.html.erb。
创建表单可以使用表单构建器**form_for**，具体暂不分析。表单使用了：article作为参数，告诉这个代码块应该处理哪个对象。这个表单还应该指向其他URL，因为articles/new只是用于显示表单,因此我们使用url这个辅助参数，articles_path会告诉Rails把表单指向和articles前缀相关联的URI模式。默认情况下，表单会发起**POST**请求，结合前面列出的路由表，POST和这个URL会与当前控制器的create动作相关联。接下来我们就需要创建**create**方法。
```html
<%= form_for :article, url: articles_path  do |f| %>
  <p>
    <%= f.label :title %><br>
    <%= f.text_field :title %>
  </p>
 
  <p>
    <%= f.label :text %><br>
    <%= f.text_area :text %>
  </p>
 
  <p>
    <%= f.submit %>
  </p>
<% end %>
```



