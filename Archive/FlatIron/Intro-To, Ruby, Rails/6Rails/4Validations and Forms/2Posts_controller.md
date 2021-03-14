```ruby
class Author < ActiveRecord::Base
  validates :name, presence: true
  validates :email, uniqueness: true
  
end
```

```ruby
class Post < ActiveRecord::Base
  validates :title, presence: true
  validates :category, inclusion: {in: %w(Fiction Non-Fiction)}
  validates :content, length: {minimum: 100}
end

```

```ruby
class AuthorsController < ApplicationController
  def show
    @author = Author.find(params[:id])
  end

  def new
  end

  def create
    @author = Author.new(author_params)

    if @author.valid?
      @author.save

    redirect_to author_path(@author)
    else 
      render :new
    end
  end

  private

  def author_params
    params.permit(:email, :name)
  end
end
```

```ruby
class PostsController < ApplicationController
  before_action :set_post!, only: [:show, :edit, :update]

  def show
    @post = Post.find(params[:id])
  end

  def edit
  end

  def update
    @post.update(post_params)
    if @post.valid?
    

    redirect_to post_path(@post)
    else
      render "edit"
    end
  end

  private

  def post_params
    params.permit(:category, :content, :title)
  end

  def set_post!
    @post = Post.find(params[:id])
  end
end
```

