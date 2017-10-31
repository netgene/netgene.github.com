---
layout: post
title: flask+mysql webservice 书籍增删查
date: 2015-05-08 20:20:20
categories:
- python
tags:
- mysql
- flask
---

```
create table sdbook(id int NOT NULL AUTO_INCREMENT, name varchar(50), price float, author varchar(50), publisher varchar(50), publishtimie date, publishnum int, pagenum int, wordnum int, ISBN varchar(20), type int,memo varchar(100), createdate date, primary key(id));
```

book.py  

{% highlight python %}
#coding=utf-8
from flask import Flask, request, jsonify, make_response, abort
import MySQLdb

#curl -i http://127.0.0.1:5000/book
#curl -i http://127.0.0.1:5000/book/1
#curl -i -H "Content-Type: application/json" -X POST -d '{"name":"mybook", "price":49.8, "author":"long", "publisher":"publisher", "publishnum":1, "pagenum":353, "wordnum":335000, "isbn":"9787539981697", "type":1}' http://127.0.0.1:5000/book
#curl -i -X DELETE http://127.0.0.1:5000/book/3

conn = MySQLdb.connect(host='localhost', user='root', passwd='0709', db='shudang', charset='utf8')
cur = conn.cursor()

app = Flask(__name__)

@app.route('/', methods=['GET'])
def roots():
	cur.execute('select id,name,price,author,publisher,publishnum,pagenum,wordnum,isbn,type from sdbook')
	books = cur.fetchall()
	return jsonify({'books':books})

@app.route('/book', methods=['GET'])
def getbooks():
	cur.execute('select id,name,price,author,publisher,publishnum,pagenum,wordnum,isbn,type from sdbook')
	books = cur.fetchall()
	return jsonify({'books':books})

@app.route('/book/<int:bookid>', methods=['GET'])
def getbook(bookid):
	cur.execute('select id,name,price,author,publisher,publishnum,pagenum,wordnum,isbn,type from sdbook')
	books = cur.fetchall()
	book = filter(lambda t: t[0] == bookid, books)
	if len(book) == 0:
		abort(404)
	return jsonify({'books':book})

@app.route('/book', methods=['POST'])
def postbook():
	if not request.json:
		abort(404)
	param = (request.json['name'], request.json['price'], request.json['author'], request.json['publisher'], request.json['publishnum'], request.json['pagenum'], request.json['wordnum'], request.json['isbn'], request.json['type'])
	sql = "insert into sdbook(name, price, author, publisher, publishnum, pagenum, wordnum, isbn, type)values(%s,%s,%s,%s,%s,%s,%s,%s,%s)"
	cur.execute(sql, param)
	conn.commit()
	cur.execute('select id,name,price,author,publisher,publishnum,pagenum,wordnum,isbn,type from sdbook')
	books = cur.fetchall()
	return jsonify({'books':books}), 201

@app.route('/book/<bookid>', methods=['DELETE'])
def deletebook(bookid):
	sql = "delete from sdbook where id = %s"
	cur.execute(sql, bookid)
	conn.commit()
	cur.execute('select id,name,price,author,publisher,publishnum,pagenum,wordnum,isbn,type from sdbook')
	books = cur.fetchall()
	return jsonify({'books':books}), 201

@app.errorhandler(404)
def not_found(error):
	return make_response(jsonify({'error': 'Not found'}), 404)

if __name__ == "__main__":
	app.run(host='0.0.0.0', debug = True)
{% endhighlight %}
