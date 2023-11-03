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