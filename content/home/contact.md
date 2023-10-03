---
# An instance of the Contact widget.
widget: contact

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 200

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
    street: CODA at Tech Square, 756 W Peachtree St
    city: Atlanta
    region: GA
    postcode: '30308'
    country: United States
    country_code: US
  coordinates:
    latitude: '33.7752651'
    longitude:  '-84.3876426'
#  directions: 
#  office_hours:
#    - 'Monday 10:00 to 13:00'
#    - 'Wednesday 09:00 to 10:00'
#  appointment_url: 'https://calendly.com'
  contact_links:
    - icon: twitter
      icon_pack: fab
      name: Twitter
      link: 'https://twitter.com/glennmatlin'
#    - icon: video
#      icon_pack: fas
#      name: Zoom Me
#      link: 'https://gatech.zoom.us/my/glennmatlin'

design:
  columns: '2'
---
