# How File-based Routing Works

In` React `we Create a file and then export that file which is then later imported and Passed to a `Route `component from the `react-router-dom`. 

NextJS instead uses our the file structure from inside our pages folder to generate routes for us. 

```
/pages
	index.js === https://domaiName.com/
	about.js === https://domainName.com/about
	/products
		index.js => https://domainName.com/products
		[id].js => https://domainName.com/products/:id
	
```

Another really cool feature to Next.js is that we can use subfolder for more organization like so

```
pages/
	about/
		index.js => https://domainName.com/about
	products/
		index.js = domain.com/products
```

We can create dynamic links like so 

```jsx
// path pages/products/[id].js
import link from 'next/link'
	
const products = () =>{
 const clients = {{name: max, id: 1}, {name: "cam", id: 2}}
 return (
   <div>
   	<h1>Product List</h1>
 	     <ul>
          	{clients.map((client)=> {
              <li key={client.id}>
              	<Link href={{}}>
                	
                </Link>   
              </li>
             })}
          </ul>
      </div>
  )
}
```

