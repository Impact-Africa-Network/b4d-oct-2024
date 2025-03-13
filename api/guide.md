# Building Your First Backend API: A Google Keep Clone

This guide will walk you through creating your first backend API using Flask, UV, and SQLite3. We'll build a Google Keep clone (a note-taking application) while learning fundamental concepts of backend development.

## What is a Backend API?

Before we dive into coding, let's understand some key concepts:

### What is a Backend?

The "backend" is the server-side of an application that users don't directly interact with. It's responsible for:
- Storing and managing data
- Processing requests from the frontend (the user interface)
- Implementing business logic
- Ensuring security

### What is an API?

API stands for Application Programming Interface. In web development, an API is a set of rules that allows different software applications to communicate with each other. A web API:
- Receives requests from clients (like web browsers or mobile apps)
- Processes those requests
- Returns appropriate responses (usually in JSON format)

Think of an API like a waiter in a restaurant:
1. You (the client) place an order (make a request)
2. The waiter (API) takes your order to the kitchen (server)
3. The kitchen prepares your food (processes the request)
4. The waiter brings back your meal (returns a response)

### What is REST?

REST (Representational State Transfer) is a popular architecture style for designing networked applications. RESTful APIs follow these principles:

- **Resources**: Everything is a resource (in our case, notes)
- **HTTP Methods**: Use standard HTTP methods to perform actions:
  - GET: Retrieve data
  - POST: Create new data
  - PUT/PATCH: Update existing data
  - DELETE: Remove data
- **Stateless**: Each request contains all information needed to complete it

Don't worry if this seems complex now - we'll see these concepts in action as we build our application.

## Prerequisites

- Python 3.10 or newer installed
- Basic knowledge of Python
- Basic command line familiarity
- A text editor or IDE (like VS Code, PyCharm, etc.)

## Step 1: Understanding Our Project

We're building a Google Keep clone API that will:
- Store and manage notes
- Allow creating, reading, updating, and deleting notes
- Support features like pinning and archiving notes

This backend API will handle data storage and business logic, while a frontend application (which we have been working on in previous sessions) would provide the user interface.

## Step 2: Setting Up Your Development Environment

First, let's create a new project directory:

```bash
mkdir keep-clone
cd keep-clone
```

### Installing UV

[uv](https://docs.astral.sh/uv/getting-started/installation/) is a modern, fast Python package installer. Let's install it:

For *Nix systems: 

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
For Windows:

```bash
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
You can also install `uv` via `pip`:

```bash
pip install uv
```

### Creating a Virtual Environment

A virtual environment is an isolated space where we can install packages without affecting other Python projects:

```bash
uv venv
```

This creates a virtual environment in a `.venv` directory.

Now, activate this environment:

- On Windows:
  ```bash
  .venv\Scripts\activate
  ```

- On macOS/Linux:
  ```bash
  source .venv/bin/activate
  ```

Your command prompt should now show `(.venv)` at the beginning, indicating the environment is active.

### Understanding Virtual Environments

Virtual environments are important because:
- They keep dependencies for different projects separate
- They prevent version conflicts between projects
- They make your project portable and easier to set up on other machines

## Step 3: Installing Required Packages

Let's install the packages we need:

```bash
uv add flask flask-sqlalchemy
```

Let's understand what we're installing:
- **Flask**: A lightweight web framework for Python
- **Flask-SQLAlchemy**: An extension that adds SQLAlchemy support to Flask (SQLAlchemy is a database toolkit)

## Step 4: Project Structure

Let's create a basic structure for our project:

```
keep-clone/
├── app/
│   ├── __init__.py
│   ├── models.py
│   └── routes.py
├── instance/
├── .venv/
└── run.py
```

Create this structure:

```bash
mkdir -p app instance
touch app/__init__.py app/models.py app/routes.py run.py
```

### Understanding This Structure:

- **`app/`**: Contains our application code
  - **`__init__.py`**: Initializes our Flask application
  - **`models.py`**: Defines our database models (how our data is structured)
  - **`routes.py`**: Defines our API endpoints (URLs that clients can access)
- **`instance/`**: Will store our SQLite database
- **`.venv/`**: Contains our virtual environment
- **`run.py`**: Script to start our application

## Step 5: Setting Up the Database Models

Let's define what our notes will look like in the database. Edit `app/models.py`:

```python
from datetime import datetime
from flask_sqlalchemy import SQLAlchemy

# Initialize SQLAlchemy
db = SQLAlchemy()

class Note(db.Model):
    """Model for notes in our Google Keep clone"""
    
    # Each note has these attributes (columns in the database)
    id = db.Column(db.Integer, primary_key=True)
    title = db.Column(db.String(200))
    content = db.Column(db.Text)
    color = db.Column(db.String(20), default='white')
    is_pinned = db.Column(db.Boolean, default=False)
    is_archived = db.Column(db.Boolean, default=False)
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    updated_at = db.Column(db.DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
    
    def to_dict(self):
        """Convert note to dictionary (for JSON responses)"""
        return {
            'id': self.id,
            'title': self.title,
            'content': self.content,
            'color': self.color,
            'is_pinned': self.is_pinned,
            'is_archived': self.is_archived,
            'created_at': self.created_at.isoformat(),
            'updated_at': self.updated_at.isoformat()
        }
```

### Understanding Models:

- **Models** define the structure of our data
- Each model becomes a table in our database
- Each attribute becomes a column in that table
- SQLAlchemy is an ORM (Object-Relational Mapper) that lets us work with database data as Python objects

In our case, each note has:
- A title
- Content
- Color
- Status flags (pinned, archived)
- Timestamps

## Step 6: Initializing the Flask Application

Now let's set up our Flask application. Edit `app/__init__.py`:

```python
from flask import Flask
import os
from app.models import db

def create_app():
    """Initialize and configure the Flask application"""
    
    # Create the Flask application object
    app = Flask(__name__, instance_relative_config=True)
    
    # Configure the SQLite database
    app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///' + os.path.join(app.instance_path, 'notes.db')
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
    
    # Ensure the instance folder exists
    try:
        os.makedirs(app.instance_path)
    except OSError:
        pass
    
    # Initialize the database with our app
    db.init_app(app)
    
    # Register our routes (API endpoints)
    from app.routes import register_routes
    register_routes(app)
    
    # Create all database tables
    with app.app_context():
        db.create_all()
    
    return app
```

### Understanding This Code:

- We create a Flask application factory (`create_app`)
- We configure our database connection (SQLite)
- We initialize our database with our app
- We register our routes (we'll define these next)
- We create all database tables based on our models

## Step 7: Creating API Routes

Now, let's define our API endpoints in `app/routes.py`:

```python
from flask import request, jsonify
from app.models import db, Note

def register_routes(app):
    """Register all API routes with the Flask application"""
    
    # GET /api/notes - Get all notes
    @app.route('/api/notes', methods=['GET'])
    def get_notes():
        """Get all notes, with optional filtering"""
        # Check if we should show archived notes
        show_archived = request.args.get('archived', 'false').lower() == 'true'
        
        # Query the database for notes
        notes = Note.query.filter_by(is_archived=show_archived).order_by(
            Note.is_pinned.desc(),  # Pinned notes first
            Note.updated_at.desc()  # Then by most recently updated
        ).all()
        
        # Convert notes to dictionaries and return as JSON
        return jsonify([note.to_dict() for note in notes])
    
    # GET /api/notes/<id> - Get a specific note
    @app.route('/api/notes/<int:note_id>', methods=['GET'])
    def get_note(note_id):
        """Get a specific note by ID"""
        # Find the note or return 404 if not found
        note = Note.query.get_or_404(note_id)
        return jsonify(note.to_dict())
    
    # POST /api/notes - Create a new note
    @app.route('/api/notes', methods=['POST'])
    def create_note():
        """Create a new note"""
        # Get JSON data from the request
        data = request.get_json()
        
        # Create a new Note object
        note = Note(
            title=data.get('title', ''),
            content=data.get('content', ''),
            color=data.get('color', 'white'),
            is_pinned=data.get('is_pinned', False)
        )
        
        # Add to database and save changes
        db.session.add(note)
        db.session.commit()
        
        # Return the created note with 201 Created status
        return jsonify(note.to_dict()), 201
    
    # PUT /api/notes/<id> - Update a note
    @app.route('/api/notes/<int:note_id>', methods=['PUT'])
    def update_note(note_id):
        """Update an existing note"""
        # Find the note or return 404 if not found
        note = Note.query.get_or_404(note_id)
        
        # Get JSON data from the request
        data = request.get_json()
        
        # Update note fields if provided in the request
        if 'title' in data:
            note.title = data['title']
        if 'content' in data:
            note.content = data['content']
        if 'color' in data:
            note.color = data['color']
        if 'is_pinned' in data:
            note.is_pinned = data['is_pinned']
        if 'is_archived' in data:
            note.is_archived = data['is_archived']
        
        # Save changes to database
        db.session.commit()
        
        # Return the updated note
        return jsonify(note.to_dict())
    
    # DELETE /api/notes/<id> - Delete a note
    @app.route('/api/notes/<int:note_id>', methods=['DELETE'])
    def delete_note(note_id):
        """Delete a note"""
        # Find the note or return 404 if not found
        note = Note.query.get_or_404(note_id)
        
        # Remove from database
        db.session.delete(note)
        db.session.commit()
        
        # Return success message
        return jsonify({"message": "Note deleted successfully"})
```

### Understanding API Routes:

Let's break down each endpoint:

1. **GET /api/notes**
   - Returns all notes
   - Supports filtering by archive status
   - Orders by pinned status and update time

2. **GET /api/notes/<id>**
   - Returns a specific note by ID
   - Returns 404 if the note doesn't exist

3. **POST /api/notes**
   - Creates a new note
   - Takes JSON data with title, content, etc.
   - Returns the created note and a 201 status code

4. **PUT /api/notes/<id>**
   - Updates an existing note
   - Takes JSON data with fields to update
   - Updates only the provided fields

5. **DELETE /api/notes/<id>**
   - Deletes a note
   - Returns a success message

### Understanding HTTP Methods:

- **GET**: Requests data (safe, doesn't change data)
- **POST**: Creates new data
- **PUT**: Updates existing data (replaces entire resource)
- **DELETE**: Removes data

## Step 8: Creating the Run Script

Create a script to run our application. Edit `run.py`:

```python
from app import create_app

# Create the application
app = create_app()

# Run the application if this file is executed directly
if __name__ == '__main__':
    app.run(debug=True)
```

### Understanding This Script:

- We import the `create_app` function we defined
- We create our Flask application
- We run the application in debug mode (which shows helpful errors and automatically reloads when code changes)

## Step 9: Running Your API

Now, let's start our Flask API server:

```bash
python run.py
```

You should see output like:

```
 * Serving Flask app '...'
 * Debug mode: on
 * Running on http://127.0.0.1:5000
```

Your Google Keep clone API is now running locally!

## Step 10: Testing Your API

Let's test our API to make sure it works. We can use tools like:

- **cURL**: A command-line tool for sending HTTP requests
- **Postman**: A GUI application for API testing
- **Browser DevTools**: For simple GET requests

### Examples using cURL:

#### Creating a Note:

```bash
curl -X POST http://127.0.0.1:5000/api/notes \
  -H "Content-Type: application/json" \
  -d '{"title": "Shopping List", "content": "Milk\nEggs\nBread", "color": "yellow"}'
```

#### Getting All Notes:

```bash
curl http://127.0.0.1:5000/api/notes
```

#### Updating a Note:

```bash
curl -X PUT http://127.0.0.1:5000/api/notes/1 \
  -H "Content-Type: application/json" \
  -d '{"is_pinned": true}'
```

#### Deleting a Note:

```bash
curl -X DELETE http://127.0.0.1:5000/api/notes/1
```

## Key Concepts to Understand

### API Endpoints

API endpoints are specific URLs that clients can access to perform operations. In our API:

- `/api/notes` is an endpoint for working with all notes
- `/api/notes/1` is an endpoint for working with the note with ID 1

### JSON

JSON (JavaScript Object Notation) is a standard format for exchanging data in web APIs:

```json
{
  "id": 1,
  "title": "Shopping List",
  "content": "Milk\nEggs\nBread",
  "color": "yellow",
  "is_pinned": false,
  "is_archived": false
}
```

### Status Codes

HTTP status codes indicate the result of a request:

- **200 OK**: Request succeeded
- **201 Created**: Resource was successfully created
- **400 Bad Request**: Invalid input
- **404 Not Found**: Resource not found
- **500 Internal Server Error**: Server error

### Database Operations

Our API performs CRUD (Create, Read, Update, Delete) operations:

- **Create**: Adding new notes (POST)
- **Read**: Retrieving notes (GET)
- **Update**: Modifying notes (PUT)
- **Delete**: Removing notes (DELETE)

## Next Steps and Further Learning

Now that you've created your first backend API, here are some ways to expand your knowledge:

### Improve This API:

1. **Add User Authentication**: Allow users to register, log in, and have their own notes
2. **Add Note Categories or Tags**: Allow organizing notes with labels
3. **Add Search Functionality**: Allow searching through notes
4. **Add Rich Text Support**: Support formatting in notes
5. **Add Error Handling**: Improve how errors are handled and returned

### Learn More About:

1. **Database Design**: Learn about relationships, indexes, and optimizations
2. **Authentication & Authorization**: Learn about JWT, OAuth, etc.
3. **API Documentation**: Learn about tools like Swagger/OpenAPI
4. **Testing**: Learn how to write tests for your API
5. **Deployment**: Learn how to deploy your API to production

## Troubleshooting Common Issues

### Database Issues:

If you can't access or modify the database:
- Check that the `instance` directory exists and is writable
- Try deleting the database file (`instance/notes.db`) and restart the app

### API Request Issues:

If your API requests aren't working:
- Make sure you're using the correct URL and HTTP method
- Check that your JSON is valid
- Check that you're providing all required fields

### Flask App Issues:

If your Flask app won't start:
- Make sure you've activated your virtual environment
- Check for error messages in the console
- Verify all required packages are installed

## Conclusion

Congratulations! You've created your first backend API using Flask, UV, and SQLite3. This Google Keep clone API demonstrates fundamental concepts of backend development and RESTful API design.

Remember that this is just the beginning - there is more to learn, so let's keep exploring!
