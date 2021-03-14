<img src="C:\Users\camer\AppData\Roaming\Typora\typora-user-images\image-20200726134810156.png" alt="image-20200726134810156" style="zoom: 200%;" />

```ruby
class SessionsController < ApplicationController
  skip_before_action :verify_authenticity_token, only: :create

  def new
  end

  def create
    @user = User.find_by(name: params[:user][:name])
    if @user && @user.authenticate(password: params[:user][:password])
      session[:user_id] = @user.id
      redirect_to user_path(@user)
    else
      redirect_to new_session_path
    end
  end

  def destroy
    session.clear
    redirect_to root_path
  end
  
  private
  
  def auth
    request.env['omniauth.auth']
  end
end

--------------------------------------------------------------------
    ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
--------------------------------------------------------------------
    
class UsersController < ApplicationController
  before_action :current_user
  def new
    binding.pry
    @user = User.new
  end

  def create
    @user = User.new(user_params)
    if @user.save
      session[:user_id] = @user.id
      
      redirect_to user_path(@user)
    end
  end

  def show
    @user = User.find_by(id: params[:id])
  end

  private

  def user_params
    params.require(:user).permit(:name, :email, :uid, :password, :admin)
  end

  def require_login
    redirect_to root_path if session[:user_id].nil?
  end
end


```

```ruby
{
  "response_code": 0,
  "results": [
    {
      "category": "Entertainment: Film",
      "type": "multiple",
      "difficulty": "medium",
      "question": "Which of the following is NOT a quote from the 1942 film Casablanca? ",
      "correct_answer": "&quot;Frankly, my dear, I don&#039;t give a damn.&quot;",
      "incorrect_answers": [
        "&quot;Here&#039;s lookin&#039; at you, kid.&quot;",
        "&ldquo;Of all the gin joints, in all the towns, in all the world, she walks into mine&hellip;&rdquo;",
        "&quot;Round up the usual suspects.&quot;"
      ]
    }
```

 `Category.where("id > ?", 2).destroy_all`

```ruby


def urls
    urls = [
  "https://opentdb.com/api.php?amount=3&category=9&difficulty=easy&type=multiple",
  "https://opentdb.com/api.php?amount=3&category=10&difficulty=easy&type=multiple", "https://opentdb.com/api.php?amount=3&category=12&difficulty=easy&type=multiple"]
end

 
def urls_parse
  nest = urls.map {|url| JSON.parse(open(url).read)["results"]}.flatten
  nest.map {|hash| hash.delete("type")} 
  return nest
end

@data = urls_parse

def create_category
   categories = @data.map {|hash| hash["category"]}.uniq
  categories.each {|name| Category.find_or_create_by(name: name)}
  end

def category_ids
  @data.map do |hash| category = Category.find_by(name: hash["category"])
  	hash["category_id"] = category.id 
  end
     @data.each {|hash| hash.delete("category")}
end
    
def create_questions
	@data.each do |hash|
        q = Question.create(hash)
 	end
 end
```

```
 https://opentdb.com/api.php?amount=50&category=11&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=12&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=13&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=14&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=15&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=16&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=17&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=18&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=19&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=20&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=21&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=22&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=23&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=24&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=25&difficulty=easy&type=multiple
      https://opentdb.com/api.php?amount=50&category=26&difficulty=easy&type=multiple
```

```ruby
easy
urls = [
      "https://opentdb.com/api.php?amount=20&category=9&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=10&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=11&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=12&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=13&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=14&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=15&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=16&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=17&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=18&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=19&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=20&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=21&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=22&difficulty=easy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=23&difficulty=easy&type=multiple"
    ]

medium
urls = [
"https://opentdb.com/api.php?amount=20&category=9&difficulty=medium&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=10&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=11&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=12&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=13&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=14&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=15&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=16&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=17&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=18&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=19&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=20&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=21&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=22&difficulty=mediumy&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=23&difficulty=mediumy&type=multiple"
]


#hard
urls = [
      "https://opentdb.com/api.php?amount=20&category=9&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=10&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=11&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=12&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=13&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=14&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=15&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=16&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=17&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=18&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=19&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=20&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=21&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=22&difficulty=hard&type=multiple",
      "https://opentdb.com/api.php?amount=20&category=23&difficulty=hard&type=multiple"

    ]
```

```ruby
#multi liks

"https://opentdb.com/api.php?amount=50&difficulty=easy&type=multiple"
""
"https://opentdb.com/api.php?amount=10&category=19&difficulty=mediumy&type=multiple"
"https://opentdb.com/api.php?amount=10&category=19&difficulty=medium&type=multiple"
""
""
""
""
""
""
""
""
""
```



```
class User < ApplicationRecord
  has_many :games
  has_many :likes
  has_many :categories, through: :games
    
class Game < ApplicationRecord
  has_many :rounds
  has_many :questions
  belongs_to :user
  belongs_to :category
    
class Category < ApplicationRecord
  has_many :likes
  has_many :games
  has_many :questions
  has_many :users, through: :games
  has_many :rounds, through: :questions
         
class Round < ApplicationRecord
  belongs_to :game
  belongs_to :user
  belongs_to :question
    
class Question < ApplicationRecord
  has_one :round
  has_many :users, through: :rounds
  belongs_to :game, optional: true
  belongs_to :category
    
class Like < ApplicationRecord
  belongs_to :user
  belongs_to :category
```

