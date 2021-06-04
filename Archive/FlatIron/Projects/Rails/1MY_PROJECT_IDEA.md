# Idea

## Trivia App

## Model

```ruby
class TriviaGame 
    has_many :games
    has_many :categories
    has_many :rounds
    has_many :questions
    has_many :likes
    has_many :users, through: :likes
end
```

```ruby
class Like 
    belongs_to :user
    belongs_to :triviagame
end
```

```ruby
class User
    has_many :games
    has_many :likes
    has_many :triviagames, through: :likes
	has_many :categories, through: :games
    has_many :questions, through: :games
end
```

```ruby
class Game
    has_many :rounds
    has_many :questions
    belongs_to :user
    belongs_to :category
    belongs_to :TriviaGame
end
```

```ruby
class Category
    has_many :games
    has_mnay :questions
    has_many :users, through: :games
    has_many :rounds, through: :questions
end
```

```ruby
class Question
    belongs_to :round
    belongs_to :category
    belongs_to :game
    belongs_to :user
end
```

```ruby
class Round
    belongs_to :game
 	belongs_to :triviaGame
    has_one :question
end
```



## Admin

```ruby
class Admin::QuestionsController < ApplicationController
    #This would be for creating New questions for users  
    new
    create
    update
end
```

```ruby
class Admin::TrivaGame < ApplicationController
    #this should be Where an Admin will Create a new Trivia Game index
    
     index#should list all the Triva games
     new#should render a page for a form to create new trivia game
     create#should create said Trivia game
     show#should show the game update #should update the game if Admin so chooses
     destroy #should destoy if the admin who create the game so chooses
```



## Views

###### application

```
home
login or signup
```

###### sessions

```erb
<!--new-->
	<%=form_for(@session) do %>
	<%=end%>
```

###### static

```

```

###### users

```
new - this would be accencially where the dashboard would be
		# There would be a link to categories, 
show
```

###### games

``` 
index - this might be where a button called, "Start Game" might be
```

###### categories

```
index - this could be where a user might be able to select a category
	# there would be links to all categories
show
	# here there would be links to specified category links
```

###### rounds

```
new - here we might see the question and
```



## Controllers

```ruby
class ApplicationController
	def home
	end
end
```

```ruby
class SessionsController
    #this should handle signing in an exitsting user
end
```

```ruby
class StaticController
    #this should controller the routs for any non-dynamic views
end
```

```ruby
class UsersController
    #this should controller Signing up new user and editing and existin user Profile
end
```

```ruby
class GamesController
    #this should be responsible for Creating a new Game for a user
end
```

```ruby
class CategoriesController
    #this should be only accessible to a user if they want to pick a subject or
    #IF and ADMIN would like to CREATE a NEW Subject obj
end
```

```ruby
class RoundsController
    #Create new Round for a Particular Users Game
end
```



## routes

```ruby
Rails.application.routes.draw
	root "application#home)"
	get '/login', to: 'sessions#new', as: 'login'
  	post '/login', to: 'sessions#create'
	get '/signout', to: 'sessions#destroy', as: 'signout'

	resouces :users do
        resouces :categories only: [:index, :show]
        resouces :likes only: [:show]
    end

	resouces :categories
	resouces :triviagames
	resouces :games do 
    	resources :rounds except: :index
        resouces :categories
    end

	namespace :admin do
    	resources :TriviaGame
        resources :Questions
    end
end
```

## Helpers

```ruby
class GamesPlayed
    #would Display Games played for a pitcular user
end
```

```ruby
class BestSubjects
	#would display a user's best subject is
end
```

```ruby
class AverageScore
    #would display a user's score average
end
```

```ruby
class Score
    #would display a scoure for a pitcular user
end
```

## DB/Migrations

```ruby
class CreateLikes < ActiveRecord::Migrations
	def change
        create_table :likes do |f|
            f.integer :user_id
            f.integer :triviagame_id
            f.boolean :liked default: nil
    end
end
```

```ruby
class CreateUsers < ActiveRecord::Migrations
	def change
        create_table :users do |f|
           	f.string :name
            f.string :password_digest
            f.integer :points
    end
end
```

```ruby
class CreateGames < ActiveRecord::Migrations
	def change
        create_table :games do |f|
            f.integer :user_id
            f.integer :subject_id
    end
end
```

```ruby
class CreateCategories < ActiveRecord::Migrations
	def change
        create_table :subjects do |f|
            f.string :name
            f.belongs_to :triviagame
    end
end
```

```ruby
class CreateRounds < ActiveRecord::Migrations
	def change
        create_table :rounds do |f|
            f.integer :user_id
            f.integer :question_id
            f.integer :game_id
    end
end
```

```ruby
class CreateQuestions < ActiveRecord::Migrations
	def change
        create_table :questions do |f|
            f.string :question
            f.string :difficulty
            f.string :correct_answer
            f.string :incorrect_answers
           	f.integer :points
            f.belongs_to :round
    		f.belongs_to :category
            f.belongs_to :game
            f.belongs_to :user
    end
end
```

```ruby
class CreateTriviaGame < ActiveRecord::Migrations
	def change
        create_table :TriviaGame do |f|
            f.string :name
    end
end
```



## dynamics

## seed

```ruby
#triviagames
TriviaGame.create(name: "Film")
#likes
#users
#games
#categories
Categories.create(id:1, category: "Entertainment: Film")
#Questions
#easy
Quesion.create(question: "In the 1995 film &quot;Balto&quot;, who are Steele&#039;s accomplices?", difficulty: "easy", correct_answer: "Kaltag, Nikki, and Star", incorrect_answers: ["Dusty, Kirby, and Ralph", "Nuk, Yak, and Sumac", "Jenna, Sylvie, and Dixie"], points: 20, category_id: 1)

Question.create(question: "Who starred as Bruce Wayne and Batman in Tim Burton&#039;s 1989 movie &quot;Batman&quot;?", difficulty: "easy", correct_answer: "Michael Keaton", incorrect_answers:["George Clooney", "Val Kilmer", "Adam West"], points: 20, category_id: 1)

Question.create(questiont: "In the 1992 film &quot;Army of Darkness&quot;, what name does Ash give to his double-barreled shotgun?", correct_answer: "Boomstick", incorrect_answers: ["Bloomstick", "Blastbranch", "2-Pump Chump"], points: 20, category_id: 1)

Question.create("question": "When does &quot;Rogue One: A Star Wars Story&quot; take place chronologically in the series?", difficulty: "easy", correct_answer: "Between Episode 3 and 4", incorrect_answers: ["After Episode 6", "Before Episode 1", "Between Episode 4 and 5"], points: 20, category_id: 1)

Question.create(question: "Which of these actors/actresses is NOT a part of the cast for the 2016 movie &quot;Suicide Squad&quot;?", difficulty: "easy", correct_answer: "Scarlett Johansson", incorrect_answers: ["Jared Leto", "Will Smith", "Margot Robbie"], points: 20, category_id: 1)

Question.create(difficulty: "easy", "question": "Which actress danced the twist with John Travolta in &#039;Pulp iction&#039;?", correct_answer: "Uma Thurman", incorrect_answers: ["Kathy Griffin", "Pam Grier", "Bridget Fonda"], points: 20, category_id: 1)

Question.create(question: "Who directed &quot;E.T. the Extra-Terrestrial&quot; (1982)?", difficulty: "easy", correct_answer: "Steven Spielberg", incorrect_answers: ["Stanley Kubrick", "James Cameron","Tim Burton"], points: 20, category_id: 1)
#medium
#hard
#rounds
```

