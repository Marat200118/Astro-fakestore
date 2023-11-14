# Astro Fakestore

We would like to apologize if you were kept awake by the fact that we created a Fake Store site (recap exercise) which heavely depended on client-side javascript.

Since then, we've learned a lot of new things in the course, and we would like to make it up to you by creating a new version of the Fake Store site, but this time with Astro!

## Astro setup

- Create a new -Empty- Astro project to get started.
- Create a base layout, move everything from the `index.astro` to there since we will be using it on all pages. Use the BaseLayout in the `index.astro` page.
- Add the Google font data to the base layout, so it's available on all pages.

  ```html
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link
    href="https://fonts.googleapis.com/css2?family=Roboto+Slab:wght@400;800&display=swap"
    rel="stylesheet"
  />
  ```

- Set the `<slot/>` tag in a `<main>` tag in the body of the base layout.
- Make sure we can add something to the title of the page (hint: props)

### CSS

We will use some site-wide CSS, the rest will be component specific.

- In a `styles` directory, add a `reset.css` file
- Add a `global.css` file to the `styles` directory with the following content:

  ```css
  html {
    box-sizing: border-box;
  }
  *, *::before, *::after {
    box-sizing: inherit;
  }

  img {
    max-width:  100%;
    height: auto;
  }

  .visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    border: 0;
  }

  :root {
    --s_2: .2rem;
    --s_1: .5rem;
    --s1: 1rem;
    --s2: 1.2rem;
    --s3: 1.5rem;
    --s4: 2rem;

    --fs-s: .8rem;
    --fs-m: 1rem;
    --fs-l: 1.5rem;
    --fs-xl: 2rem;

    --c-bg1: #8EC5FC;
    --c-bg2: #E0C3FC;

    --c-dark: #4c4c4c;
    --c-mid: #757575;
    --c-primary: #ff3f40;

    --border-radius: .25rem;
    --border-radius-xl: 1rem;

    --maxContent: 60rem;
     --defaultShadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
  }


  body {
    line-height: 1.5;
    color:var(--c-dark); 
    padding: clamp(1rem, 5vw, 5rem);
    
    background-color: var(--c-bg1);
    background-image: linear-gradient(80deg, var(--c-bg1) 0%, var(--c-bg2) 100%);

    font-family: 'Roboto Slab', serif;
  }
  ```

## Header component

- Create a new component called `Header.astro` and add it to the base layout.
- Add the following HTML to the component:

  ```html
  <header>
    <h1><a href="/">Fake Astro Store</a></h1>
  </header>
  ```

- This is the CSS for that component
  
  ```css
  h1 {
    background-color: white;
    color: var(--c-primary);
    padding: var(--s1) var(--s3);
    font-size: clamp(2rem, 5vw, 4rem);
    font-weight: 800;
    border-radius: var(--border-radius-xl);
    line-height: 1;

    text-align: center;
    display: block;
    margin-bottom: var(--s4);
  }

  a {
    text-decoration: none;
    color: inherit;
  }

  h1:has(a:hover) {
    background-color: var(--c-primary);
    color: white;
  }
  ```

## Products

- Create a new component called `ProductsList.astro` and add it to the index.astro page.
- In the [component script](https://docs.astro.build/en/core-concepts/astro-components/#the-component-script) fetch the product data. <https://fakestoreapi.com/products>
- In the [component template](https://docs.astro.build/en/core-concepts/astro-components/#the-component-template) loop over the products and show them in a list. For now, just the title of the product is enough.

## Product card

We could create a product card directly in the products list, but we will create a separate component for it. Reusability ftw!

- Create a new component called `ProductCard.astro`
- Destructure the 'product' variable from the props in the component script.
- Add the HTML to show the product, something like this: (implement the product details with the data from the product variable)

  ```html
   <article>
    <img src="Product image url"
      alt="Product image"
      width="300"
      height="400"
    />
    <h3>Product title</h3>
    <p class="description">
      Product description
    </p>
    <p class="price">&euro; <span>Product price</span></p>
    <button>Buy now</button>
  </article>
  ```

- Style it with some CSS

```css
 article {
    height: 100%;

    background-color: white;
    border-radius: var(--border-radius);
    box-shadow: var(--defaultShadow);
    position: relative;

    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: auto min-content;
    grid-template-areas:
      "img img"
      "title title"
      "desc desc"
      "price cta";

    align-items: flex-start;
    gap: var(--s1);

    padding: var(--s2);

    max-width: var(--maxContent);
    margin: 0 auto;
    margin-bottom: var(--s3);
  }

  article:hover {
    box-shadow: rgba(0, 0, 0, 0.4) 0px 4px 12px;
  }

  h3 {
    font-size: var(--fs-l);
    font-weight: 900;
    grid-area: title;
  }

  img {
    aspect-ratio: 3/4;
    object-fit: contain;

    grid-area: img;

    justify-self: center;

    padding: var(--s1);
  }

  .description {
    grid-area: desc;
    align-self: baseline;

    display: -webkit-box;
    -webkit-line-clamp: 4;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;

    color: var(--c-mid);
  }

  .price {
    color: var(--c-primary);
    grid-area: price;
    line-height: 1;
    align-self: flex-end;
  }

  .price span {
    font-size: var(--fs-l);
  }

  button {
    grid-area: cta;
    align-self: flex-end;

    background-color: var(--c-primary);
    color: white;
    border: none;
    padding: var(--s_2) var(--s_1);
    font: inherit;
    outline-color: var(--c-primary);
    outline-offset: var(--s_2);
  }

  button:hover {
    background-color: var(--c-dark);
  }
  ```

- Use this `ProductCard` in the ProductsList component to show the products. It can be something like this:

  ```jsx
  <ul>
  {
    products.map((product) => (
      <li>
          <ProductCard product={product} />
      </li>
    ))
  }
  </ul>
  ```

- We just have to add some CSS to style the product list. Here you have it:
  
  ```css
  ul {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(20rem, 1fr));
    grid-gap: var(--s4);
  }

  a {
    text-decoration: none;
    color: inherit;
  }
  ```
  
- We don't show if a product is popular or not. Let's fix this. First add the CSS:
  
  ```css
  .popular::after {
    content: "";
    background-color: var(--c-primary);
    position: absolute;
    top: 0;
    right: 0;
    width: 4em;
    height: 4em;
    padding-top: 2em;
    clip-path: polygon(0 0, 100% 0, 100% 100%);
  }

  .popular::before {
    content: "Top rated";
    color: white;
    position: absolute;
    top: 1.4em;
    right: 0;
    rotate: 45deg;
    z-index: 2;
    font-size: 0.7em;
  }
  ```

  Now, add a conditinal rendering to the class attribute of the article tag in the `ProductCard` component. Only add the class `popular` to the article tag when the `rating.rate` property is higher than 4.5

Et voilà, our index page is done!

## Product detail page

We will create a detail page for a product. A user will be able to click on a product card on the homepage and get a detail page for that specific product. We will use the same ProductCard component but style it differently using container queries.

- In the `pages` directory, create a new directory `products` with an `[id].astro` file in it.
- We will be using dynamic routes to create all the product pages at build time. See [Dynamix routes (SSG mode)](https://docs.astro.build/en/core-concepts/routing/#static-ssg-mode) for more info. Inside the `getStaticPaths` function, fetch all the products from the API and return an array of objects with the product ID's. Something like this:

  ```js
  return products.map(product => ({
    params: { id: product.id.toString() },
    props: {
      product
    },
  }));
  ```

- Now, get the product data from the props, include the `ProductCard` component and pass the product data to it. Don't forget to add the BaseLayout to the page. Don't worry about the styling, we will fix that in the coming steps.
- Before we forget, wrap the `ProductCard` component in the `ProductsList` with an anchor tag to link to the detail page like this:

  ```jsx
  <a href={`/products/${product.id}`}>
    <ProductCard product={product} />
  </a>
  ```

### Container query

We need some different styling for the product detail card on the detail page. If we want to use container queries for this (and we do want this 😉)

- Add a `div` with a class `wrapper` around the `article` in the `ProductCard` component.
- Start with the following CSS to create a query container:

  ```CSS
  .wrapper {
    height: 100%;
    container-type: inline-size;
  }
  ```

  We leave it up to you to create a fabulous desing for the details view. A min width of 30rem is a good value to kick things in.

  Note that we simply show the same content here. In a real context, we would expect to see more details about the product...

### Paging

We will let the user browse to a previous or next product via the detail page. For this to work, we need the previous and next product (if any) and a component that renders the paging links.

- Create a new component called `PrevNext.astro` and destructure the `prev`, `next` and `path` props in the component script. If we can set the `path` via props, we can use this component for other occasions as well.
- Noting realy spectacular in here, so this is the CSS and HTML

  ```JSX
  <nav>
  <ul>
    {
      prev && (
        <li>
          <a href={`/${path}/${prev.id}`}>Previous</a>
        </li>
      )
    }
    {
      next && (
        <li class="right">
          <a href={`/${path}/${next.id}`}>Next</a>
        </li>
      )
    }
  </ul>
  </nav>
  ```

  ```CSS
  nav {
    background-color: white;
    border-radius: var(--border-radius-xl);
    margin-right: auto;
    margin-left: auto;
    padding: var(--s1) var(--s4);
    max-width: var(--maxContent);
  }

  ul {
    display: flex;
    justify-content: space-between;
  }

  .right {
    margin-left: auto;
  }
  ```

- To implement this on our detail page, we need to provide a previous and next property to the page props.
  
  ```js
  next: i < products.length - 1 ? products[i + 1] : null,
  prev: i > 0 ? products[i - 1] : null,
  ```

- Add the `PrevNext` component to the detail page and pass the `prev`, `next` and `path` props to it. The `path` will be `products` in this case.
- Check if you can navigate to the previous and next product. And back and forth to the index page.

We are done with the basics, but let us enhace it even more!

## Images

There are a couple of ways to [handle images](https://docs.astro.build/en/guides/images/) in Astro. We will make use of the `<Image />` component from Astro itself. This component will automatically optimize the images for us.

- Replace the `<img>` tag with a `<Image>` tag
- Add some widths to it <https://docs.astro.build/en/guides/images/#widths>
- Don't forget the `sizes` property.

There is an issue though. Sizes are based on vieuwport widths, but we are using container queries. There doesn't seem to be a solution for this yet. Sacrifices have to be made here. Try to find an in-between solution.

- Since the images are hosted on another domain than ours (fakestoreapi.com) we have to authorize them. See <https://docs.astro.build/en/guides/images/#authorizing-remote-images> for more context. If you've set everything up correctly, you should see webp images in different sizes in your inspector. Try to make a build as well.

## View transitions

You might have picked it up somewhere already, [view transitions](https://developer.chrome.com/docs/web-platform/view-transitions/) (aka: page transitions) are a thing now on the web. Astro jumped on the wagon and made [implementing them](https://docs.astro.build/en/guides/view-transitions/) a breeze.

- Add the `<ViewTransitions />` component to the head of our BaseLayout.
- To let the browser know wich element corresponds with another element on another page, we can give them names. In our case, we would like to do this with our product cards. To distinguish them, we will use the product ID. We end up with something like this, add it to the product card wrapper div

  ```jsx
  transition:name={`product-${product.id}`}
  ```

  Without specifying any other things, we shoul be seeing a default fade transistion when navigating between the index and detail page. This should also be the case when navigating to previous or next products
