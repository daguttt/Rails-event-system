# Event System

The Event System is a Ruby on Rails application designed to manage events. It allows you to create, store, and retrieve events with attributes such as name, date, and description. Additionally, the system includes features like custom scopes for filtering events by date ranges and callbacks to manage event data more effectively.

This README provides step-by-step instructions to set up the application, configure the necessary gems and environment variables, and use custom scopes and callbacks to enrich the functionality of the app.

# Step 1

Setup Instructions
Follow the steps below to set up and run the Event System on your local machine.

1. Create a New Rails Application
Open your terminal and run the following command to create a new Rails application with PostgreSQL as the database:

```bash
rails new event-system --database=postgresql
```
This will create a new Rails application named event-system and configure it to use PostgreSQL as the database.

2. Add the dotenv Gem
To manage environment variables, we will use the dotenv gem. This will allow us to securely store sensitive information such as database credentials.

Open the Gemfile and add the following line:
ruby
```bash
gem 'dotenv-rails'
```

Run the following command to install the gem:
```bash
bundle install
```

3. Set Up Environment Variables
Create a .env file in the root directory of the project:
```bash
touch .env
```
Add your PostgreSQL database credentials (or any other sensitive information) in the .env file. For example:
```bash
DATABASE_USERNAME=your_db_username
DATABASE_PASSWORD=your_db_password
```


4. Configure Database Settings
Now, update the config/database.yml file to use the environment variables you just added. Replace the database username and password.

5. Create the Database
Run the following command to create the database:

```bash
rake db:create
```

6. Generate the Event Scaffold (model , view, controller)
To generate the Event model, controller, and views, run the following command:

```bash
rails generate scaffold Event name:string date:date description:text
```
This will create all the necessary files for the Event resource, including migration files, model, controller, and views.

7. Run Migrations
Apply the generated migration to create the events table in the database:

```bash
rake db:migrate
```

8. Set the Root Path
To configure the root of the application to display the events page, edit the config/routes.rb file and set the root route as follows:

```bash
root 'events#index'
```

9. Add Custom Scopes in the Model
In the Event model (app/models/event.rb), add custom scopes to filter events based on specific date ranges. For example, to get events created in the last 30 days:

10. Implement Callbacks for Custom Functionality
Add custom callbacks in the Event model to handle specific logic. For example, you can add a before_save callback to adjust the event date before saving the record:

11. Create a Controller Object with Scopes
In the EventsController, create a controller action to use the defined scopes and send the filtered events to the view:

12. Consume the Scopes in the View
In the app/views/events/index.html.erb file, display the filtered events.


# Step 2 Devise Installation

Add an authentication layer with the Devise gem https://rubygems.org/gems/devise
Add the gem to the Gemfile
```bash
  gem 'devise', '~> 4.9', '>= 4.9.4'
```
In the console, install the new gem:
```bash
  bundle install
```
Follow the gem installation configuration:
```bash
  rails generate devise:install
```
The previous command will display the required steps in the console to continue with the gem installation. It should show something like the following:
In config/environments/development.rb, add:
 ```bash
   config.action_mailer.default_url_options = { host: 'localhost', port: 3000 }
```
Specify the model Devise will use. This command will create a new migration to generate a new table for this model:
```bash
   rails generate devise MODEL
```
In the above command, replace MODEL with the model name you want to use, e.g., User.

Go into the migration generated by scaffold and configure any necessary additional fields, like confirmable and lockable.
Create another migration to associate the events and user models:
```bash
  rails generate migration AddUserToEvents user:references
```

Within the reference migration, add something like this:
```bash
   add_reference :events, :user, null: false, foreign_key: true
```

Next, run the migration so that it reflects in the schema:
```bash
  rails db:migrate
```
In the events controller, add a callback to restrict actions. Allow all users to view index and show views, but only allow authenticated users to create, edit, and delete events.
```bash
  before_action :authenticate_user!, except: [:index, :show]
```

Add the necessary logic to store the currently signed-in user as the creator of an event when it is created:

```bash
    @event = current_user.events.build(event_params)
```

# Step 3 Testing


1. Add required gems to your Gemfile

```bash
    # rspec-rails integrates the Rails testing helpers into RSpec.
    gem 'rspec-rails', '~> 7.1'
    # factory_bot provides a framework and DSL for defining and using factories - less error-prone, more explicit, and all-around easier to work with than fixtures
    gem 'factory_bot_rails', '~> 6.4', '>= 6.4.4'
    gem 'rails-controller-testing', '~> 1.0', '>= 1.0.5'
    # Faker, a port of Data::Faker from Perl, is used to easily generate fake data: names, addresses, phone numbers, etc.
    gem 'faker', '~> 3.5', '>= 3.5.1'
    # Code coverage for Ruby with a UI
    gem 'simplecov', '~> 0.22.0'
    # autenticacion
    gem 'devise', '~> 4.9', '>= 4.9.4'
    # pruebas de integración o pruebas de características (feature tests)
    gem "capybara"
    gem "selenium-webdriver"
```
2. Run bundle install
```bash
    bundle install
```
3. Install RSpec

```bash
    rails generate rspec:install
```
4. Configure rails_helper.rb for Additional Testing Tools
Edit rails_helper.rb to configure RSpec with FactoryBot, Faker, Devise, and SimpleCov.

5. Create Testing Directories
Ensure directories like spec/models, spec/controllers, and spec/factories are present. You can organize factories under spec/factories to keep them organized.

# Step 4 Tailwind CSS


1. Add required gems to your Gemfile
In your `Gemfile`, add the `tailwindcss-rails` gem:

```bash
gem "tailwindcss-rails"
```
Then, execute:

```bash
bundle install
```
2. Installing Tailwind CSS in the project
After adding the gem, you must run a command to install Tailwind CSS in your Rails project:
```bash
rails tailwindcss:install
```
This command does the following:
- Creates a `tailwind.config.js` file in the root of your project, where you can customize the Tailwind configuration.
- Adds an initial CSS file for Tailwind (`app/assets/stylesheets/application.tailwind.css`).
- Update the necessary files to make Tailwind CSS ready for use.
- That is, you should check that the files mentioned above are created as expected.


3. Update the CSS file to include Tailwind
On installation, a file named `application.tailwind.css` will be generated in the `app/assets/stylesheets/` folder. This file already includes the necessary directives for Tailwind, such as:

```bash
@import "tailwindcss/base";
@import "tailwindcss/components";
@import "tailwindcss/utilities";
```
This file will take care of importing all Tailwind styles.

4. Configure your layout to use Tailwind.
You may need to make sure that your application is using the CSS generated by Tailwind. Open the `app/views/layouts/application.html.erb` file and verify that it is using the correct CSS file:
**Note**: Make sure the line `<%= stylesheet_link_tag “application”, “data-turbo-track”: “reload” %>` is present. This will take care of loading the Tailwind CSS into the application.

5. Optional Tailwind customization
Tailwind CSS includes a configuration file (`tailwind.config.js`) that allows you to customize colors, spacing, and other settings. You can edit this file to suit your needs.
For example, open `tailwind.config.js`:

```js
module.exports = {
  content: [
    './app/**/*.html.erb',
    './app/helpers/**/*.rb',
    './app/javascript/**/*.js',
    './app/views/**/*'
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```
Make sure `content` has the proper paths to scan and purge unused CSS.

6. Start the server and verify.
Finally, start your Rails server:

```bash
./bin/dev
```
You should now be able to use Tailwind classes in your views. You can test it by adding a class like `bg-blue-500` in one of your views to see if it works:

### ¿Qué es ./bin/dev? **

./bin/dev is a script provided by the foreman gem (or overmind, depending on your configuration) that allows you to run multiple processes in parallel when developing your Rails application. These processes include:

The Rails server (which was started by rails s).
JavaScript/CSS processors such as esbuild, webpack, or Vite.
Other services needed for your local environment, such as Redis or Sidekiq.

The bin/dev file is usually configured to run these processes simultaneously using a file called Procfile.dev.

### Common Troubleshooting**
1. Problems with Turbo and Tailwind: Turbo can sometimes cause problems with CSS reloading. You can try disabling Turbo for some sections if you find that the CSS does not apply as expected.
2. Compiling the CSS: Make sure that the generated CSS file is being compiled correctly. If you make changes to the `tailwind.config.js` file, you may need to restart the Rails server for the changes to be reflected.
Concerns
3. PurgeCSS: Tailwind purges unused styles to keep the final CSS lightweight. Make sure your classes are within the paths specified in `tailwind.config.js`.

### Summary 🚀
- You added `tailwindcss-rails` to the `Gemfile`.
- You ran `rails tailwindcss:install` to install Tailwind.
- You verified that the `application.html.erb` file includes the reference to `application.css`.
- You can customize Tailwind in the `tailwind.config.js` file.


# Step 5: Implementing User Roles
In this step, you will learn how to add and use roles in your Rails application using a `permission_level` field in the `users` table. This field will allow you to distinguish between normal users and administrators.

1. Add the `permission_level` field.

Create migration
Generate a migration to add the `permission_level` field to the `users` table:
```bash
rails generate migration AddPermissionLevelToUsers permission_level:integer
```
Modify migration
Edit the generated migration to set a default value so that the field does not accept null values:

```bash
class AddPermissionLevelToUsers < ActiveRecord::Migration[7.0]
  def change
    add_column :users, :permission_level, :integer, default: 0, null: false
  end
end
```
Execute migration
Applies the migration to update the database:

```bash
rails db:migrate
```
2. Create the logic in the model
In the `User` model, add methods that define the logic for checking roles:

```bash
class User < ApplicationRecord
  # Devuelve true si el usuario es normal (permission_level = 0)
  def is_normal_user?
    self.permission_level == 0
  end

  # Devuelve true si el usuario es administrador (permission_level = 1)
  def is_admin?
    self.permission_level == 1
  end
end
```
3. Crear un helper para reutilizar la lógica
Para evitar lógica repetida en las vistas, crea un helper en `app/helpers/application_helper.rb`:

```bash
module ApplicationHelper
  def admin_user?
    current_user&.is_admin?
  end
end
```
This helper checks if the current user is an administrator, handling also the case where `current_user` is `nil`.

4. Using the helper in the views.
Now you can use the `admin_user` method in any view to customize the content according to the user's role. For example, in the `app/views/partials/_navbar.html.erb` view:

```bash
<% if admin_user? %>
  <li>
    <%= link_to 'Admin', admin_path, class: 'hover:text-gray-300 text-lg font-semibold' %>
  </li>
<% end %>
```
## **Expected result**

With this implementation:
- The `permission_level` field is used to define user roles.
- The `is_normal_user` and `is_admin` methods allow to check roles easily.
- The `admin_user` helper encapsulates the logic for use in views in a clean and reusable way.

You can now extend this functionality as needed to add more roles or restrictions.

# **Step 6: Modular Monolith with Internal API**
In this step, we will implement an internal endpoint inside our monolith to handle specific requests related to events and their ticketing capability. This endpoint will be located in an Api::V1 namespace and will allow communication with other parts of the application or external services.

1. Create the endpoint structure 
Generate the API driver
Run the following command to generate the controller inside the Api::V1 namespace:

```bash
rails generate controller Api::V1/Events create
```
This will create the app/controllers/api/v1/events_controller.rb file and update the routes.

2. Configure the routes 
In the config/routes.rb file, add the following path for the POST endpoint /api/v1/tickets:

```bash

namespace :api do
  namespace :v1 do
    resources :events, only: [:create]
  end
end

```

3. Implement the controller logic 
Open the app/controllers/api/v1/events_controller.rb file and add the necessary logic to handle the request and process it:
include in event_params the fields

4. Create the service to handle the external API 
Create a file for the service in app/services/event_creation_service.rb that will handle communication with the external API.
In the config/routes.rb file, add the following path for the POST endpoint /api/v1/tickets:

```bash
class User < ApplicationRecord
namespace :api do
  namespace :v1 do
    resources :tickets, only: [:create]
  end
end
```

5. Updating the Event model 
Include the ticket_quantity and capacity fields in the model and make sure the validations are correct. Also, use a callback after_create to send the information automatically to the endpoint.
In app/models/event.rb:

6. Test the flow 
Create an event from the GUI or console. Be sure to include tickets_quantity and capacity in the form or in the parameters when creating the event.
Check the logs. Verify that the request to the endpoint is done correctly and that the response is handled without errors.

# **Step 7: API Documentation with OasRails**

In this step, we will integrate automatic API documentation for our Rails project using the **OasRails** gem. This documentation will dynamically generate an OpenAPI Specification (OAS) document for all the API endpoints, making it easier for developers to understand and interact with your API.

---

1. Add the OasRails Gem

Add the gem to your `Gemfile` under the default group:

```ruby
gem 'oas_rails'
```

Then install it by running:

```bash
bundle install
```

---

2. Mount the OasRails Engine

To expose the API documentation, you need to mount the OasRails engine in your `config/routes.rb`. Add the following line:

```ruby
mount OasRails::Engine => '/docs'
```

This will make the documentation available at `http://localhost:3000/docs`.

---

3. Configure OasRails

Generate the configuration file by running:

```bash
rails generate oas_rails:config
```

This will create a new initializer file in `config/initializers/oas_rails.rb`. Open this file and customize the basic information about your API, such as title, description, and contact details:

```ruby
OasRails.configure do |config|
  config.info.title = "Ticketing API"
  config.info.description = "API documentation for ticketing capabilities within the application."
  config.info.contact.name = "Sebastián Agudelo"
  config.info.contact.email = "support@example.com"
  config.servers = [
    { url: "http://localhost:3000", description: "Local Development Server" },
    { url: "https://api.example.com", description: "Production Server" }
  ]
end
```

---

4. Document Your Endpoints

OasRails uses YARD-style comments to enrich the generated documentation. Open the `app/controllers/api/v1/tickets_controller.rb` file and add documentation tags to the `create` action:

```ruby
class Api::V1::TicketsController < ApplicationController
  # @summary Creates tickets for an event.
  # @parameter event_id(path) [Integer] The ID of the event.
  # @parameter tickets_quantity(body) [Integer] The number of tickets to be created.
  # @response Successful creation(200) [Hash{message: String, data: Hash}]
  # @response Event not found(404) [Hash{error: String}]
  # @response Unprocessable entity(422) [Hash{error: String}]
  def create
    event = Event.find(params[:event_id])
    return render json: { error: "Event not found" }, status: :not_found if event.nil?

    payload = {
      event_id: event.id,
      user_id: event.user_id,
      tickets_quantity: params[:tickets_quantity],
      capacity: event.capacity
    }

    response = TicketCreationService.new(payload).call

    if response[:success]
      event.update(tickets_quantity: params[:tickets_quantity])
      render json: { message: "Tickets successfully created", data: response[:data] }, status: :ok
    else
      render json: { error: response[:error] }, status: :unprocessable_entity
    end
  end
end
```

---

5. Start Your Server and Access the Documentation

Start your Rails server:

```bash
rails server
```

Visit `http://localhost:3000/docs` to see the dynamically generated API documentation.

---

6. Secure the Documentation (Optional)

If you want to restrict access to the documentation, you can use Devise or any authentication mechanism. For example, restrict it to admin users:

```ruby
authenticate :user, ->(user) { user.admin? } do
  mount OasRails::Engine => '/docs'
end
```

---

7. Testing the Documentation

1. Visit the `/docs` endpoint to ensure the documentation is visible.
2. Verify that all endpoints are correctly documented with parameters, responses, and descriptions.
3. Test the functionality of the interactive documentation by making sample requests using the UI.

---

Final Notes

Integrating **OasRails** in your project ensures that your API is well-documented and developer-friendly. The dynamic generation of OpenAPI specs makes it easier to maintain and keeps the documentation up-to-date with minimal effort.


# **Step 8: Jobs - Sidekiq - Redis**

1. **Add the Gems**

Add the required gems for job scheduling and Redis to your `Gemfile`:

```ruby
# Jobs and scheduling
gem 'sidekiq', '~> 7.2', '>= 7.2.4'
gem 'sidekiq-scheduler', '~> 5.0', '>= 5.0.3'
gem 'redis', '~> 4.0'
```

Then install them by running:

```bash
bundle install
```

---

2. **Configure Sidekiq**

Create a new initializer file for Sidekiq to set up the configuration.

**File:** `config/initializers/sidekiq.rb`

```ruby
require 'sidekiq-scheduler'

Sidekiq.configure_server do |config|
  # Load the schedule file when Sidekiq starts
  config.on(:startup) do
    Sidekiq.schedule = YAML.load_file(File.expand_path('../../sidekiq_schedule.yml', __FILE__))
    Sidekiq::Scheduler.reload_schedule!
  end
end

Sidekiq.configure_client do |config|
  # Client configuration can go here (optional)
end
```

---

3. **Set Up Redis**

Ensure Redis is installed and running on your machine or server. You can install Redis on Ubuntu with:

```bash
sudo apt update
sudo apt install redis
```

Start Redis:

```bash
sudo service redis start
```

in some cases it is not necessary to run redis manually verify if once installed it is running in the background with the following command
Verify Redis is running:
```bash
redis-cli ping
```

4. **Create the Sidekiq Schedule**

Create a YAML file to define the schedule for recurring jobs.

**File:** `config/sidekiq_schedule.yml`

```yaml
# Configuration for recurring jobs in Sidekiq
daily_events_report:
  cron: "0 0 * * *" # Runs every day at 12:00 AM
  class: "DailyEventsReportJob"
  queue: "default"
  description: "Send a daily report of events created to admin users."
```

This file tells Sidekiq to run the `DailyEventsReportJob` every day at midnight.

---

5. **Create the Job**

Define the job that will be triggered by Sidekiq.

**File:** `app/jobs/daily_events_report_job.rb`

```ruby
class DailyEventsReportJob < ApplicationJob
  # Use the default queue
  queue_as :default

  def perform
    # Find events created today
    today_events = Event.where(created_at: Date.today.all_day)

    # Find admin users (permission_level: 1)
    admins = User.where(permission_level: 1)

    # If there are events and admins, send the report
    if today_events.any? && admins.any?
      admins.each do |admin|
        EventMailer.daily_events_report(admin, today_events).deliver_now
      end
    else
      Rails.logger.info "No events or admins to notify for the day #{Date.today}."
    end
  end
end
```

---

6. **Create the Mailer**

Define the mailer to format and send the emails.

**File:** `app/mailers/event_mailer.rb`

```ruby
class EventMailer < ApplicationMailer
  def daily_events_report(admin, events)
    @admin = admin
    @events = events
    mail(to: @admin.email, subject: "Daily Events Report - #{Date.today}")
  end
end
```

---

7. **Design the Email Template**

Create an HTML template for the email.

**File:** `app/views/event_mailer/daily_events_report.html.erb`

8. **Run Sidekiq**

Start the Sidekiq server to process jobs:

```bash
bundle exec sidekiq
```

---

9. **Test the Job**

To manually test the job, open the Rails console and run:

```ruby
DailyEventsReportJob.perform_now
```

This will execute the job and send the email to admin users for today’s events.

10. **Summary**

With these steps, you’ve successfully:
- Set up Sidekiq and Redis for background job processing.
- Created a recurring job to send daily event reports to admins.
- Designed and implemented a clean email template for the report.

This is a scalable and efficient solution for recurring background tasks in Rails. 🚀


# **Step 9: Configure I18n in Rails**

1. **Verify Rails' Built-in I18n Support**

Rails 7 comes with built-in support for internationalization (I18n). Ensure your application is ready to use it by adding or verifying the following configuration in `config/application.rb`:

```ruby
module YourAppName
  class Application < Rails::Application
    config.i18n.default_locale = :es
    config.i18n.available_locales = [:en, :es, :ja]
    config.i18n.load_path += Dir[Rails.root.join('config', 'locales', '**', '*.{rb,yml}')]
  end
end
```

2. **Add Locale Files**

Create YAML files in the `config/locales` directory for each language you want to support. For example:

- `config/locales/en.yml`
- `config/locales/es.yml`
- `config/locales/ja.yml`

Here’s an example structure for `es.yml`:
```yaml
es:
  events:
    title: "Eventos"
    attributes:
      name: "Nombre"
      date: "Fecha"
      description: "Descripción"
```

Repeat for the other languages with appropriate translations.

---

3. **Update Views to Use I18n**

Replace hardcoded text with calls to the I18n helper `t` (short for translate). Example:

### Original Code:
```erb
<h1>Events</h1>
```

### Updated Code:
```erb
<h1><%= t("events.title") %></h1>
```

4. **Include Locale in Routes**

Update `config/routes.rb` to add a scope for locales:
```ruby
scope "(:locale)", locale: /en|es|ja/ do
  root "events#index"
  resources :events
end
```

5. **Set Locale in ApplicationController**

Update `ApplicationController` to handle the locale:
```ruby
class ApplicationController < ActionController::Base
  before_action :set_locale

  private

  def set_locale
    I18n.locale = params[:locale] || I18n.default_locale
  end

  def default_url_options
    { locale: I18n.locale }
  end
end
```

---

5. Add a Language Switcher**

Replace static links with a language switcher using a `select`:

```erb
<%= form_with url: request.path, method: :get, local: true do |f| %>
  <%= f.select :locale,
    [["🇬🇧 English", "en"], ["🇨🇴 Español", "es"], ["🇯🇵 日本語", "ja"]],
    { selected: I18n.locale },
    { onchange: "this.form.submit()", class: "bg-gray-100 border rounded p-2" } %>
<% end %>
```

---

6. Test Your Implementation**

- Visit your application and confirm that:
   - The locale parameter is added to the URL (e.g., `/en/events` or `/es/events`).
   - Text updates dynamically based on the selected language.

- Test edge cases:
   - Switching locales on various pages.
   - Pages without the locale parameter.

7.  Add Date Localization**

Update the translation files to include date formats. For example, in `config/locales/es.yml`:
```yaml
es:
  date:
    formats:
      long: "%d de %B de %Y"
```

---

8. Expand Translations**

1. Ensure all user-facing text is replaced with `t` calls.
2. Add translations for form labels, button texts, and validation messages in your YAML files.

Example for validation messages:
```yaml
es:
  errors:
    template:
      header:
        one: "1 error impidió guardar este %{model}"
        other: "%{count} errores impidieron guardar este %{model}"
      body: "Los siguientes errores necesitan ser corregidos:"
```