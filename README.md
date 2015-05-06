# passport-google-oauth-jwt

[Passport](http://passportjs.org/) strategy for authentication with [Google](http://www.google.com/), meet [Migrating to Google Sign-In Guide](https://developers.google.com/identity/sign-in/auth-migration).

The strategy will get email of signed-in account by parsing JWT returned from Google OAuth. It does not get full Google profile, but it does not require [Google + API](https://developers.google.com/+/api/auth-migration#sign-in) enabled in [Google Developers Console](https://console.developers.google.com/).

If you want to get a full Google profile, please consider using [Passport-Google-OAuth](https://github.com/jaredhanson/passport-google-oauth).

## Install
	$ npm install passport-google-oauth-jwt

## Usage

#### Configurate Strategy

```Javascript
var GoogleStrategy = require('passport-google-oauth-jwt').GoogleOauthJWTStrategy;

passport.use(new GoogleStrategy({
	clientId: GOOGLE_CLIENT_ID,
	clientSecret: GOOGLE_CLIENT_SECRET
}, function verify(accessToken, loginInfo, refreshToken, done) {
	User.findOrCreate({
		googleEmail: loginInfo.email
	}, function (err, user) {
		return done(err, user);
	});
}));
```

#### Authentication Requests

Use `passport.authentication()`, specifying the `'google-oauth-jwt'` strategy, to authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/) application:

```Javascript
app.get('/auth/google', passport.authenticate('google-oauth-jwt', {
	callbackUrl: 'http://localhost:3000/auth/google/callback',
	scope: 'email'
}));

app.get('/auth/google/callback', passport.authenticate('google-oauth-jwt', {
	callbackUrl: 'http://localhost:3000/auth/google/callback'
}), function(req, res) {
	// Successful authentication, redirect home
	res.redirect('/');
});
```

## Examples

For a complete, working example, refer to the [example](https://github.com/longlh/passport-google-oauth-jwt/tree/master/examples).

	$ node app.js

## License

[The MIT License](http://opensource.org/licenses/MIT)
