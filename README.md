# Larazon
A multi-user eCommerce platform writen in Laravel! 

### Design a database table to hold user comments on a product. 

Since comments are by far the coolest part of Amazon, I figured we should start there. First we need to create a database migration profile, using the make:migration Artisan command:

```
php artisan make:migration create_comments_table
```

Then we have to define the database Schema inside the migration for our newly created "create_comments_table" profile located inside of the database/migrations directory. We have to define both the up() function, which is initialized when moving the application from the staging environment  into  production, and the down() function, which we can trigger if we ever want to revert the changes. 

```

class CreateCommentsTable extends Migration
{
    /**
     * Run the database migration that creates the comments table.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('comments', function (Blueprint $table) {
            $table->increments('id');
            $table->timestamps();
            $table->text('content');
            $table->integer('user_id')->unsigned();
            $table->integer('product_id');

        });

        Schema::table('comments', function(Blueprint $table) {
            $table->foreign('user_id')->references('id')->on('users')
                ->onDelete('restrict')
                ->onUpdate('restrict');
        });

    }

    /**
     * Reverse the database migration and drop the comments table.
     *
     * @return void
     */
    public function down()
    { 

        Schema::drop('comments');
    }
}

```

Next, from inside the project directory, and inside the Homestead VM we execute the migrate Artisan command:

```
php artisan migrate
```

