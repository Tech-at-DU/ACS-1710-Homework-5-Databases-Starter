# Homework 5: Databases

With this project you will create a web site that uses a database to track plants and harvests. 

You will create the entire project from scratch. The project will be built with Python and Flask. 

## Creating Files and folders

Start by creating the files and folders you will need. 

Create a folder for your project: 

```
acs-1710-assignment-5/
├── app.py
├── requirements.txt
├── static/
└── templates/
```

Add the following to `requirements.txt`:

```
blinker==1.9.0
certifi==2024.8.30
charset-normalizer==3.4.0
click==8.1.7
dnspython==2.7.0
Flask==3.1.0
Flask-PyMongo==2.3.0
idna==3.10
itsdangerous==2.2.0
Jinja2==3.1.4
MarkupSafe==3.0.2
pymongo==4.10.1
python-dotenv==1.0.1
requests==2.32.3
urllib3==2.2.3
Werkzeug==3.1.3
```

This project uses restful route mapping: 

| Purpose                | Method | Route                         | View Function       |
| ---------------------- | ------ | ----------------------------- | ------------------- |
| List all plants        | GET    | `/`                           | `list_plants()`     |
| Show form to add plant | GET    | `/plants/new`                 | `new_plant_form()`  |
| Create a new plant     | POST   | `/plants/new`                 | `create_plant()`    |
| View a specific plant  | GET    | `/plants/<plant_id>`          | `show_plant()`      |
| Show Edit a plant form | GET    | `/plants/<plant_id>/edit`     | `edit_plant_form()` |
| Update a plant         | PUT    | `/plants/<plant_id>/edit`     | `update_plant()`    |
| Delete a plant         | DELETE | `/plants/<plant_id>/delete`   | `delete_plant()`    |
| Create a harvest       | POST   | `/plants/<plant_id>/harvests` | `create_harvest()`  |

Stub the routes for each of these in `app.py`:

```python
from flask import Flask, request, redirect, render_template, url_for
from flask_pymongo import PyMongo
from bson.objectid import ObjectId

############################################################
# SETUP
############################################################

app = Flask(__name__)

app.config["MONGO_URI"] = "mongodb://localhost:27017/plantsDatabase"
mongo = PyMongo(app)

############################################################
# ROUTES
############################################################

# List all plants
@app.route('/')
def list_plants():
    """Display the plants list page."""

    # TODO: Replace the following line with a database call to retrieve *all*
    # plants from the Mongo database's `plants` collection.
    plants_data = ''

    context = {
        'plants': plants_data,
    }
    return render_template('list_plants.html', **context)


# Displays about page
@app.route('/about')
def about():
    """Display the about page."""
    return render_template('about.html')


# GET Shows the add plant form, POST creates a new plant 
@app.route('/plants/new', methods=['GET', 'POST'])
def create():
    """Display the plant creation page & process data from the creation form."""
    if request.method == 'POST':
        # TODO: Get the new plant's name, variety, photo, & date planted, and 
        # store them in the object below.
        new_plant = {
            'name': '',
            'variety': '',
            'photo_url': '',
            'date_planted': ''
        }
        # TODO: Make an `insert_one` database call to insert the object into the
        # database's `plants` collection, and get its inserted id. Pass the 
        # inserted id into the redirect call below.

        return redirect(url_for('detail', plant_id=''))

    else:
        return render_template('create.html')


# View a specific plant with id
@app.route('/plants/<plant_id>')
def detail(plant_id):
    """Display the plant detail page & process data from the harvest form."""

    # TODO: Replace the following line with a database call to retrieve *one*
    # plant from the database, whose id matches the id passed in via the URL.
    plant_to_show = ''

    # TODO: Use the `find` database operation to find all harvests for the
    # plant's id.
    # HINT: This query should be on the `harvests` collection, not the `plants`
    # collection.
    harvests = ''

    context = {
        'plant' : plant_to_show,
        'harvests': harvests
    }
    return render_template('detail.html', **context)


# Create a harvest 
@app.route('/plants/<plant_id>/harvests', methods=['POST'])
def harvest(plant_id):
    """
    Accepts a POST request with data for 1 harvest and inserts into database.
    """

    # TODO: Create a new harvest object by passing in the form data from the
    # detail page form.
    new_harvest = {
        'quantity': '', # e.g. '3 tomatoes'
        'date': '',
        'plant_id': plant_id
    }

    # TODO: Make an `insert_one` database call to insert the object into the 
    # `harvests` collection of the database.

    return redirect(url_for('detail', plant_id=plant_id))


# GET shows plant edit form, PUT updates a plant 
@app.route('/plants/<plant_id>/edit', methods=['GET', 'PUT'])
def edit(plant_id):
    """Shows the edit page and accepts a POST request with edited data."""
    if request.method == 'PUT':
        # TODO: Make an `update_one` database call to update the plant with the
        # given id. Make sure to put the updated fields in the `$set` object.

        
        return redirect(url_for('detail', plant_id=plant_id))
    else:
        # TODO: Make a `find_one` database call to get the plant object with the
        # passed-in _id.
        plant_to_show = ''

        context = {
            'plant': plant_to_show
        }

        return render_template('edit.html', **context)


# Delete a plant 
@app.route('/plants/<plant_id>/delete', methods=['POST'])
def delete(plant_id):
    # TODO: Make a `delete_one` database call to delete the plant with the given
    # id.

    # TODO: Also, make a `delete_many` database call to delete all harvests with
    # the given plant id.

    return redirect(url_for('list_plants'))

if __name__ == '__main__':
    app.run(debug=True)

```

Read the code above closely, and match the routes to the table above showing the routes, methods, and purposes. 

Get familiar with the url for each route and the method. I've stubbed in some comments maked `TODO`, you'll take care of these later. For now, notice that some of the routes handle more than one method. For example, `/plants/<plant_id>/edit` handles `GET` and `PUT` requests. Inside the function an if else block handles each of these methods. 

The comments tell you what you need to do to complete the assignment. Start solving these after you complete the project setup. 

## Static folder

In the static folder you'll serve files that are not dynamic. Create two files in the static folder: 

```
static/
├── index.js
└── style.css
```

In `static/index.js` add the following: 

```js
function confirm_delete() {
    return confirm('Are you sure you want to delete?')
}
```

This will be used to ask people to confirm before deleting plants and harvests. 

In `static/style.css` add the following: 

```CSS
body {
    color: #222;
    font-family: 'Helvetica', sans-serif;
    margin: 0;
    padding: 0;
    position: relative;
    padding-bottom: 100px;
}

main {
    width: 800px;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
    margin: auto;
    margin-top: 20px;
    min-height: 100vh;
}

section {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: center;
}

nav {
    width: 100%;
    padding: 0;
    margin: 0;
    height: 40px;
    display: flex;
    justify-content: center;
    background-color: #229954;
    position: relative;
    top: 0;
    left: 0
}

nav a {
    margin: auto 0;
    padding: 0 25px;
    background-color: #229954;
    color: white;
    text-decoration: none;
}

footer {
    width: 100%;
    padding: 0;
    margin: 0;
    height: 40px;
    display: flex;
    justify-content: center;
    align-items: center;
    align-content: center;
    background-color: #229954;
    color: white;
    position: absolute;
    bottom: 0;
    left: 0
}

a.nav, input.nav, form input.submit {
    border: none;
    background-color: #229954;
    border-radius: 3px;
    color: white;
    padding: 10px 15px;
    text-decoration: none;
    font-size: 1em;
}

div#content {
    width: 100%;
    text-align: center;
}

div#items {
    display: flex;
    justify-content: flex-start;
    flex-wrap: wrap;
    margin-top: 50px;
}

div.card {
    /* box-shadow: 0 4px 8px 0 rgba(0,0,0,0.2);
    transition: 0.3s; */
    border: 1px solid #229954;
    border-radius: 10px;
    width: 200px;
    height: 200px;
    margin: 30px;
}

div.card img {
    width: 100%;
    height: 150px;
    object-fit: cover;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
}

div.card-content {
    display: flex;
    flex-direction: column;
    justify-content: center;
    height: 50px;
}

div.card-content a {
    color: #229954;
    font-weight: bold;
    text-decoration: none;
    align-self: center;
}

h1, h2, h3 {
    color: #229954;
    font-family: 'Homemade Apple', cursive;
}

form {
    width: 100%;
    margin: 30px 0;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

form fieldset {
    border: 1px solid #229954;
    border-radius: 20px;
    padding: 10px 20px;
    width: 100%;
}

form legend {
    color: #229954;
    text-align: center;
    font-family: 'Homemade Apple', cursive;
}

form label {
    display: flex;
    flex-direction: column;
    margin: 5px 0;
}

form input {
    border: 1px solid #229954;
    padding: 10px;
    margin: 10px 0;
    max-width: 700px;
}

form input.submit {
    align-self: flex-end;
}

form#delete-form {
    display: flex;
    align-items: center;
}

div.info {
    margin-top: 30px;
}

div.harvest-history {
    margin: 30px 0;
    color: #666;
}
```

This is some default styles that will be used on the pages you will will create. 

## Templates 

Your site will have a couple templates. The sample code provide for the default templates will need some work! 

In the templates folder create the following files: 

```
templates/
├── about.html
├── base.html
├── create.html
├── detail.html
├── edit.html
└── plant_list.html
```

`about.html`: 

```HTML
{% extends 'base.html' %}
{% block content %}

<main>
    <section id="content">
        <h1>About Us</h1>
        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
        </p>
    </section>
</main>

{% endblock content %}
```

This a placeholder page. We can use this for testing our server. 

`base.html`:

```HTML
<!DOCTYPE html>
<html>
    <head>
        <title>My Plants</title>
        <link rel="stylesheet" href="/static/style.css">
        <link href="https://fonts.googleapis.com/css2?family=Homemade+Apple&display=swap" rel="stylesheet">
    </head>
    <body>

        <nav>
            <a href="/">Home</a>
            <a href="/about">About</a>
        </nav>

        {% block content %}{% endblock %}


        <footer><div>
            Copyright 2020 @ makeschool.com
        </div></footer>

        <script src="/static/index.js"></script>
    </body>
</html>
```

This is the base template for all of the other pages. It imports the style sheet and js files from the static folder. Find those here. 

`create.html`:

```HTML
{% extends 'base.html' %}
{% block content %}


<main>
    <h1>New Plant</h1>

    <form method='POST'>
        <fieldset>
            <legend>Please fill out your plant's info:</legend>

            <label>
                Plant's name:
                <input type="text" name="plant_name">
            </label>
            
            <label>
                Plant's variety:
                <input type="text" name="variety">
            </label>

            <label>
                Plant's photo URL:
                <input type="url" name="photo">
            </label>

            <label>
                Date planted:
                <input type="date" name="date_planted">
            </label>

            <input type="submit" class="submit" value="Submit!">
        </fieldset>
    </form>
</main>

{% endblock content %}
```

The content here will be inserted into the base template when we show the route `/plants/new`. 

`detail.html`:

```HTML
{% extends 'base.html' %}
{% block content %}


{#
    TODO: Much of the data on this page is hard-coded! Replace the name, photo,
    variety, date planted, & id with the plant object's actual data, using what
    is passed from the route's context. 
    
    HINT: For example, you can use `plant.name` or `plant['name']` for the
    plant's name.
#}

<main>
        <h1>Tomato</h1>

        <img src="https://www.almanac.com/sites/default/files/styles/primary_image_in_article/public/image_nodes/tomatoes_helios4eos_gettyimages-edit.jpeg?itok=4KrW14a4" alt="some plant">

        <section id="info">
            <h3>Info</h3>

            <strong>Date Planted</strong>
            3/20/2020
            <br><br>

            <strong>Variety</strong>
            Early Girl
            <br><br>

            <a class="nav" href="/plants/123456/edit">Edit Plant</a><br><br>
        </section>

        <form action="/plants/123456/harvest" method="POST">
            <fieldset>
                <legend>Harvested:</legend>
                
                <label>
                    Amount harvested
                    <input type="text" name="harvested_amount" placeholder="e.g. 2 tomatoes">
                </label>

                <label>
                    Date harvested
                    <input type="date" name="date_planted">
                </label>

                <input type="submit" class="submit" value="Harvest!">
            </fieldset>
        </form>

        {#
            TODO: Create a for loop here to loop over all harvests (passed in 
            from the route's context) and display their information.
        #}
        <section id="harvest-history">
            <h3>Harvest History</h3>

            <ul>
                <li>5/15/2020: Harvested 3 tomatoes</li>
                <li>5/18/2020: Harvested 2 tomatoes</li>
            </ul>
        </section>

        <form action="/plants/123456/delete" method='POST' id="delete-form" onsubmit="return confirm_delete()">
            <input type="submit" class="nav" onclick="delete_modal()" value="Delete Plant">
        </form>
</main>

{% endblock content %}
```

This page shows details for a plant. It will show the name, image, and harvests for that plant. 

The template page shows a mock up of what will be displayed. You'll replace this with code that displays data provided by your server. 

`edit.html`:

```html
{% extends 'base.html' %}
{% block content %}

{#
    TODO: Modify this page to show the pre-existing data for the given plant
    object, using the context data passed in from the route.
#}

<main>
    <h1>Edit Plant</h1>

    <form method='POST'>
        <fieldset>
            <legend>Please fill out your plant's info:</legend>

            <label>
                Plant's name:
                <input type="text" name="plant_name" value="Tomato">
            </label>
            
            <label>
                Plant's variety:
                <input type="text" name="variety" value="Early Girl">
            </label>

            <label>
                Plant's photo URL:
                <input type="url" name="photo" value="https://mypic.co/image.png">
            </label>

            <label>
                Date planted:
                <input type="date" name="date_planted" value="2020-08-01">
            </label>

            <input type="submit" class="submit" value="Submit!">
        </fieldset>
    </form>
</main>

{% endblock content %}
```

This page shows a form that allows us to update a plant's info. You'll need to display the data for a given plant in the form fields. 

`plants_list.html`:

```HTML
{% extends 'base.html' %}
{% block content %}


<main>
    <div id="content">
        <h1>My Plants</h1>

        <a href="/plants/new" class="nav">+ New Plant</a>

        {#
            TODO: All of the following cards are what we'd refer to as
            "hard-coded". That is, their values do not actually come from the 
            database! Refactor this page to use a 'for' loop to display all
            plants with the data passed from the route function's context.

            HINT: Use the plant's _id field as the id for constructing URLs.
        #}

        <div id="items">
            <div class="card">
                <a href="/plants/123456"><img src="https://www.almanac.com/sites/default/files/styles/primary_image_in_article/public/image_nodes/tomatoes_helios4eos_gettyimages-edit.jpeg?itok=4KrW14a4"></a>
                <div class="card-content">
                    <a href="/plants/123456">Tomato</a>
                </div>
            </div>

            <div class="card">
                <a href="/plants/234567"><img src="https://opimedia.azureedge.net/-/media/Images/MEN/Editorial/Blogs/Organic-Gardening/Growing-Corn-from-Sowing-to-Harvest/corn-jpg.jpg?h=367&w=550&la=en&hash=9675303A51AB99E3007C9C1B13CB72BDE9D9C561"></a>
                <div class="card-content">
                    <a href="/plants/234567">Corn</a>
                </div>
            </div>

            <div class="card">
                <a href="/plants/345678"><img src="https://cdn.shopify.com/s/files/1/1698/1675/products/Herb_Italian_Basil.jpg?v=1557458148"></a>
                <div class="card-content">
                    <a href="/plants/345678">Basil</a>
                </div>
            </div>

            <div class="card">
                <a href="/plants/456789"><img src="https://img.thrfun.com/img/077/665/jalapeno_peppers_s1.jpg"></a>
                <div class="card-content">
                    <a href="/plants/456789">Jalapeno</a>
                </div>
            </div>
        </div>
    </div>

</main>

{% endblock content %}
```

This the main page that shows a list of all of the plants created. This page mocks up four "cards" that show how plants will be displayed. 

Once you have all of the default files setup follow the [instructions](https://github.com/Tech-at-DU/ACS1710-Web-Architecture/blob/master/Assignments/04-Databases.md) to complete this assignment!
