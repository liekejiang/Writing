# Ruby Learnging--Day 1

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
