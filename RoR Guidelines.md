Guideline for Ruby On Rails.

#Naming 

* Avoid abbreviations.
* Avoid types in names (`user_array`).
* Name the enumeration parameter the singular of the collection.
* Name variables, methods, and classes to reveal intent.
* Use Snake Case  (`lower_case_separeted_by_underscore`).
* Constants (`ALL_UPPERCASE`).

###Local Variables
(`order_amount`).

###Instance Variables
(`@colour`).


## Model Naming 
**Table:** orders<br />
**Class:** Order<br />
**File**: /app/models/order.rb<br />
**Primary Key:** id<br />
**Foreign Key:** customer_id<br />
**Link Tables:** items_orders<br />

##Controller Naming Convention

**Class:** OrdersController<br />
**File:** /app/controllers/orders_controller.rb<br />
**Layout:** /app/layouts/orders.html.erb<br />

##View Naming Convention

**Helper:** /app/helpers/orders_helper.rb<br />
**Helper Module:** OrdersHelper<br />
**Views:** /app/views/orders/… (list.html.erb for example)<br />

##Tests Naming Convention

**Unit:** /test/unit/order_test.rb<br />
**Functional:** /test/functional/orders_controller_test.rb<br />
**Fixtures:** /test/fixtures/orders.yml<br />



# Controllers

* Keep the controllers skinny - they should only retrieve data for the
  view layer and shouldn't contain any business logic (all the
  business logic should naturally reside in the model).


# Models

* Name the models with meaningful (but short) names without
abbreviations.


    ```Ruby
    class Article
      # associations
      has_many :comments
      belongs_to :author

      #scopes
      default_scope order("id desc")
      scope :published, where(:published => true)
      scope :created_after, lambda{|time| ["created_at >= ?", time]}

      # atributes
      attr_accessible :name, :email, :content

      #validates
      validates :name, presence: true
      validates :email, format: { with: /\A[-a-z0-9_+\.]+\@([-a-z0-9]+\.)+[a-z0-9]{2,4}\z/i }
      validates :content, length: { maximum: 500 }

      # class methods
      def sel.finit
        # do stuff here
        # ...
        # do stuff here
      end

      # instance methods
      def any_instance_method
        # ...
      end

    end
    ```
#Helpers
* Bad Smell
```Ruby
   <%= select_tag :state, options_for_select( [[t(:draft), "draft"],
                                            [t(:published), "published"]],
                                           params[:default_state] ) %>
    ```
*Refactor
```Ruby
   <%= select_tag :state, options_for_post_state(params[:default_state]) %>

# app/helpers/posts_helper.rb
def options_for_post_state(default_state)
  options_for_select( [[t(:draft), "draft"], [t(:published), "published"]],
                      default_state )
end
    ```


# Functions
```Ruby
self.run
self.can_run? # can this object run?
self.running_allowed? # is running allowed here or by this user?

self.redeem!
self.redeemable? # is this object currently redeemable?

self.copy_to_clipboard
self.copy_to_dashboard
self.copyable? or self.can_be_copied? #can this object be copied
self.copy_allowed?(:clipboard) # is copying to the clipboard allowed by me?
```


# Routing

* When you need to add more actions to a RESTful resource (do you
  really need them at all?) use `member` and `collection` routes.

    ```Ruby
    # bad
    get 'subscriptions/:id/unsubscribe'
    resources :subscriptions

    # good
    resources :subscriptions do
      get 'unsubscribe', on: :member
    end

    # bad
    get 'photos/search'
    resources :photos

    # good
    resources :photos do
      get 'search', on: :collection
    end
    ```

* If you need to define multiple `member/collection` routes use the
  alternative block syntax.

    ```Ruby
    resources :subscriptions do
      member do
        get 'unsubscribe'
        # more routes
      end
    end

    resources :photos do
      collection do
        get 'search'
        # more routes
      end
    end
    ```

* Use nested routes to express better the relationship between
  ActiveRecord models.

    ```Ruby
    class Post < ActiveRecord::Base
      has_many :comments
    end

    class Comments < ActiveRecord::Base
      belongs_to :post
    end

    # routes.rb
    resources :posts do
      resources :comments
    end
    ```

* Use namespaced routes to group related actions.

    ```Ruby
    namespace :admin do
      # Directs /admin/products/* to Admin::ProductsController
      # (app/controllers/admin/products_controller.rb)
      resources :products
    end
    ```

* Never use the legacy wild controller route. This route will make all
  actions in every controller accessible via GET requests.

    ```Ruby
    # very bad
    match ':controller(/:action(/:id(.:format)))'
    ```

#Good Documentation
Comments should add to the clarity of your code. 
Keep comments simple. Some of the best comments I have ever seen are simple, point-form notes. You do not have to write a book, you just have to provide enough information so that others can understand your code.

Write the documentation before you write the code. The best way to document code is to write the comments before you write the code.





