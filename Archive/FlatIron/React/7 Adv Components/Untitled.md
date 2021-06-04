https://api.nytimes.com/svc/movies/v2/reviews/all.json?offset=3&order=by-publication-date&api-key=[RjGNDbaNXYxVE68l] HTTP/1.1

GET https://api.nytimes.com/svc/movies/v2/reviews/all.json?offset=3&order=by-publication-date&api-key=[YOUR_API_KEY] HTTP/1.1

Accept: application/json

```js
<!-- Demo code sample. Not indended for production use. -->

<button onclick="execute()">Execute</button>

<script>

// WARNING: fetch is not supported in IE, so it may need a polyfill

function execute() {
  const url = "https://api.nytimes.com/svc/movies/v2/reviews/all.json?offset=3&order=by-publication-date&api-key=6x9NpOeL4UlzFsFJAK7b4w08GM1AfXmM";
  const options = {
    method: "GET",
    headers: {
      "Accept": "application/json"
    },
  };
  fetch(url, options).then(
    response => {
      if (response.ok) {
        return response.text();
      }
      return response.text().then(err => {
        return Promise.reject({
          status: response.status,
          statusText: response.statusText,
          errorMessage: err,
        });
      });
    })
    .then(data => {
      console.log(data);
    })
    .catch(err => {
      console.error(err);
    });
}

</script>

```

