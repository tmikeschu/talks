footer: @tmikeschu
build-lists: true
slidenumbers: true
slidecount: true

#[fit] Get Off My Grass

## Weeding Out Logic with _Presenters_

Mike Schutte
8/8/19
THAT Conference

^ Welcome and thanks for coming to the talk formerly known as "The Declarative
Dark". I fell in love with this dandelion analogy this week so here I am telling
you to get off my grass. Regardless of the title, we're gonna be learning the
about the same thing today: the presenter pattern.

---

![](./images/that-branding.png)

^ Before we get started, a quick reminder about where you are right now in case
you forgot.

---

![](./images/that-sponsors.png)

^...and of course a thank you to these organizations.

---

[.hide-footer]

- @tmikeschu (:tea::microphone::shoe:)

- :house_with_garden:

- :sunrise_over_mountains:

- :guitar: :microphone: :musical_keyboard:

- :runner: :bike:

- ![inline](https://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/TED_three_letter_logo.svg/1200px-TED_three_letter_logo.svg.png)

^ My name is Mike Schutte. For anything that has a username I am tmikeschu,
which can luckily be said using emojis. I live in Detroit with my now fiancée
in our 119 year-old house. I am originally from out west, Wyoming Montana
Washington and Colorado, so you can imagine I am a big lover of big sky and wild places. I play guitar and piano and I sing. Ask me about the Avett Brothers. I love running long distances and biking around urban areas. I help spread cool ideas doing front-end stuff at TED.

---

Questions along the way?

_@tmikeschu_ `#getOffMyGrass`

---

#[fit] FWBAT

_friendgineers_ will be able to...

---

- Explain what a presenter object is

- Explain why a presenter object is useful

- Detect when a presenter is the tool for the job

- Expound upon the virtues of the Ruby language

---

# Roadmap

---

- The View Layer

- The Presenter Pattern

- Defining Dependencies

- Conditional Rendering

- Rendering Collections

- Message passing

- Testing

^ We'll start at a more abstract and theoretical level and discuss what the view
layer is, what the presenter pattern is, and why it is useful. Then we'll
descend from the theoretical clouds to the firm ground of production code. Based
on recent work, I'll highlight some benefits that the pattern can precipitate.

---

# The View Layer

- MVC

- Modern JS libraries

- HTML

- JSON

^ The view is the part of an application consumed in the wild. Consumption could
be viewing HTML in a web browser, fetching a JSON payload from an external API,
and more.

---

# The Presenter Pattern

- Controller _gathers_ model data and
- Controller _instantiates_ a presenter object with model data
- View _sends messages_ to the presenter for all logical needs

^ I'll be using Ruby on Rails as a heuristic. If you're not familiar with Rails
or MVC architecture, the controller is the http aware unit in the system. The
model is the database aware unit. We've discussed the view.

---

[.slide-transition: true]<br>

## Controller _constructs_

## View _consumes_

---

```rb
# app/controllers/quidditch_controller.rb
class QuidditchController
  def index
    @presenter = QuidditchPresenter.present(players: Player, scores: Score)
  end
end
```

---

```rb
# app/presenters/quidditch_presenter.rb
class QuidditchPresenter
  def self.present(players:, scores:)
    # ...
  end

  def leading_scorers
    # ...
  end
end
```

---

```erb
<!-- # app/views/quidditch/index.html.erb -->
<div>
  <% @presenter.leading_scorers.each do |leading_scorer| %>
    ...
  <% end %>
</div>
```

---

## Not just server-side templates!

---

```jsx
class QuidditchContainer extends React.Component {
  state = { players: [], scores: [] };

  componentDidMount() {
    PlayerService.get().then(players => {
      this.setState({ players });
    });
    ScoresService.get().then(scores => {
      this.setState({ scores });
    });
  }
  render() {
    const { players, scores } = this.state;
    return <QuidditchScores {...quidditchPresenter({ players, scores })} />;
  }
}
```

---

```js
function quidditchPresenter({ players, scores }) {
  return {
    leadingScorers: /* ... */
  };
}
```

---

```jsx
function QuidditchScores({ leadingScorers }) {
  return (
    <div>
      {leadingScorers.map(leadingScorer => (
        <div>...</div>
      ))}
    </div>
  );
}
```

---

# What

---

A presenter object _encapsulates logic_ (e.g., boolean and transformation) needed for building a particular _view_.

^ A presenter is an object that is easy to talk to.

^ The view is under so much pressure to look good and say the right thing. It is
soothed by having a presenter object it can depend on for its data.

^ The view is still tightly coupled to something, but that something is an
interface instead of concrete content.

---

# Why

---

[.slide-transition: true]<br>

## Controller: integration :dango:

^ there shouldn't be much unit-testable logic in the controller

---

[.slide-transition: true]<br>

## View: content and style :nail_care:

---

[.slide-transition: true]<br>

## View: declarative interface :round_pushpin:

^ If the view template knows too much about the data it needs to compute,
then any time the data changes, the view needs to change (or be copied).

---

[.slide-transition: true]<br>

## View: signal to noise :signal_strength:

---

[.slide-transition: true]<br>

## View: explicit dependencies :mag:

^ The presenter's constructor defines everything the view needs to render
correctly.

---

[.slide-transition: true]<br>

## Unit test :white_check_mark:

---

[.slide-transition: true]<br>

# Lessons from the Field :pencil:

^ As fate would have it, I ran into a great use case for a presenter just last
month.

---

## Context

- Event highlights for each attendee of an event

- Same general layout for all events

- Unique branding for each event

- Various data sources

- Precedence: copy and paste previous event

---

[.slide-transition: true]<br>

#[fit] :nose:

---

- _84_-line controller action, _0_ helper methods
- _23_ instance variables used by view
- _3_ view directories with _basically identical_ code
- _7_ `&&`, _11_ `||`
- _26_ `if` statements, _20_ ternary expressions
- _5_ cases of manually iterated markup
- _20+_ Law of Demeter violations

---

[.build-lists: false]<br>
[.slide-transition: true]<br>

- _84_-line controller action, _0_ helper methods
- _23_ instance variables used by view
- _3_ view directories with _basically identical_ code
- _7_ `&&`, _11_ `||`
- ~~_26_ `if` statements,~~ _20_ ternary expressions
- _5_ cases of manually iterated markup
- _20+_ Law of Demeter violations

---

## post-presenter...

---

- _~~84~~ 12_-line controller action, _~~0~~ 16_ helper methods
- _~~23~~ 1_ instance variable used by view
- _1_ view template with shared partials
- _~~7~~ 5_ `&&`, _~~11~~ 1_ `||`
- _19_ `if`, _6_ `elsif`, _~~20~~ 0_ ternaries
- _~~5~~ 0_ cases of manually iterated markup
- _~~20+~~ 1_ Law of Demeter violation

---

# Defining dependencies

^ In a templating system where variables can be accessed by the view layer, it
becomes challenging to understand what the template's dependencies are.
Especially if there are multiple sources that can define variables for the view.

^ A controller might define an instance variable to be used in the view. The
variable actually is only used in a partial. A dev sees that the variable in the
controller action is not used by the corresponding template, so they delete the
variable.

^ View libraries like Vue and React do this well with prop types.

---

```erb
<%=
  render "frontend/highlights/#{params[:event_slug].upcase}/index",
  locals: {
    activity_names: @activity_names,
    conf_activities: @conf_activities,
    conf_connections: @conf_connections,
    conf_general: @conf_general,
    conf_hearts: @conf_hearts,
    conf_messages: @conf_messages,
    dinners: @dinners,
    event_photos: @event_photos,
    faceoff: @faceoff,
    lunches: @lunches,
    photo_ids: @photo_ids,
    public_token: @public_token,
    session_list: @session_list,
    t19_registrant: @t19_registrant,
    t20_registrant: @t20_registrant,
    workshops: @workshops,
    sunday: @sunday,
    monday: @monday,
    tuesday: @tuesday,
    wednesday: @wednesday,
    thursday: @thursday,
    friday: @friday,
    saturday: @saturday
  }
%>
```

---

[.code-highlight: all]<br>
[.code-highlight: 3]<br>
[.code-highlight: 4-8]

```rb
def show
  @presenter = Frontend::HighlightsPresenter.present(
    json_resources.merge({
      event_config: event_config,
      event_titles: event_titles,
      eventster: eventster,
      faceoff_data: faceoff_data,
      view_more_photos_url: view_more_photos_url,
    })
  )
  render :layout => 'layouts/highlights'
end
```

---

```rb
  def self.present(
    activities:,
    connections:,
    event_config:,
    event_titles:,
    eventster:,
    faceoff_data:,
    hearts:,
    view_more_photos_url:
  )
    # initialize/construct
  end
```

---

# Conditional rendering

^ More often than not, the need for conditional rendering does not change. We only
want to show this content or visual element under certain conditions. What does
change is how those conditions are evaluated. Moving boolean logic to the
presenter allows the view to focus on asking the question and not answering it
too.

^ One reason to change

---

## Separate the _need_ for a test from the _test itself_

---

[.slide-transition: true]

## Separate the _statement_ from the _expression_

---

```erb
<% if locals[:workshops] > 0 || locals[:dinners] > 0 || locals[:lunches] > 0 || locals[:sunday].any? || locals[:monday].any? || locals[:tuesday].any? || locals[:wednesday].any? || locals[:thursday].any? || locals[:friday].any? || locals[:saturday].any? %>
  <!-- ... -->
<% end %>
```

---

[.slide-transition: true]

```erb
<% if locals[:workshops] > 0 ||
      locals[:dinners] > 0 ||
      locals[:lunches] > 0 ||
      locals[:sunday].any? ||
      locals[:monday].any? ||
      locals[:tuesday].any? ||
      locals[:wednesday].any? ||
      locals[:thursday].any? ||
      locals[:friday].any? ||
      locals[:saturday].any? %>
  <!-- ... -->
<% end %>
```

---

vs.

---

```rb
# presenter
def show_activities?
  @show_activities ||= went_to_workshops? || went_to_dinners? || went_to_lunches?
end
```

```erb
<!-- view -->
<% if @presenter.show_activities? %>
  <!-- ... -->
<% end %>
```

---

# Iteration logic

^ Iteration is another situation where a lot of hairy logic can seep into the
controller. Not only does the view need to know the API for iterating the
collection, it has to know the API for each element.

---

[.code-highlight: all]<br>
[.code-highlight: 4, 7]<br>
[.code-highlight: 12, 15]<br>
[.code-highlight: 20, 23]<br>
[.code-highlight: 28, 31]<br>
[.code-highlight: 36, 39]<br>

```erb
<div class="flex flex-wrap tc-ns">
  <div class="w-50 w-20-ns mb3 mb0-ns">
    <div class="f2 f1-l pv2">
      54
    </div>
    <div class="f6 ph3">
      Countries represented
    </div>
  </div>
  <div class="w-50 w-20-ns mb3 mb0-ns">
    <div class="f2 f1-l pv2">
      105
    </div>
    <div class="f6 ph3">
      Speakers
    </div>
  </div>
  <div class="w-50 w-20-ns mb3 mb0-ns">
    <div class="f2 f1-l pv2">
      44
    </div>
    <div class="f6 ph3">
      Discovery Sessions
    </div>
  </div>
  <div class="w-50 w-20-ns mb3 mb0-ns">
    <div class="f2 f1-l pv2">
      23
    </div>
    <div class="f6 ph3">
      Exhibits + social spaces
    </div>
  </div>
  <div class="w-50 w-20-ns mb3 mb0-ns">
    <div class="f2 f1-l pv2">
      1,411
    </div>
    <div class="f6 ph3">
      Conversations with Gigi
    </div>
  </div>
</div>
```

---

vs.

---

[.code-highlight: all]<br>
[.code-highlight: 5, 8]<br>

```erb
<div class="flex flex-wrap tc-ns">
  <% @presenter.at_a_glance_stats.each do |(label, value)| %>
    <div class="w-50 w-20-ns mb3 mb0-ns">
      <div class="f2 f1-l pv2">
        <%= value %>
      </div>
      <div class="f6 ph3">
        <%= label %>
      </div>
    </div>
  <% end %>
</div>
```

---

## Limit element interface to _simple message passing_

---

[.code-highlight: all]<br>
[.code-highlight: 3-4]<br>

```erb
<div class="w-third w-20-ns bg-near-black square cover hide-child relative bg-center"
     style="background-image:url(<%= crushinate(
        locals[:event_photos][4].present? ? locals[:event_photos][4]['url']
                                          : 'https://live.staticflickr.com/65535/33746025918_03319ba6a1_o_d.jpg',
        w: 550, quality:90
      ) %>)">
  <a href="<%= locals[:event_photos][4].present? ? locals[:event_photos][4]['url']
                                                 : 'https://live.staticflickr.com/65535/33746025918_03319ba6a1_o_d.jpg' %>"
     class="db f6 white child v-mid bg-black-50 w-100 h-100 absolute absolute--fill lightbox">
    <div class="vertical-align">
      <img src="<%= image_path 'highlights/expand.svg' %>" />
    </div>
  </a>
</div>
```

(\* 10)

---

vs.

---

[.code-highlight: all]<br>
[.code-highlight: 2]<br>
[.code-highlight: 3]<br>
[.code-highlight: 4]<br>

```erb
<% @presenter.tile_photos.each do |tile_photo| %>
  <div class="<%= tile_photo.css_classes %>"
       style="background-image:url(<%= crushinate(tile_photo.id, w: 550, quality:90) %>)">
     <a href="<%= tile_photo.id %>"
        class="db f6 white child v-mid bg-black-50 w-100 h-100 absolute absolute--fill lightbox">
        <div class="vertical-align">
          <img src="<%= image_path 'highlights/expand.svg' %>" />
        </div>
     </a>
  </div>
<% end %>
```

---

# Testing

---

## Many presenter methods _might not need unit tests_

---

```rb
def show_hearts?
  hearts_enabled? && heart_count > 0
end

def show_more_hearts?
  heart_count > 10
end

def heart_count
  @heart_count ||= hearts.count
end
```

---

Non-trivial computations or transformations are _simple_ to test

- (i.e., no need for spies, mocks, or stubs)

---

[.code-highlight: all]<br>
[.code-highlight: 16]<br>
[.code-highlight: 18]<br>

```rb
def workshop_days
  default_days = %i(Sunday Monday Tuesday Wednesday Thursday Friday Saturday)
  start = default_days.index(first_day.capitalize.to_sym)
  days = default_days.rotate(start)
  abbreviations = {
    Sun: :Sunday,
    Mon: :Monday,
    Tue: :Tuesday,
    Wed: :Wednesday,
    Thu: :Thursday,
    Fri: :Friday,
    Sat: :Saturday,
  }

  @workshop_days ||= activities
    .group_by { |raw| raw.values.first.second.slice(0, 3) }
    .map { |day, activities| WorkshopDay.new(abbreviations.fetch(day.to_sym), activities) }
    .sort_by { |workshop_day| days.index(workshop_day.to_s.to_sym) }
end
```

---

## Reminder

Presenters are _dependent_ on some kind of _orchestrator_ for their data

- (e.g., controller, container component)

---

[.slide-transition: true]<br>
[.code-highlight: all]<br>
[.code-highlight: 4-13]<br>
[.code-highlight: 15-22]<br>
[.code-highlight: 3, 23]<br>

```rb
# spec/support/presenter_factory.rb
module PresenterFactory
  def self.make_highlights_presenter(options = {})
    titles = OpenStruct.new(
      asset_slug: "TEDSpaceship3000",
      full_title: "TEDSpaceship 3000: A Galaxy Far Far Away",
      title: "TEDSpaceship 3000"
    )

    eventster = OpenStruct.new(
      badge: OpenStruct.new(firstname: "Voldemort")
    )

    Frontend::HighlightsPresenter.present({
      activities: [],
      connections: { old: [], new: [] },
      event_titles: titles,
      event_config: {},
      eventster: eventster,
      faceoff_data: { events: [], interesting: [], personal: [] },
      hearts: [],
      view_more_photos_url: "www.com",
    }.merge(options))
  end
end
```

---

[.slide-transition: true]<br>
[.code-highlight: all]<br>
[.code-highlight: 1]<br>
[.code-highlight: 2-9]<br>
[.code-highlight: 11]<br>
[.code-highlight: 13-19]<br>

```rb
describe "#workshop_days" do
  let(:activities) {
    JSON.parse(
      Rails.root.join(
        *%w[spec fixtures frontend highlights s3_resources.json]
      ).read,
      symbolize_names: true
    ).fetch(:activities)
  }

  let(:subject) { PresenterFactory.make_highlights_presenter(activities: activities) }

  it "returns an ordered collection of days and their activities" do
    actual = subject.workshop_days
    expect(actual.first.to_s).to eq("Tuesday")
    expect(actual.first.activities.count).to eq(2)
    expect(actual.second.to_s).to eq("Friday")
    expect(actual.second.activities.count).to eq(1)
  end
  # ...
```

---

[.slide-transition: true]<br>
[.code-highlight: all]<br>
[.code-highlight: 1]<br>
[.code-highlight: 2-9]<br>
[.code-highlight: 11-17]<br>

```rb
  context "when first_day is configured" do
    let(:subject) {
      PresenterFactory.make_highlights_presenter(
        activities: activities,
        event_config: {
          first_day: "friday"
        }
      )
    }

    it "orders relative to that day" do
      actual = subject.workshop_days
      expect(actual.first.to_s).to eq("Friday")
      expect(actual.first.activities.count).to eq(1)
      expect(actual.second.to_s).to eq("Tuesday")
      expect(actual.second.activities.count).to eq(2)
    end
  end
end
```

---

# Review :dizzy:

---

- The View Layer

- The Presenter Pattern

- Defining Dependencies

- Conditional Rendering

- Rendering Collections

- Message passing

- Testing

---

#[fit] FWBAT

---

## What is a presenter object?

a) a laser pointer

b) an object that encapsulates logic needed for building a view

c) the view layer of an application

---

[.slide-transition: true]<br>

## What is a presenter object?

a) a laser pointer

b) _an object that encapsulates logic needed for building a view_ :white_check_mark:

c) the view layer of an application

---

## Why is a presenter object useful?

a) controller focuses on integration

b) view focuses on content and styling

c) explicitly defines the view's dependencies

d) easy to unit test

e) all the above

---

[.slide-transition: true]<br>

## Why is a presenter object useful?

a) controller focuses on integration

b) view focuses on content and styling

c) explicitly defines the view's dependencies

d) easy to unit test

e) _all the above_ :white_check_mark:

---

## When might a presenter be the tool for the job?

a) computational logic in a controller (-like object)

b) repetitive markup

c) complex logic in the view

d) multiple objects used in the view

e) all the above

---

[.slide-transition: true]<br>

## When might a presenter be the tool for the job?

a) computational logic in a controller (-like object)

b) repetitive markup

c) complex logic in the view

d) multiple objects used in the view

e) _all the above_ :white_check_mark:

---

## What do you love about the Ruby language?

a) yes

b) I choose to fail the quiz

---

[.slide-transition: true]<br>

## What do you love about the Ruby language?

a) _yes_ :white_check_mark:

b) I choose to fail the quiz

---

![](./images/that-2020.png)

---

[.slide-transition: true]<br>
[.hide-footer]

#[fit] Thank you!

## :tada: :clap: :tada: :clap: :tada: :clap: :tada: :clap: :tada: :clap: :tada: :clap: :tada:

Questions/comments: _@tmikeschu_ `#getOffMyGrass`
