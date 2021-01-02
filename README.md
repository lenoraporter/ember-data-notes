# ember-data-notes

So, I don't understand Ember Data and I'm pissed. I'm tired of just not knowing so I'm going to learn out loud.

Resources I've found or purchased to learn more about Ember Data:
- [Pro Ember Data: Getting Ember Data to Work with Your API
Tang, David](https://www.amazon.com/Pro-Ember-Data-Getting-Work/dp/1484265602)
- [Ember Data: A Comprehensive Tutorial for the ember-data Library](https://www.toptal.com/emberjs/a-thorough-guide-to-ember-data)
- [Ember.js tutorial for beginners #10 Ember data, store, adapter, serializer (2020)](https://www.youtube.com/watch?v=Le0ifGiNyq4)
- [Ember Data Repository](https://github.com/emberjs/data)
- [Ember Data Guides](https://guides.emberjs.com/release/models/)

<ins>Initial questions</ins>
1. What is Ember Data?
2. Why should we use it?
3. How does it look?
4. How to use it?
5. How to fetch data from a Rails API?
6. How to make a small API and practice using Ember Data?
7. What's ember-concurrency? Does that have anything to do with Ember data?
8. What are the most efficient ways of using Ember Data?
9. What are some gotchas or things I should watch out for?
10. How can I get Ember Data wrong?
11. What ways should I not use Ember Data?
12. Who are some folks who know Ember Data well?
13. How to contribute to Ember Data as an EXTREME newbie?
14. Who hates Ember Data? What are they saying?
15. Who loves Ember Data? What are they saying?
16. Can I explain Ember Data to my mom?
17. What are some real world examples of using Ember Data in the wild?
18. What are a few best practices we should all live by while using Ember Data?

Let's start with Ember Data's README. I hate reading btw but here we go 😩. I will include my "dumb" thoughts and questions as well.


| Learning Ember Data | Website | Link |
|--------------------:|--------:|-----:|
| Ember Data ReadMe.  | https://github.com/emberjs/data | (link)[0-ember-data-readme.md]

## [Ember.js tutorial for beginners #10 Ember data, store, adapter, serializer (2020)](https://www.youtube.com/watch?v=Le0ifGiNyq4)

Shawn started the video by revealing a `data` folder called `products.js`. He showed us a raw `JSON` file filled with JavaScript objects. These objects held important product information.

Shawn wanted to make this application interact with the backend server so: 

1. Shawn move the `data/products.js` JSON file into the `public/api/items.json` file. 

```JavaScript
// public/api/items.json

{
  "data": [{
      "id": "1",
      "name": "Beats Solo Wireless Headphones",
      "description": "With up to 40 hours of battery life, Beats Solo Wireless is your perfect everyday headphone",
      "price": {
        "original": 199.95,
        "current": 99.98
      },
      "features": [
        "High-performance wireless noise cancelling headphones in red",
        "Active Noise Cancelling (ANC) blocks external noise",
        "Transparency helps you stay aware of your surroundings while listening",
        "Features the Apple H1 Headphone Chip and Class 1 Bluetooth for extended range and fewer dropouts",
        "Compatible with iOS and Android",
        "Hands-free controls via “Hey Siri” on iOS devices, and voice capability with the push of the b button on a variety of compatible devices ",
        "Up to 22 hours of listening time (up to 40 hours with ANC and Transparency turned off)"
      ],
      "colors": [{
          "color": "red",
          "image": "/assets/images/beats-solo-red.png"
        },
        {
          "color": "black",
          "image": "/assets/images/beats-solo-black.png"
        }
      ]
    },
    {
      "id": "2",
      "name": "Nike Aire Force 1",
      "description": "Debuting in 1982, the AF1 was the first basketball shoe to house Nike Air, revolutionizing the game while rapidly gaining traction around the world.",
      "price": {
        "original": 109.95,
        "current": 89.98
      },
      "features": [
        "Full-grain leather in the upper adds a premium look and feel.",
        "Originally designed for performance hoops, Nike Air cushioning adds lightweight, all-day comfort.",
        "The padded, low-cut collar looks sleek and feels great."
      ],
      "colors": [{
        "color": "white",
        "image": "/assets/images/nike-af1-white.png"
      }]
    }
  ]
}
```



2. Back in his Index route `index.js`, instead of returning `products` (from the `data/products.js` file), he used the fetch function to fetch from the public api.

Before: 

```JavaScript
import Route from '@ember/routing/route';
import { products } from '../data/products';

export default class IndexRoute extends Route {
  model() {
    return products;
  }
}
```

After:

```JavaScript
import Route from '@ember/routing/route';

export default class IndexRoute extends Route {
  async model() {
    const response = await fetch('/api/items.json'); // Since this is now a asynchronous call, we need to change the model into an async function
    const { data } = await response.json();
    return data
  }
}
```
3. Shawn needed to fetch the data for the item's details page as well.
- Shawn went to the `item` route `item.js`, changed the model to an async function, and updated the product detail's page to fetch data from the public api.

Before

```JavaScript
import Route from '@ember/routing/route';
import { products } from '../data/products';

export default class ItemRoute extends Route {
  model(params) {
    const {
      item_id
    } = params;
    const product = products.find(({ id }) => id === item_id);
    return product;
  }
}

setupController(controller, model) {
  super.setupController(controller, model);
  controller.color = model.colors[0].color;
}
```

After

```JavaScript
import Route from '@ember/routing/route';

export default class ItemRoute extends Route {
  async model(params) {
    const {
      item_id
    } = params;
    const response = await fetch('/api/items.json');
    const { data } = await response.json();
    const product = data.find(({ id }) => id === item_id); // find the specific data from the product we are looking at.
    return product;
  }
}

setupController(controller, model) {
  super.setupController(controller, model);
  controller.color = model.colors[0].color;
}
```
4. You think we stop here? No way! Now, it's time to create an Ember Data model.

Run this command in your console.
```JavaScript
ember g model product
```

This creates a `app/models/product.js` file and a `tests/unit/models/product-test.js` file. 

```JavaScript
// app/models/product.js

import Model from `@ember-data/model';

export default class ProductModel extends Model {

}
```

In order to use Ember data to pull in our products, we need to create the attributes with the { attr } decorator. { attr } has autotracking features so if anything changes, it automatically updates.

```JavaScript
// app/models/product.js

import Model, { attr } from `@ember-data/model';

export default class ProductModel extends Model {
  @attr id;
  @attr name;
  @attr description;
  @attr price;
  @attr features;
  @attr colors;
}
```

5. Now, the model is all setup. We should go back to our route handler and make sure it is using the new model from Ember data.

Before


```JavaScript
import Route from '@ember/routing/route';
import { products } from '../data/products';

export default class ItemRoute extends Route {
  async model(params) {
    const {
      item_id
    } = params;
    const response = await fetch('/api/items.json');
    const { data } = await response.json();
    const product = data.find(({ id }) => id === item_id);
    return product;
  }
}

setupController(controller, model) {
  super.setupController(controller, model);
  controller.color = model.colors[0].color;
}
```


After

a. First we need to inject a service called store.
b. Replace the `fetch` with `findall`

```JavaScript
import Route from '@ember/routing/route';
import { inject as service } from '@ember/service';

export default class ItemRoute extends Route {
@service store // It provides the find and findall methods for loading records.

async model(params) {
  const {
    item_id
  } = params;
  const data = await this.store.findAll('product'); // Add await since it is an async call.
  const product = data.find(({ id }) => id === item_id);
  return product;
 }

  setupController(controller, model) {
    super.setupController(controller, model);
    controller.color = model.colors[0].color;
  }
}
```
6. After saving the changes to the route handler, the app crashes. The error states, "Ember Data Request GET /products returned a 404
so now we need to update the name of our public api from `items.json` to `products.json`. After refreshing, it still returns a 404. Ember Data doesn't know how to get the right data so you have to teach it using adapters and serializers.

Adapters help you define how to fetch the data and where to fetch it from the server. After receiving the response, you need the serializer to convert the response or reformat it to a way where our app can recognize it.

So, let's create an adapter.

Run this in your console.

```JavaScript
ember g adapter application
```

We want to have an adapter to manage all the API calls throughout the application.

Run this in your console.

```JavaScript
ember g serializer application
```

So, let's go into the adapter.

(a). Overwrite the buildURL function
(b). Change the `import JSONAPIAdapter from '@ember-data/adapter/json-api';` to `import RESTAdapter from '@ember-data/adapter/rest';`
(c). The file is at api/products.json so you'll need to add a namespace.

```JavaScript
import RESTAdapter from '@ember-data/adapter/rest';

export default class ApplicationAdapter extends RESTAdapter {
  namespace = 'api';
  buildURL(...args) {
    return `${super.buildURL(...args)}.json`;
  }
}

```

Then, we need to update the serializer

(a). Overwrite the buildURL function
(b). Change the `import JSONAPISerializer from '@ember-data/serializer/json-api';` to `import RESTSerializer from '@ember-data/serializer/rest';`

```JavaScript
import RESTSerializer from '@ember-data/serializer/rest';

export default class ApplicationSerializer extends RESTAdapter {

}

```

7. After saving it, we need to update the `products.json` file. We need to change the object name from `data` to `products`.
Why? Well, this tells Ember Data this array contains the data for the product model.

```JavaScript
// public/api/products.json

{
  "products": [{
      "id": "1",
      "name": "Beats Solo Wireless Headphones",
      "description": "With up to 40 hours of battery life, Beats Solo Wireless is your perfect everyday headphone",
      "price": {
        "original": 199.95,
        "current": 99.98
      },
      "features": [
        "High-performance wireless noise cancelling headphones in red",
        "Active Noise Cancelling (ANC) blocks external noise",
        "Transparency helps you stay aware of your surroundings while listening",
        "Features the Apple H1 Headphone Chip and Class 1 Bluetooth for extended range and fewer dropouts",
        "Compatible with iOS and Android",
        "Hands-free controls via “Hey Siri” on iOS devices, and voice capability with the push of the b button on a variety of compatible devices ",
        "Up to 22 hours of listening time (up to 40 hours with ANC and Transparency turned off)"
      ],
      "colors": [{
          "color": "red",
          "image": "/assets/images/beats-solo-red.png"
        },
        {
          "color": "black",
          "image": "/assets/images/beats-solo-black.png"
        }
      ]
    },
    {
      "id": "2",
      "name": "Nike Aire Force 1",
      "description": "Debuting in 1982, the AF1 was the first basketball shoe to house Nike Air, revolutionizing the game while rapidly gaining traction around the world.",
      "price": {
        "original": 109.95,
        "current": 89.98
      },
      "features": [
        "Full-grain leather in the upper adds a premium look and feel.",
        "Originally designed for performance hoops, Nike Air cushioning adds lightweight, all-day comfort.",
        "The padded, low-cut collar looks sleek and feels great."
      ],
      "colors": [{
        "color": "white",
        "image": "/assets/images/nike-af1-white.png"
      }]
    }
  ]
}
```
8. After saving, we get one more error. Error states "Error while processing route: item Assertion Failed: You may not set `id` as an attribute on your model. Please remove any lines that look like `id`;


```JavaScript
// app/models/product.js

import Model, { attr } from `@ember-data/model';

export default class ProductModel extends Model {
  @attr id; // Remove id
  @attr name;
  @attr description;
  @attr price;
  @attr features;
  @attr colors;
}
```

9. Last step is to update the index.

Before

```JavaScript
// app/routes/index.js

import Route from '@ember/routing/route';

export default class IndexRoute extends Route {
  async model() {
    const response = await fetch('/api/items.json'); // Since this is now a asynchronous call, we need to change the model into an async function
    const { data } = await response.json();
    return data
  }
}
```


After
```JavaScript
// app/routes/index.js

import Route from '@ember/routing/route';
import { inject as service } from `@ember/service`;

export default class IndexRoute extends Route {
  @service store;
  
  async model() {
    return this.store.findAll('product');
  }
}
```

Questions while watching this tutorial
1. What is the `buildURL` function?
2. Do we still need to use setupController?
3. What is the difference between Rest and JSON?
4. When to make things plural and when to make it singular? I'm confused by the `products` `product` thing.
5. Why did we have to remove the id?
