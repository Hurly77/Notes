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

Any folder we nest will become the a route name and will require an `index.js` file



We can create dynamic links like so 

```jsx
// path pages/products/[id].js
import Link from 'next/link'

const ClientsPage = () => {
  const clients = [
    {id: "cam", name: "Cameron"},
    {id: "max", name: "Maximilian"},
    {id: 'ty', name: "Tyler"}
  ]
  return (
    <div>
      <h1>Clients Page</h1>
        <li><Link href="/">Home</Link></li>
      <ul>
        {clients.map(client => <li key={client.id}>
          <Link href={{
          pathname: 'clients/[id]'',
          query: client
          }}>{client.name}</Link>
        </li>)}
      </ul>
    </div>
  )
}

export default ClientsPage
```

## imperative navigation

**Navigation programmatically**

We can also change routes like so 

```jsx
const client = () => {
  const router = useRouter()

  const loadProjectHandler = () => {
    // load data...
    router.push('/clients/max/projecta');
  }
  return (
    <div>
      <h1>{router.query.name}</h1>
      <ul>
        <Link href="/clients"> back </Link>
        <button onClick={loadProjectHandler()}>project</button>
      </ul>
    </div>
  )
}
```

```js
router.push({
pathname: 'path',
query: {id: 'id', clientProject: 'project'}
})
```

## 404

for our  404 page it is quit simple we can just create a `404.js` file on top level of our `pages` directory.

```jsx
//404.js
const notFound = () => {
return(
	<div>
		<h1>Sorry Page Not Found</h1>
	</div>
	)
};
```

### Catch all 

```
//
```

