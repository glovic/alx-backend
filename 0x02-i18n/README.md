# 0x02. i18n

This project involves internationalizing a Flask web application using Babel. The tasks focus on setting up Babel, localizing the application, and managing user settings for language and time zone preferences.

## Tasks :page_with_curl:

* **0. Basic Flask app**
  * **[0-app.py](./0-app.py), [templates/0-index.html](./templates/0-index.html):** Set up a basic Flask app with a single route `/`. Create an `index.html` template that displays “Welcome to Holberton” as the page title and “Hello world” as the header.
    * **Usage:**
      ```python
      from flask import Flask, render_template
      
      app = Flask(__name__)
      
      @app.route('/')
      def index():
          return render_template('0-index.html')
      
      if __name__ == "__main__":
          app.run()
      ```

* **1. Basic Babel setup**
  * **[1-app.py](./1-app.py), [templates/1-index.html](./templates/1-index.html):** Install and configure Babel for the Flask app. Create a `Config` class to define available languages and set the default locale and timezone. Instantiate Babel with the Flask app.
    * **Usage:**
      ```python
      from flask_babel import Babel
      
      class Config:
          LANGUAGES = ["en", "fr"]
          BABEL_DEFAULT_LOCALE = "en"
          BABEL_DEFAULT_TIMEZONE = "UTC"
      
      app.config.from_object(Config)
      babel = Babel(app)
      ```

* **2. Get locale from request**
  * **[2-app.py](./2-app.py), [templates/2-index.html](./templates/2-index.html):** Implement a `get_locale` function to determine the best match for supported languages from `request.accept_languages`. Use the `babel.localeselector` decorator to apply this function.
    * **Usage:**
      ```python
      from flask import request
      
      @babel.localeselector
      def get_locale():
          return request.accept_languages.best_match(app.config['LANGUAGES'])
      ```

* **3. Parametrize templates**
  * **[3-app.py](./3-app.py), [babel.cfg](./babel.cfg), [templates/3-index.html](./templates/3-index.html):** Use the `_` or `gettext` function to parametrize templates with message IDs `home_title` and `home_header`. Extract translations, initialize dictionaries, and provide translations for both English and French.
    * **Usage:**
      ```bash
      $ pybabel extract -F babel.cfg -o messages.pot .
      $ pybabel init -i messages.pot -d translations -l en
      $ pybabel init -i messages.pot -d translations -l fr
      $ pybabel compile -d translations
      ```

* **4. Force locale with URL parameter**
  * **[4-app.py](./4-app.py), [templates/4-index.html](./templates/4-index.html):** Implement logic in `get_locale` to check for a `locale` parameter in the URL. If present and valid, use this locale; otherwise, fall back to the default behavior.
    * **Usage:**
      ```python
      @babel.localeselector
      def get_locale():
          locale = request.args.get('locale')
          if locale in app.config['LANGUAGES']:
              return locale
          return request.accept_languages.best_match(app.config['LANGUAGES'])
      ```

* **5. Mock logging in**
  * **[5-app.py](./5-app.py), [templates/5-index.html](./templates/5-index.html):** Emulate user login by creating a mock user table. Implement `get_user` to return user data based on the `login_as` URL parameter. Use `before_request` to set the user globally, and display a personalized message if the user is logged in.
    * **Usage:**
      ```python
      users = {
          1: {"name": "Balou", "locale": "fr", "timezone": "Europe/Paris"},
          2: {"name": "Beyonce", "locale": "en", "timezone": "US/Central"},
          3: {"name": "Spock", "locale": "kg", "timezone": "Vulcan"},
          4: {"name": "Teletubby", "locale": None, "timezone": "Europe/London"},
      }
      
      @app.before_request
      def before_request():
          g.user = get_user()
      ```

* **6. Use user locale**
  * **[6-app.py](./6-app.py), [templates/6-index.html](./templates/6-index.html):** Modify `get_locale` to prioritize the user's preferred locale if available, falling back to the URL parameter, request headers, and finally the default locale.
    * **Usage:**
      ```python
      @babel.localeselector
      def get_locale():
          if g.user and g.user.get('locale') in app.config['LANGUAGES']:
              return g.user['locale']
          locale = request.args.get('locale')
          if locale in app.config['LANGUAGES']:
              return locale
          return request.accept_languages.best_match(app.config['LANGUAGES'])
      ```

* **7. Infer appropriate time zone**
  * **[7-app.py](./7-app.py), [templates/7-index.html](./templates/7-index.html):** Implement `get_timezone` using the `babel.timezoneselector` decorator. Validate the timezone and apply it based on URL parameters, user settings, or default to UTC.
    * **Usage:**
      ```python
      import pytz
      from pytz.exceptions import UnknownTimeZoneError
      
      @babel.timezoneselector
      def get_timezone():
          try:
              if 'timezone' in request.args:
                  return pytz.timezone(request.args['timezone']).zone
              if g.user and g.user['timezone']:
                  return pytz.timezone(g.user['timezone']).zone
          except UnknownTimeZoneError:
              return app.config['BABEL_DEFAULT_TIMEZONE']
          return app.config['BABEL_DEFAULT_TIMEZONE']
      ```

* **8. Display the current time**
  * **[app.py](./app.py), [templates/index.html](./templates/index.html), [translations/en/LC_MESSAGES/messages.po](./translations/en/LC_MESSAGES/messages.po), [translations/fr/LC_MESSAGES/messages.po](./translations/fr/LC_MESSAGES/messages.po):** Based on the inferred time zone, display the current time on the home page in the default format. Use the appropriate translations for displaying the time in English and French.
  * **Usage:**
    ```bash
    $ pybabel compile -d translations
    $ python3 app.py
    ```
