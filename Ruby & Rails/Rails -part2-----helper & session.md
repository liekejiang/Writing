## 至今不是很懂的Rails命名规则
在学习Rails的过程中，对于对象的单复数的命名规则让我很是头疼，在阅读blog和教程的时候，有许多人提到Rails会自动根据对象命名推导出单复数并加以使用，最典型的就是作业中声明了`resources: movies` 然后在路由中推导出了单数形式并且自动命名了一个`movie_path`的变量。这就对我们命名的严谨性提出了更高的要求。

先占坑，以后填


## 关于something_path
在Rails框架中，在**link_to**方法后面经常跟着一些名为**something_path**的变量名，如下所示：
```haml
-#haml
%table#movies
  %thead
    %tr
      %th{class: current_page?('/movies?title') && 'hilite' }=link_to 'Movie Title' , '/movies?title'
      %th Rating
      %th{class: current_page?('/movies?release') && 'hilite'}=link_to 'Release Date', '/movies?release'
      %th More Info
  %tbody
    - @movies.each do |movie|
      %tr
        %td= movie.title 
        %td= movie.rating
        %td= movie.release_date
        %td= link_to "More about #{movie.title}", movie_path(movie)
= link_to 'Add new movie', new_movie_path
```
在我一开始学习Rails时，为了弄清这个`movie_path`是什么，翻遍了整个工程文件夹都没找到，最后还是[stackoverflow](https://stackoverflow.com/questions/25820138/what-is-the-path-method-in-ruby-on-rails)回答了我这个问题。这个`movie_path`其实是Rails框架自己生成一个辅助方法，叫做`route helper`,下面看一个`rake routes`的执行输出：
```
    Prefix Verb   URI Pattern                Controller#Action
      root GET    /                          movies#index
    movies GET    /movies(.:format)          movies#index
           POST   /movies(.:format)          movies#create
 new_movie GET    /movies/new(.:format)      movies#new
edit_movie GET    /movies/:id/edit(.:format) movies#edit
     movie GET    /movies/:id(.:format)      movies#show
           PATCH  /movies/:id(.:format)      movies#update
           PUT    /movies/:id(.:format)      movies#update
           DELETE /movies/:id(.:format)      movies#destroy
```
因为我们已经在routes.rb里面使用`resource: movie`声明过了`movie`这个对象，因此rails会自动帮我们生成一共四个变量用于http请求：
* movies_path: 输出所有movie对象
* movie_path： 单个movie对象
* edit_movie_path： 更新一个movie对象
* new_movie_path： 创建一个movie对象
因此我们可以不用自己声明而直接使用这些指向页面的变量。

## request 请求对象



## Session
Session（会话）在rail中主要用于存贮（少量）数据，其只能在控制器和视图中使用。session有多种存储机制，在[教程](https://ruby-china.github.io/rails-guides/action_controller_overview.html#session)有比较详细的介绍。session的实现基于cookie，存储每个会话的ID（必须使用 cookie，因为 Rails 不允许在 URL 中传递会话 ID，这么做不安全）。
默认的存储机制为`CookieStore`，这种机制把所有会话数据都存储在 cookie 中（如果需要，还是可以访问 ID）。CookieStore 的优点是轻量，而且在新应用中使用会话也不用额外的设置。cookie 中存储的数据会使用密令签名，以防篡改。cookie 还会被加密，因此任何能访问 cookie 的人都无法读取其内容。（如果修改了 cookie，Rails 会拒绝使用。）

### 1.访问会话
在控制器中可以通过实例方法访问session，（会话是惰性加载的。如果在动作中不访问，不会自动加载。因此任何时候都无需禁用会话，不访问即可。），session中的数据以hash键值对的形式存储。
```ruby
class ApplicationController < ActionController::Base
  private
  # 使用会话中 :current_user_id  键存储的 ID 查找用户
  # Rails 应用经常这样处理用户登录
  # 登录后设定这个会话值，退出后删除这个会话值
  def current_user
    @_current_user ||= session[:current_user_id] &&
      User.find_by(id: session[:current_user_id])
  end
end
```
##### 1.1 ||=符号
在上面的代码中我们发现了`||=`这种符号，其实上面定义的`current_user`符号等价于下面的代码：
```ruby
def current_user
    #如果@current_user不为空则直接返回该值
    if @current_user
       return @current_user
    else
        #若@current_user为空，则根据:user_id判断是否登陆
        #如果已经登陆，则查找用户信息并返回
       if session[:user_id]
          @current_user = User.find(session[:user_id])
        #如果没有登陆，则返回nil
       else
          @current_user = nil
       end
       return @current_user
    end
end
```
虽然懂了代码的本身含义，但是`||=`是什么意思还是不懂。在Ruby中，`&&`和`||`的用法与其在其他语言中不太一致：
* &&运算法则为：左边为false或nil时，直接分别返回false或nil，右边将不会运算。左边不为false或nil时，返回右边的对象。
* ||运算法则为：左边为false或nil时，返回右边的对象。左边不为false或nil时，直接返回左边的对象，右边的不会运算。

再查看更多范例：
```ruby
puts false && "abc"      # => false
puts nil   && "abc"      # => nil

puts true  && "abc"      # => "abc"
puts "123" && "abc"      # => "abc"

puts false || "abc"      # => "abc"
puts nil   || "abc"      # => "abc"

puts true  || "abc"      # => true
puts "123" || "abc"      # => "123"
```
而这两种符号与等号`=`结合起来，就更复杂一点了：
```ruby
x ||= y
#相当于
x || x=y
#而不是
x = x||y
#区别在于如果x存在且不为空时不会执行任何操作，直接返回。
#还相当于
if defined? x
    x || x=y
else
    x = y
end
```
---
结合上面的session实例，我们分析可得`||=`右边的代码只会返回:user_id的信息或者nil。
```
session[:user_id] && User.find(session[:user_id])
```
而`||=`保证要不在@_current_user已经是用户信息的时候直接返回，要不返回使用`.find`找到的用户信息。

若想把数据存入会话，给键赋值即可：
```ruby
class LoginsController < ApplicationController
  # “创建”登录，即“登录用户”
  def create
    if user = User.authenticate(params[:username], params[:password])
      # 把用户的 ID 存储在会话中，以便后续请求使用
      session[:current_user_id] = user.id
      redirect_to root_url
    end
  end
end
```
从会话中删除数据，把键值设置为nil:
```ruby
class LoginsController < ApplicationController
  # “删除”登录，即“退出用户”
  def destroy
    # 从会话中删除用户的 ID
    @_current_user = session[:current_user_id] = nil
    redirect_to root_url
  end
end
```
若想重设整个会话，使用 reset_session 方法。

### 2.闪现消息
闪现消息是会话的一个特殊部分，每次请求都会清空。也就是说，其中存储的消息只能在下次请求中使用，一般用于传递错误消息。闪现消息的访问方式与会话差不多，类似于散列。（闪现消息是 FlashHash 实例。）下面以退出登录为例。控制器可以发送一个消息，在下次请求时显示：
```ruby
class LoginsController < ApplicationController
  def destroy
    session[:current_user_id] = nil
    flash[:notice] = "You have successfully logged out."
    redirect_to root_url
  end
end
```
重定向也可以设置闪现消息。可以指定 :notice、:alert 或者常规的 :flash：
```ruby
redirect_to root_url, notice: "You have successfully logged out."
redirect_to root_url, alert: "You're stuck here!"
redirect_to root_url, flash: { referral_code: 1234 }
```
需要注意的是，只有下一个动作才能处理前一个动作设置的闪现消息。一般会在应用的布局中加入显示警告或提醒消息的代码：
```html
<html>
  <!-- <head/> -->
  <body>
    <% flash.each do |name, msg| -%>
      <%= content_tag :div, msg, class: name %>
    <% end -%>
    <!-- more content -->
  </body>
</html>
```
### 3. Cookies
应用可以在客户端存储少量数据（称为 cookie），在多次请求中使用，甚至可以用作会话。在 Rails 中可以使用 cookies 方法轻易访问 cookie，用法和 session 差不多，就像一个散列：
