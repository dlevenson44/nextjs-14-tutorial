## Next.js App Router Course - Final

This is the final template for the Next.js App Router Course. It contains the final code for the dashboard application.

For more information, see the [course curriculum](https://nextjs.org/learn) on the Next.js Website.


## NOTES
- Static Rendering is useful for UI has no data or has data that is shared across users, such as a static blog post or a product page. But it might not be a good fit for a dashboard that has data that is regularly updated.

##### Static Benefits
- Faster Websites - Prerendered content can be cached. This ensures that users around the world can access your website's content more quickly and reliably.
- Reduced Server Load - Because the content is cached, your server does not have to dynamically generate content for each user request.
- SEO - Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.

##### Dynamic Benefits
- Real-Time Data - Dynamic rendering allows your application to display real-time or frequently updated data. This is ideal for applications where data changes often.
- User-Specific Content - It's easier to serve user-specific content, such as personalized dashboards or user profiles, through dynamic rendering, as the data is updated based on user interaction.
- Request Time Information - Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL's search params.

##### Streaming
- Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.
- Fixes issue of having one slower endpoint delying whole page
- Each React component is considered its own chunk
- Chunks rendered in parallel reducing overall load time 
- Two ways to implement streaming-- with `loading.tsx` at Page level, or using `Suspense` for specific components
- `loading.tsx` is built on top of Suspense, and shows static content immediately while dynamic content loads, allowing user to navigate away quicker
- `loading.tsx` will apply to all child routes too so we have...
- Route groups which allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses ()

##### Streaming Components w React Suspense
- Suspense lets you defer rendering parts of app until some condition is met
- Can wrap dynamic components in Suspense then pass a fallback component to show while dynamic component loads

##### Grouping Components
- wrapping our `Card` component in suspense depends on a fetch for each individual card which can lead to "popping effect" as cards load in
- For a more staggered effect, group cards using a wrapper component, causing sidebar to show first then cards

###### Deciding where to place Suspense boundaries depends on...
1. How you want the user to experience the page as it streams.
2. What content you want to prioritize.
3. If the components rely on data fetching.

##### Partial Pre-Rendering (aka PPR)
- allows you to render a route statically while keeping some parts dynamic
- when user visits route static route shell is served to make initial load fast
- shell leaves holes where dynamic content will load in async
- async holes is loaded in parallel, reducing overall page load time
- PPR uses React's Concurent APIs, and uses Suspense to defer rendering parts of your app until condition is met
- Fallback is embedded into intitial static file along with other static content
- at build time static parts are prerendered while rest is postponed

##### Search Params, usePathname, and useRouter
- benefits to using url search params to manage search state:
1. Bookmarkable and Shareable URLs: Since the search parameters are in the URL, users can bookmark the current state of the application, including their search queries and filters, for future reference or sharing.
2. Server-Side Rendering and Initial Load: URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.
3. Analytics and Tracking: Having search queries and filters directly in the URL makes it easier to track user behavior without requiring additional client-side logic.

##### Server Actions
- React Server Actions let you run async code directly on the server, eliminating need to create API endpoints to mutate data
- instead you write async functions that execute on the server and can be invoked from Client or Server components
- Server Actions offer effective security solution protecting against different types of attacks, securing data, and ensuring authorized access
- Server Actions achieve this through things like POST requests, encrypted closures, strict input checks, error message hashing, and host restrictions ALL working together
- use `action` attribute in `form` elements to invoke actions
- actions automatically receive native formData object with captured data
- Advantage of calling Server Action in Server component is progressive enhancement-- forms work even if JavaScript is disabled on the client
- Server Actions deeply integrated w NextJs caching
- when form is submitted via server action, action can be used to mutate data... can also revalidated associated cache using APIs such as `revalidatePath` and `revalidateTag`
- Server Actions create a `POST` API endpoint, so you don't need to manually create endpoints

##### Error Handling
- `error.tsx` file acts as catch-all for unexpected errors and lets you display fallback UI-- kind of like `loading.tsx`
- `error.tsx` must be a Client Component
- accepts two props:  error and reset
- `error` prop is object of [Javascript's native error object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Error)
- `reset` prop is function to reset error boundary-- when executed, function will try to rerender the route
- `not-found.tsx` is another file that acts for-- you guessed it-- routes that cannot be found
- to call above, import `noteFound` from `next/navigation` to handle instances when a page is not found, can be used like below example:
```
if (!invoice) notFound();
```

##### Accessibility
- `eslint-plugin-jsx-a11y` helps catch accessibility issues-- such as images without alt text, bad use of `role` or `aria-*`, and more
- Semantic HTML: using semantic elements `input`, `option`, etc. instead of `div`-- allows assistive tech to focus on the input elements and provide proper contextual info to the user, making form easier to navigate and understand
- Labeling: `label` and `htmlFor` attributes as examples, ensures that each form field has a descriptive text label to improve AT support
- Focus Outline: fields are properly styled to show an outline when they are in focus acting as visual indicator
- Benefits of server-side validation include:
1. ensure data is in proper format before sending to DB
2. reduce risk of malicious users bypassing client-side vlaidation
3. have one source of truth for what is considered valid data

##### Authentication
- Authentication is about making sure the user is who they say they are. You're proving your identity with something you have like a username and password.
- Authorization is the next step. Once a user's identity is confirmed, authorization decides what parts of the application they are allowed to use.

##### Metadata
- Title metadata: Responsible for the title of a webpage that is displayed on the browser tab. It's crucial for SEO as it helps search engines understand what the webpage is about.
```
<title>Page Title</title>
```
- Description Metadata: This metadata provides a brief overview of the webpage content and is often displayed in search engine results.
```
<meta name="description" content="A brief description of the page content." />
```
- Keyword Metadata: This metadata includes the keywords related to the webpage content, helping search engines index the page.
```
<meta name="keywords" content="keyword1, keyword2, keyword3" />
```
- Open Graph Metadata: This metadata enhances the way a webpage is represented when shared on social media platforms, providing information such as the title, description, and preview image.
```
<meta property="og:title" content="Title Here" />
<meta property="og:description" content="Description Here" />
<meta property="og:image" content="image_url_here" />
```
- Favicon Metadata: This metadata links the favicon (a small icon) to the webpage, displayed in the browser's address bar or tab.
```
<link rel="icon" href="path/to/favicon.ico" />
```
- can add metadata via config using `metadata` object or `generateMetadata` helper in a layout or page file
- can also use file-based metadata, as Nextjs has special files
1. `favicon.ico`, `apple-icon.jpg`, and `icon.jpg`: Utilized for favicons and icons
2. `opengraph-image.jpg` and `twitter-image.jpg`: Employed for social media images
3. `robots.txt`: Provides instructions for search engine crawling
4. `sitemap.xml`: Offers information about the website's structure
- can move images from `/public` into root of `/app` and next.js automatically id and use these files as favicon and OG files, etc... can be verified in `<head>` element in dev tools
- can adjust metadata for a page in each `page.tsx` file