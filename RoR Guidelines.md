#Guides 

Guia de estilo para desenvolvimento em Ruby On Rails.

#Naming 

* Avoid abbreviations.
* Avoid types in names (`user_array`).
* Name the enumeration parameter the singular of the collection.
* Name variables, methods, and classes to reveal intent.
* Use Snake Case  (`lower_case_separeted_by_underscore`).
* Constants (`ALL_UPPERCASE`).

##Local Variables
(`order_amount`).

##Instance Variables
(`@colour`).


## Model Naming 
Table: orders
Class: Order
File: /app/models/order.rb
Primary Key: id
Foreign Key: customer_id
Link Tables: items_orders

##Controller Naming Convention

Class: OrdersController
File: /app/controllers/orders_controller.rb
Layout: /app/layouts/orders.html.erb

##View Naming Convention

Helper: /app/helpers/orders_helper.rb

Helper Module: OrdersHelper

Views: /app/views/orders/… (list.html.erb for example)

##Tests Naming Convention

Unit: /test/unit/order_test.rb
Functional: /test/functional/orders_controller_test.rb
Fixtures: /test/fixtures/orders.yml



# Controllers

* Keep the controllers skinny - they should only retrieve data for the
  view layer and shouldn't contain any business logic (all the
  business logic should naturally reside in the model).
* Each controller action should (ideally) invoke only one method other
  than an initial find or new.
* Share no more than two instance variables between a controller and a view.



# Models

* Name the models with meaningful (but short) names without
abbreviations.


    ```Ruby
    class Article
      has_many :comments
      belongs_to :author

      default_scope order("id desc")
      scope :published, where(:published => true)
      scope :created_after, lambda{|time| ["created_at >= ?", time]}

      attr_accessible :name, :email, :content

      validates :name, presence: true
      validates :email, format: { with: /\A[-a-z0-9_+\.]+\@([-a-z0-9]+\.)+[a-z0-9]{2,4}\z/i }
      validates :content, length: { maximum: 500 }

      def init
        # do stuff here
        # ...
        # do stuff here
      end

    end
    ```

# Functions

##Boolean

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





