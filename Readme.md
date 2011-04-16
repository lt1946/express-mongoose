
# Express-goose

 Express-goose adds Mongoose Query and Promise support to Express render, partial, and send methods.

## Installation

    $ npm install express-goose

### Example

    In your models:

    User.methods.getLikes = function (callback) {
      // returns a Query
      return this.model('Likes').find({ _user: this._id }, callback);
    };

    News.statics.getLatest = function (callback) {
      var promise = new Promise;
      if (callback) promise.addBack(callback);
      this.find({ datePublished: { $gt: new Date(Date.now() - 60000*60) } }, promise.resolve.bind(promise));
      return promise;
    };

    In your routes:

    app.get('/dashboard', function (req, res) {
      var News = db.model('News');

      // render support
      res.render('dashboard', {
          likes: req.user.getLikes()
        , latestNews: News.getLatest()
      });
    });

    app.get('/partial', function (req, res, next) {
      res.partial('likes', {
          likes: req.user.getLikes()
      });
    });

    app.get('/send', function (req, res) {
      // just pass a Query or Promise in and the result will be rendered as json.
      // if an error occurs, the error will be next()ed along as expected.
      var News = db.model('News');
      res.send(News.getLatest());
    });

    app.get('/promises', function (req, res) {
      var promise = new Promise(somethingAsync);
      res.send(promise);
    });

### Features

 res.render support
 res.partial support
 res.send support

### Error handling

 If a Query or Promise resolves to an error it will be forwarded on with `next(err)` as expected.

## Authors

  - [Aaron Heckmann (aheckmann)](http://github.com/aheckmann)

## More Information

## License

(The MIT License)

Copyright (c) 2011 LearnBoost &lt;dev@learnboost.com&gt;

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
