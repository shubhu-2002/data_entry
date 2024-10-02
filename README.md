from flask import Flask, render_template, request
from flask_sqlalchemy import SQLAlchemy

app = Flask(__name__)
app.config["SQLALCHEMY_DATABASE_URI"] = "sqlite:///project.db"
db = SQLAlchemy(app)

class POSTS(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(30), nullable=False)
    content = db.Column(db.String(200), nullable=False)


with app.app_context():
    db.create_all()

def create_post(name, post):
    
    new_post = POSTS(name=name, content=post)
    db.session.add(new_post)
    db.session.commit()

@app.route('/', methods=['GET', 'POST'])
def index():
    if request.method == 'POST':
        
        name = request.form.get('name')
        post = request.form.get('post')
        create_post(name, post)

    posts=POSTS.query.all()
        
    
    return render_template('index.html',posts=posts)

if __name__ == '__main__':
    app.run(debug=True)
