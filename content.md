# Business Insights and Analytics🕴️ 

[Lecture Video](https://youtu.be/GBTeAbqeC14)

As part of the product development lifecycle you'll want to collect and visualize data to help you make informed decisions.
For business analytics I recommend checking out the [blazer gem](https://github.com/ankane/blazer). It allows you to create reports using SQL queries and is already integrated with [chartkick](https://github.com/ankane/chartkick) to visualize the data.

```ruby
# make sure you authorize who can access the blazer dashboard!
authenticate :user, ->(user) { user.admin? } do
  mount Blazer::Engine, at: "blazer"
end
```
For collecting analytics like page views and visits you might want to use something like the [ahoy gem](https://github.com/ankane/ahoy). This way you can have a better understanding of who is visiting your site and which pages they are visiting.

You may also want to track more user data (eg sign_in_count, last_sign_in_at, current_sign_in_ip, etc) using [Devise Trackable module](https://github.com/heartcombo/devise).

Here are some helpful queries to get you started with blazer and ahoy.
```sql
-- Daily Visitors
SELECT date(started_at) AS date, count(DISTINCT visitor_token) AS unique_visitors
FROM ahoy_visits
GROUP BY date
ORDER BY date DESC

-- Source of Traffic
SELECT referring_domain, count(DISTINCT visitor_token) AS unique_visitors
FROM ahoy_visits
WHERE referring_domain IS NOT NULL
GROUP BY referring_domain
ORDER BY count(DISTINCT visitor_token) DESC;

-- Favorite pages by visits (and optional time constraint)
SELECT properties, COUNT(*) AS visit_count
FROM ahoy_events
where name = 'Ran action'
GROUP BY properties
ORDER BY visit_count DESC;

-- add time constraint where clause
time >= NOW() - INTERVAL '24 hours'
```
