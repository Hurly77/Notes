## Callbacks

we can make **bits of code run** whenever something happens in our model: **like** when it's **created** (but **not** yet **saved** to the database), **saved to** the database, or even **deleted**.

Everything we cover here is called an **Active Record Lifecycle Callback**

```ruby
class Post < ActiveRecord::Base

  belongs_to :author
  validate :is_title_case 
   #make sure that post titles are in title case

  before_validation :make_title_case
	#This one happens before validation
  before_save :email_author_about_post
    #before_save is called after validation occurs.

  private

  def is_title_case
    if title.split.any?{|w|w[0].upcase != w[0]}
      errors.add(:title, "Title must be in title case")
    end
  end

  def email_author_about_post
    # Not implemented.
    # For more information: https://guides.rubyonrails.org/action_mailer_basics.html
  end

  def make_title_case
    self.title = self.title.titlecase
  end
end
```

Here is a rule of thumb: **Whenever you are modifying an attribute of the model, use `before_validation`. If you are doing some other action, then use `before_save`.**

### Before Save

`before_save` for actions that need to occur that aren't modifying the model itself.

### Before Create

Before you move on, let's cover one last callback that is useful: `before_create`. `before_create` is very close to `before_save` with one major difference: it only gets called when a model is created for the first time. This means not every time the object is persisted, just when it is **new**.

For more information on all of the callbacks available to you, check out [this amazing rails guide](http://guides.rubyonrails.org/active_record_callbacks.html)

# notes

