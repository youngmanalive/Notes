## MERN

### Setup
Sign up for [mLab](https://mlab.com)

- Create new MongoDB deployment, use AWS for cloud storage.
- Name your database
- Add db user to deployment

Make new project directory in terminal
- `npm init` to set up package.json (MIT license for free use)
- `npm install express mongoose passport passport-jwt body-parser bcryptjs validator jsonwebtoken`
- `npm install -D nodemon`


#### packson.json
```JavaScript
// ...

"scripts": {
  "start": "node app.js",
  "server": "nodemon app.js" // watches for changes
}
```

#### app.js (entry file)
```JavaScript
const express = require('express');
const mongoose = require('mongoose');
const db = require('./config/keys').mongoURI;


const users = require('/routes/api/users');
const events = require('/routes/api/events');


mongoose
  .connect(db)
  .then(() => do(something))
  .catch(error => do(something));

const app = express();

app.get('/', (req, res) => )
app.get('/users', (req, res) => )
app.get('/events', (req, res) => )

app.use(bodyParser.urlencoded({extended: false}));
app.use(bodyParser.json());

const port = process.env.PORT || 5000;

app.listen(port, () => )
```

#### config/keys.js
(hold mlab secret keys)

```JavaScript
module.exports = {
  mongoURI: 'secret key from mlab!'
}
```

#### .gitignore
```
node_modlues
config/keys.js
```

### routes/api/
#### users.js

```JavaScript
const express = require('express');
const router = express.Router();

router.get('/route', (req, res) => do(something))

module.exports = router;
```

### models/
#### User.js
```JavaScript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const UserSchema = new Schema({
  name: {
    type: String,
    required: true
  },
  email: {
    type: String,
    required: true
  },
  password: {
    type: String,
    required: true
  },
  date: {
    type: Date,
    default: Date.now
  }
});

module.exports = User = mongoose.model('users', UserSchema)
```

#### Event.js
```JavaScript
const mongoose = require('mongoose');
const Schema = mongoose.Schema;

const EventSchema = new Schema({
  name: {
    type: String,
    required: true
  },
  description: {
    type: String,
    required: true
  },
  date: {
    type: Date,
    default: Date.now
  }
});

module.exports = Event = mongoose.model('users', EventSchema)
```
