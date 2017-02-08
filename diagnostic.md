# Rails API Relationships Diagnostic

Place your responses inside the fenced code-blocks where indivated by comments.

1.  Describe a reason why a join tables may be valuable.

  ```md
    Join tables facilitate many-to-many relationships.
  ```

1.  Provide a database table structure and explain the Entity Relationship that
  describes a many-to-many relationship for `Profiles`, `Movies` and `Favorites`
  (Think of Netflix). A `Profile` has a `given_name`, `surname` and `email` and a
  `Movies` have `title`, `release_date`, and `length` and `Favorites` would be the
  join table with references to `Movies` and `Profiles`.

  ```md
profiles
- id (integer PK)
- given_name (string)
- surname (string)
- email (string)

movies
- id (integer PK)
- title (string)
- release_date (date)
- length (integer)

favorites
- id (integer PK)
- movie_id (integer FK, references movies(id))
- profile_id (integer FK, references profiles(id))
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
    has_many :profiles, through: :favorites
    has_many :favorites
  end
  ```

  ```rb
  class Favorite < ActiveRecord::Base
    belongs_to :movie, inverse_of: :favorites
    belongs_to :profile, inverse_of: :favorites
  end
  ```

1.  What is the purpose of a serializer? What would our `Profile` serializer look
like to show all movies favorited by a profile on
`http://localhost:3000/profiles/1`

  ```md
    A serializer controls what data is returned from a request.
  ```

  ```rb
  class ProfileSerializer < ActiveModel::Serializer
    attributes :id, :given_name, :surname
    has_many :movies
  end
  ```

1.  What would the command be to _scaffold_ out a **join table** for Favorites from
the above `Movies` and `Profiles`.

  ```sh
    bundle exec rails g scaffold favorites movie:references profile:references
  ```
  ```rb
  class Movie < ActiveRecord::Base
    has_many :profiles, through: :favorites
    has_many :favorites, dependent: :destroy
  end
  ```
1.  What is `Dependent: Destroy` and where/why would we use it?

  ```md
  'dependent: :destroy is used in a model to indicate that when you delete an
'instance of that class, you should also delete associated objects.
  ```

1.  Think of **ANY** example where you would have a one-to-many relationship as well
as a many-to-many relationship in an application. You only need to list the
description about the resources and how they relate to one another.

  ```md
  In a simple blog CMS, one author can have many posts, but each post can only
  have one author. One post can have many tags, and each tag could be applied to
  many posts.
  ```
