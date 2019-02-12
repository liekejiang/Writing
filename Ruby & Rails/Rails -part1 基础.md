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

## 创建模型与迁移
```ruby
class ArticlesController < ApplicationController
  def new
  end
  def create
  end
end
```
因为new只是负责显示表单，实际的创建动作需要**create**来完成，因此我们不能只是简单地定义一个空方法。**create**方法会从表单中获取一些数据，我们首先需要保存这些传过来的这些数据。在MVC模型中，Model主要负责与逻辑层和持久层交互，也是3种元素中唯一与持久层交互的，因此遇到和数据库相关的内容就需要用到**Model**。我们首先需要新建一个**Model**：
```ruby
rails genreate model Article title:string text:string
```
这里的2个值title和text是表单form_for方法中定义的两个变量，同时也使模型具有字符串类型的title和文本属性的text属性。这两个属性会自动添加到数据库的**articles**表中，并映射到Article模型中。这行命令会创建不少文件，我们着重关注root/app/models/article.rb和root/db/migrate/YYYYMMDDHHMMSS_create_articles.rb，后者负责创建数据库。
上面提到的YYYYMMDDHHMMSS_create_articles.rb文件是数据库迁移文件，迁移是用于简化创建和修改数据库操作的Ruby类。Rails使用rake命令运行迁移，并且在迁移作用于数据库之后还可以撤销迁移操作。在该文件里会看到这些代码：
```ruby
class CreateArticles < ActiveRecord::Migration[5.0]
  def change
    create_table :articles do |t|
      t.string :title
      t.text :text
      t.timestamps
    end
  end
end
```
上面的迁移创建了change方法，在迁移时会调用这个方法，这些操作都是可逆的。之后进行迁移,会创建Articles表:
```ruby
bin/rails db:migrate
```
然后修改ArticlesController中的create方法，使用新建的Article模型把数据保存到数据库：
```ruby
def create
  @article = Article.new(params[:article])
 
  @article.save
  redirect_to @article
end
```
代码的第一行把Rails 模型用相应的属性初始化，它们会自动映射到对应的数据库字段。接下来 @article.save 负责把模型保存到数据库。最后把页面重定向到 show 动作，这个 show 动作我们稍后再定义。在这里我们一定要区别明白：这里修改的都是Article的控制器，而修改用的代码中大写的Article是Model中的方法。此外之所以这里说是show动作，在之前的路由中，article作为前缀的命令对应的方法就是article#show。
最后我们出于安全性的考虑，也是强制要求，需要为控制器参数设置白名单以避免错误地批量赋值。这里，我们想在 create 动作中合法使用 title 和 text 参数，为此需要使用 require 和 permit 方法。像下面这样修改 create 动作中的一行代码
```ruby
@article = Article.new(params.require(:article).permit(:title, :text))
```
上述代码通常被抽象为控制器类的一个方法，以便在控制器的多个动作中重用，例如在 create 和 update 动作中都会用到。除了批量赋值问题，为了禁止从外部调用这个方法，通常还要把它设置为 private。最后的代码像下面这样：
```ruby
private
  def article_params
    params.require(:article).permit(:title, :text)
  end
```
## 显示文章
现在在Article的Controller模型里添加与show有关的方法(与路由对象有关的方法都在Controller中定义)，我们先康康show对应的路由
```ruby
article GET    /articles/:id(.:format)      articles#show
```
这其中的id 告诉 Rails 这个路由期望接受 :id 参数，在这里也就是文章的 ID。

TIPS：常见的做法是按照以下顺序在控制器中放置标准的 CRUD 动作：index，show，new，edit，create，update 和 destroy。你也可以按照自己的顺序放置这些动作，但要记住它们都是公开方法，如前文所述，必须放在私有方法之前才能正常工作。

show动作的定义如下：
```ruby
class ArticlesController < ApplicationController
  def show
    @article = Article.find(params[:id])
  end
```
我们使用Article.find来查找文章，并传入params[:id]以便从中获得：id参数。我们还使用实例变量（前缀为@）保存对文章对象的引用。这样做是因为Rails会把所有实例变量传递给视图。



