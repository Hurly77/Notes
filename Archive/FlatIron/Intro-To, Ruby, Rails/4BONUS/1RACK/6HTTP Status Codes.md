## Why Status Codes are Important for the Client

Status codes allow your server to tell something special to the client, A status code is a **3-digit** integer where the **first digit represents the class of the response**, and the **remaining two** digits represent a **specific status**. There are 5 primary values that the first digit can take.

### Status Code Chart

| Status Number | Code/Description                                             |
| ------------- | ------------------------------------------------------------ |
| 1             | 1xx: Informational (request received and continuing process) |
| 2             | 2xx: Success (request successfully received, understood, and accepted) |
| 3             | 3xx: Redirection (further action must be taken to complete request) |
| 4             | 4xx: Client Error (request contains bad syntax and can't be completed) |
| 5             | 5xx: Server Error (server couldn't complete request)         |

`404`. This means that the server couldn't find the route you requested.

### Status Codes in Rack

Rack sets a status code of `200`. But when a user selects a route that doesn't exist, we need to set the `status` to `404`.

```ruby
class Application
 
  def call(env)
    resp = Rack::Response.new
    req = Rack::Request.new(env)
 
    if req.path=="/songs"
      resp.write "You requested the songs"
    else
      resp.write "Route not found"
      resp.status = 404
    end
 
    resp.finish
  end
end
```

