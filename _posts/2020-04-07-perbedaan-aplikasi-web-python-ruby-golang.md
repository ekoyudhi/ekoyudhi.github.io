---
layout: post
title: Perbedaan Aplikasi Web Python, Ruby dan Golang
categories: Programming
---

Teknologi web saat ini berkembang sangat cepat. Seperti yang biasa diketahui kebanyakan orang bahwa mengembangkan aplikasi web biasanya menggunakan bahasa pemrograman HTML (HyperText Markup Languange) yang dipadukan dengan CSS (Cascading Style Sheet) dan juga Javascript. Semua teknologi ini dijalankan pada sisi client dalam hal ini adalah browser. Untuk sisi server umumnya mengggunakan bahasa pemrograman PHP (PHP Hypertext Processor) dengan mesin basis data MySQL maupun PostgreSQL. Rata-rata penyedia layanan hosting web menggunakan platform ini. 

Selain HTML dan PHP, terdapat beberapa aplikasi web atau istilahnya disebut Web Framework yang ditulis menggunakan beberapa bahasa pemrograman lainnya yang mungkin jarang kita dengar. Diantaranya Flask yang merupakan Web Framework untuk bahasa pemrograman Python. Sinatra untuk bahasa pemrograman Ruby dan Martini untuk bahasa pemrograman Go atau Golang. Berikut ini beberapa perbedaan dari ketiga aplikasi web / Web Framework tersebut.

### Pustaka
#### Python (Flask)

> Flask adalah _framework_ mikro untuk Python berdasarkan Werkzeug, Jinja2 dan niat baik

Untuk aplikasi yang sangat sederhana, seperti yang ditunjukkan dalam demo ini, Flask adalah pilihan yang bagus. Aplikasi Flask dasar hanya terdiri dari 7 baris kode dalam satu file Python. Yang menarik dari Flask di atas pustaka web Python lainnya (seperti [Django](https://www.djangoproject.com/) atau [Pyramid](http://www.pylonsproject.org/)) adalah dapat dimulai dari yang kecil dan membangun aplikasi yang lebih kompleks sesuai kebutuhan.

```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
    return "Hello World!"

if __name__ == "__main__":
    app.run()
```

#### Ruby(Sinatra)

> Sinatra adalah bahasa untuk membuat aplikasi web dengan cepat di Ruby dengan upaya minimal.

Sama seperti Flask, Sinatra sangat bagus untuk aplikasi sederhana. Aplikasi dasar Sinatra hanya 4 baris kode dalam satu file Ruby. Sinatra digunakan sebagai pengganti perpustakaan seperti Ruby on Rails untuk alasan yang sama seperti Flask - Anda dapat memulai dari yang kecil dan memperluas aplikasi sesuai kebutuhan.


```ruby
require 'sinatra'

get '/hi' do
  "Hello World!"
end
```

#### Golang(Martini)

> Martini adalah paket ampuh untuk menulis aplikasi / layanan web modular dengan cepat di Golang.

Martini hadir dengan beberapa baterai lebih banyak daripada Sinatra dan Flask, tetapi masih sangat ringan untuk memulainya - hanya 9 baris kode untuk aplikasi dasar. Martini telah mendapat kritik dari komunitas Golang tetapi masih memiliki salah satu proyek Github berperingkat tertinggi dari semua kerangka web Golang. Penulis Martini menanggapi kritik langsung di sini. Beberapa kerangka kerja lainnya termasuk Revel, Gin, dan bahkan pustaka net/http bawaan.

```golang
package main

import "github.com/go-martini/martini"

func main() {
  m := martini.Classic()
  m.Get("/", func() string {
    return "Hello world!"
  })
  m.Run()
}
```
## Struktur Aplikasi
### Inisialiasi dan Menjalankan Aplikasi
#### Python (Flask) - _app_._py_
```python
from flask import Flask
app = Flask(__name__)

# run application
if __name__ == '__main__':
    app.run(host='0.0.0.0')
```
```shell
$ python app.py
```
#### Ruby (Sinatra) - _app_._rb_
```ruby
# initialize application
require 'sinatra'
```
```shell
$ ruby app.rb
```
#### Golang (Martini) - _app_._go_
```go
package main
import "github.com/go-martini/martini"
import "github.com/martini-contrib/render"

func main() {
    app := martini.Classic()
    app.Use(render.Renderer())

    // run application
    app.Run()
}
```
```shell
$ go run app.go
```

### Mendefinisikan _Route_ (GET/POST)
#### Python (Flask)
```python
@app.route('/')  # the default is GET only
def blog():
    # ...

#post
@app.route('/new', methods=['POST'])
def new():
    # ...
```
#### Ruby (Sinatra)
```ruby
# get
get '/' do
  # ...
end

# post
post '/new' do
  # ...
end
```
#### Golang (Martini)
```go
type Post struct {
  Title   string `form:"title" json:"title"`
  Summary string `form:"summary" json:"summary"`
  Content string `form:"content" json:"content"`
}

// get
app.Get("/", func(r render.Render) {
  // ...
}

// post
import "github.com/martini-contrib/binding"
app.Post("/new", binding.Bind(Post{}), func(r render.Render, post Post) {
  // ...
}
```
### _Render_ template JSON
#### Python (Flask)
Flask menyediakan metode [jsonify()](http://flask.pocoo.org/docs/0.10/api/#flask.json.jsonify) tetapi karena layanan menggunakan MongoDB, utilitas mongodb bson digunakan.
```python
from bson.json_util import dumps
return dumps(posts) # posts is a list of dicts [{}, {}]
```
#### Ruby (Sinatra)
```ruby
require 'json'
content_type :json
posts.to_json # posts is an array (from mongodb)
```
#### Golang (Martini)
```go
r.JSON(200, posts) // posts is an array of Post{} structs
```
### _Render_ template HTML
#### Python (Flask)
```python
return render_template('blog.html', posts=posts)
```

```html
<!doctype HTML>
<html>
  <head>
    <title>Python Flask Example</title>
  </head>
  <body>
    {% for post in posts %}
      <h1> {{ post.title }} </h1>
      <h3> {{ post.summary }} </h3>
      <p> {{ post.content }} </p>
      <hr>
    {% endfor %}
  </body>
</html>
```
#### Ruby (Sinatra)
```ruby
erb :blog
```

```html
<!doctype HTML>
<html>
  <head>
    <title>Ruby Sinatra Example</title>
  </head>
  <body>
    <% @posts.each do |post| %>
      <h1><%= post['title'] %></h1>
      <h3><%= post['summary'] %></h3>
      <p><%= post['content'] %></p>
      <hr>
    <% end %>
  </body>
</html>
```
### Golang (Martini)
```go
r.HTML(200, "blog", posts)
```

```html
<!doctype HTML>
<html>
  <head>
    <title>Golang Martini Example</title>
  </head>
  <body>
    {{range . }}
      <h1>{{.Title}}</h1>
      <h3>{{.Summary}}</h3>
      <p>{{.Content}}</p>
      <hr>
    {{ end }}
  </body>
</html>
```
### Koneksi Basis Data
Semua aplikasi menggunakan driver mongodb khusus untuk bahasa pemgrograman di atas. Variabel DB_PORT_27017_TCP_ADDR adalah IP dari tautan kontainer _docker_.
#### Python (Flask)
```python
from pymongo import MongoClient
client = MongoClient(os.environ['DB_PORT_27017_TCP_ADDR'], 27017)
db = client.blog
```
#### Ruby (Sinatra)
```ruby
require 'mongo'
db_ip = [ENV['DB_PORT_27017_TCP_ADDR']]
client = Mongo::Client.new(db_ip, database: 'blog')
```
#### Golang (Martini)
```go
import "gopkg.in/mgo.v2"
session, _ := mgo.Dial(os.Getenv("DB_PORT_27017_TCP_ADDR"))
db := session.DB("blog")
defer session.Close()
```
### Menyisipkan Data Menggunakan Metode POST
#### Python (Flask)
```python
from flask import request
post = {
    'title': request.form['title'],
    'summary': request.form['summary'],
    'content': request.form['content']
}
db.blog.insert_one(post)
```
#### Ruby (Sinatra)
```ruby
client[:posts].insert_one(params) # params is a hash generated by sinatra
```
#### Golang (Martini)
```go
db.C("posts").Insert(post) // post is an instance of the Post{} struct
```
### Menerima Data
#### Python (Flask)
```python
posts = db.blog.find()
```
#### Ruby (Sinatra)
```ruby
@posts = client[:posts].find.to_a
```
#### Golang (Martini)
```go
ar posts []Post
db.C("posts").Find(nil).All(&posts)
```
### _Deployment_ Aplikasi
Solusi hebat untuk _deploy_ semua aplikasi ini adalah dengan menggunakan [docker](https://www.docker.com/) dan [docker-compose](https://docs.docker.com/compose/).
#### Python (Flask)
##### Dockerfile
```dockerfile
FROM python:3.4

ADD . /app
WORKDIR /app

RUN pip install -r requirements.txt
```
##### docker-compose.yml
```yaml
web:
  build: .
  command: python -u app.py
  ports:
    - "5000:5000"
  volumes:
    - .:/app
  links:
    - db
db:
  image: mongo:3.0.4
  command: mongod --quiet --logpath=/dev/null
```
#### Ruby (Sinatra)
##### Dockerfile
```dockerfile
FROM ruby:2.2

ADD . /app
WORKDIR /app

RUN bundle install
```
##### docker-compose.yml
```yaml
web:
  build: .
  command: bundle exec ruby app.rb
  ports:
    - "4567:4567"
  volumes:
    - .:/app
  links:
    - db
db:
  image: mongo:3.0.4
  command: mongod --quiet --logpath=/dev/null
```
#### Golang (Martini)
##### Dockerfile
```dockerfile
FROM golang:1.3

ADD . /go/src/github.com/kpurdon/go-todo
WORKDIR /go/src/github.com/kpurdon/go-todo

RUN go get github.com/go-martini/martini && 
    \ go get github.com/martini-contrib/render && 
    \ go get gopkg.in/mgo.v2 &&
    \ go get github.com/martini-contrib/binding
```
##### docker-compose.yml
```yaml
web:
  build: .
  command: go run app.go
  ports:
    - "3000:3000"
  volumes: # look into volumes v. "ADD"
    - .:/go/src/github.com/kpurdon/go-todo
  links:
    - db
db:
  image: mongo:3.0.4
  command: mongod --quiet --logpath=/dev/null
```