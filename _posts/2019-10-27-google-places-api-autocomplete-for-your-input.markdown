---
layout: post
title:  "Google Places API Autocomplete for Your Input"
date:   2019-10-27 11:53:49 +0300
lang: en
permalink: "google-places-api-autocomplete-for-your-input"
---

Recently, I needed to understand how the Google Maps & Places API works in order to integrate it into my project. I've spent quite a lot of time, but I've figured it out and now I want to share with you how to set up Google's places autocomplete for your website.

Spoiler alert, the final result is going to look like this:

![](/assets/images/google-places-api/result.png)

#### Contents
1. [Get Google API key](#get-google-api-key)
0. [Enable required API's](#enable-required-apis)
0. [Add JavaScript to the page](#add-javascript-to-the-page)
0. [Using autocomplete options](#using-autocomplete-options)
0. [Extract data from a response](#extract-data-from-a-response)

# Get Google API Key

First, go to [Google Cloud Console](https://console.cloud.google.com/) and create
a new project. Then use a left menu to navigate to "APIs & Services" > "Credentials".

![](/assets/images/google-places-api/cred.png)

Then click "Create credentials":

![](/assets/images/google-places-api/api.png)

Now let's turn enable Maps JavaScript API and Places API in the "Library" section.

# Enable required API's

Use menu again to navigate to "APIs & Services" > "Library".

![](/assets/images/google-places-api/lib.png)

Then search for "maps javascript api" and enable it.

![](/assets/images/google-places-api/maps-enabled.png)

I recommend doing the same for Places API as well.

# Add JavaScript to the page

I hope you have your `<input>` all set up and ready to get Google'd.

To use Google Maps API add a `<script>` tag to a page:

```html
<script type="text/javascript" src="https://maps.googleapis.com/maps/api/js?key=YOUR_API_KEY&libraries=places"></script>
```

Then add this code to link the `<input>` and Google autocomplete:

```html
<script>
  const input = document.getElementById('inputId');
  const autocomplete = new google.maps.places.Autocomplete(input, {});
</script>
```

If everything was done right, you should have an autocompleted
input now.

# Using autocomplete options

Of course you'd like to customize your autocomplete and we can do that to a certain
level.

Let's make a little change:

```javascript
const input = document.getElementById('inputId');
const options = {
  types: ['establishment'],
}

const autocomplete = new google.maps.places.Autocomplete(input, options);
```

This particular `types` option narrows search results from 'everything possible'
down to 'only establishments', meaning now results will include names of shops,
restaurants, schools and not a list of names of different cities.

# Extract data from a response

Let's say you want to get name and location from result separately and not as a
single value which Google provides you with. Adding an event listener is going
to makes things done.

```javascript
google.maps.event.addListener(autocomplete, 'place_changed', () => {
  let place = autocomplete.getPlace();
  console.log(place['name'], place['formatted_location']);
})
```