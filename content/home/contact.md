---
# An instance of the Contact widget.
widget: contact

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 130

title: Contact
subtitle:

content:
  # Automatically link email and phone or display as text?
  autolink: true

  # Email form provider
  form:
    provider: netlify
    formspree:
      id:
    netlify:
      # Enable CAPTCHA challenge to reduce spam?
      captcha: true

  # Contact details (edit or remove options as required)
  email: glennmatlin@gatech.edu
  address:
    street: Klaus Advanced Computing Building
    city: Atlanta
    region: GA
    postcode: '30318'
    country: United States
    country_code: US
  coordinates:
    latitude: '33.77703637524736'
    longitude:  '-84.39589519577751'
  directions: Elevator or stairs to the 3rd Floor of KACB
  office_hours:
    - 'Monday 10:00 to 13:00'
    - 'Wednesday 09:00 to 10:00'
  appointment_url: 'https://calendly.com'
  contact_links:
    - icon: twitter
      icon_pack: fab
      name: DM Me
      link: 'https://twitter.com/glennmatlin'
    - icon: video
      icon_pack: fas
      name: Zoom Me
      link: 'https://gatech.zoom.us/my/glennmatlin'

design:
  columns: '2'
---
