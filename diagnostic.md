# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    # <To stablish a two way relationship between two main sources/tables.  >
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
    # < profile              Favorite                Movie
        given_name                                   title
        surname                                      release_date
        email                                        length

        has_many :movies,     belongs_to :Profile    has_many :profiles, through :favorites
        through: :favorites   belongs_to :movie      has_many :favorites
        has_many :movies
        >
  ```

1.  For the above example, what needs to be added to the Model files?

  ```rb
  class Profile < ActiveRecord::Base
  has_many :movies, through: :favorites
  has_many :favorites
  end
  ```

  ```rb
  class Movie < ActiveRecord::Base
  has_many profiles through: :favorites
  has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
  belongs_to :profile
  belongs_to :movie
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    # < The purpose of the serializer is to customize the json format that I will get as a response. >
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :given_name, :surname, :email, :movies
    def movies
     object.movies.pluck(:id)
    end
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    # < bin/rails generate scaffold favorites movie_id :integer profile_id :integer >
  ```

1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
    # < Dependent destroy is a method used to delete the links established in the join table, if we delete one item from one table, the association gets deleted as well. :destroy is used in our main models   >
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
    # < One to many: One user has many podcasts, one podcast has many users.
    many to many: Many podcasts have many users through favorites, many users have many podcasts through favorites.  >
  ```
