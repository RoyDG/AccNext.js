## Key points of pages features
* Pages

    In Next.js, a page is a React Component exported from a .js, .jsx, .ts, or .tsx file in the pages directory. Each page is associated with a route based on its file name.

    Example: If you create pages/about.js that exports a React component like below, it will be accessible at /about.

        function About() {
        return <div>About</div>
        }

        export default About

* Pages with dynamic routs

    Next.js supports pages with dynamic routes. For example, if you create a file called `pages/posts/[id].js, then it will be accessible at posts/1, posts/2`, etc.

* Pre-rendering

    By default, Next.js pre-renders every page. This means that Next.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript. Pre-rendering can result in better performance and SEO.

* Two forms of Pre-rendering

    Next.js has two forms of pre-rendering: Static Generation and Server-side Rendering. The difference is in when it generates the HTML for a page.

    Static Generation (Recommended): The HTML is generated at build time and will be reused on each request.
    Server-side Rendering: The HTML is generated on each request.
    Importantly, Next.js lets you choose which pre-rendering form you'd like to use for each page. You can create a "hybrid" Next.js app by using Static Generation for most pages and using Server-side Rendering for others.

* Static Generation

    If a page uses Static Generation, the page HTML is generated at build time. That means in production, the page HTML is generated when you run next build . This HTML will then be reused on each request. It can be cached by a CDN.

* Static Generation without data

    By default, Next.js pre-renders pages using Static Generation without fetching data. Here's an example:

        function About() {
        return <div>About</div>
        }

        export default About

    Note that this page does not need to fetch any external data to be pre-rendered. In cases like this, Next.js generates a single HTML file per page during build time.

* Static Generation with data

    Some pages require fetching external data for pre-rendering. There are two scenarios, and one or both might apply. In each case, you can use a special function Next.js provides:

    1.Your page content depends on external data: Use getStaticProps.
    2.Your page paths depend on external data: Use getStaticPaths (usually in addition to getStaticProps).

* When should I use Static Generation?

    We recommend using Static Generation (with and without data) whenever possible because your page can be built once and served by CDN, which makes it much faster than having a server render the page on every request.

    You can use Static Generation for many types of pages, including:

    Marketing pages
    Blog posts
    E-commerce product listings
    Help and documentation
    You should ask yourself: "Can I pre-render this page ahead of a user's request?" If the answer is yes, then you should choose Static Generation.

    On the other hand, Static Generation is not a good idea if you cannot pre-render a page ahead of a user's request. Maybe your page shows frequently updated data, and the page content changes on every request.

    In cases like this, you can do one of the following:

    Use Static Generation with Client-side Rendering: You can skip pre-rendering some parts of a page and then use client-side JavaScript to populate them. To learn more about this approach, check out the Data Fetching documentation.
    Use Server-Side Rendering: Next.js pre-renders a page on each request. It will be slower because the page cannot be cached by a CDN, but the pre-rendered page will always be up-to-date. We'll talk about this approach below.

* Server-side Rendering

    If a page uses Server-side Rendering, the page HTML is generated on each request.

    To use Server-side Rendering for a page, you need to export an async function called getServerSideProps. This function will be called by the server on every request.

    For example, suppose that your page needs to pre-render frequently updated data (fetched from an external API). You can write getServerSideProps which fetches this data and passes it to Page like below:

        function Page({ data }) {
        // Render data...
        }

        // This gets called on every request
        export async function getServerSideProps() {
        // Fetch data from external API
        const res = await fetch(`https://.../data`)
        const data = await res.json()

        // Pass data to the page via props
        return { props: { data } }
        }

        export default Page

    As you can see, getServerSideProps is similar to getStaticProps, but the difference is that getServerSideProps is run on every request instead of on build time.

* Summary

    Static Generation (Recommended): 
        The HTML is generated at build time and will be reused on each request. To make a page use Static Generation, either export the page component, or export getStaticProps (and getStaticPaths if necessary). It's great for pages that can be pre-rendered ahead of a user's request. You can also use it with Client-side Rendering to bring in additional data.

    Server-side Rendering: 
        The HTML is generated on each request. To make a page use Server-side Rendering, export getServerSideProps. Because Server-side Rendering results in slower performance than Static Generation, use this only if absolutely necessary.