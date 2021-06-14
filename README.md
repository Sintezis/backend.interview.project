# backend.interview.project

## Short description of the task

The goal is to create two services (in different programming languages, chose any two you want)
that will communicate via [protocol buffers](https://developers.google.com/protocol-buffers)

One service will be the server, and another one the client.

The client has to open the connection and send data to the server, then the server will
process the data, and stream output back to the client.

## Task detailed explanation

### First step

* Install and setup grpc protocol buffers: https://grpc.io/docs/protoc-installation/
* Create a proto file that will contain [proto definitions](https://developers.google.com/protocol-buffers/docs/proto3)
* Add a [message](https://developers.google.com/protocol-buffers/docs/proto3#simple) type (named Request) to the proto file:

The request should have the following data structure:
```
Location:
  destination string
  origin string

Request:
  location Location
```

* Add a [message](https://developers.google.com/protocol-buffers/docs/proto3#simple) type (named Response) to the proto file:

The response should have the following data structure:
```
Location:
  destination string
  origin string

Response:
  locations Location[] # List of all locations to go from one point to another
```

* Define service `ItineraryService`, with a method `GetItinerary` method.
* `GetItinerary` [method](https://developers.google.com/protocol-buffers/docs/proto3#services) will accept `Request` message as input parameter, and it will return a [stream](https://grpc.io/docs/languages/go/basics/#server-side-streaming-rpc) of `Response` messages

### Second step

* Initialize `Server Service` in any programing language
* Initialize `Client Service` in any programing language (must be written in a different programing language than `Server Service`)
* `Search Service` will expose [gRPC](https://grpc.io/docs/what-is-grpc/) server implementation of the `ItineraryService`
* `Client Service` must connect to the `Server Service` implementation of `ItineraryService`
* `Client Service` will initialize the `Request` and send it to `Server Service`, then the `Server Service` will stream the data back to the `Client Service`
* `Client Service` will output the data to the console (As soon as one item is received from the stream output it in real-time)

### Fourth step

* The implementation of the `GetItinerary` [method](https://developers.google.com/protocol-buffers/docs/proto3#services) will accept the `Request` as an input parameter. Then it will pass it to another function, let's call it `ConstructItinerary`.
* The function `ConstructItinerary` accepts the `Location` message as an input parameter and returns a list of possible `Locations` (Checkout ConstructItinerary details for more information)
* When `Location` is found stream it back to the `Client Service` and continue with the process
* Push the project to the git repo with simple README explaining how to build the project and use it.

### ConstructItinerary details

This is a simple description of how a `ConstructItinerary` has to work.

`ConstructItinerary` accepts the `Location` message as an input param and based on `destination` and `origin`
has to find out all of the possible combinations of flights to go from specified `origin` to specified `destination`,
and then stream them back to the `Client`.

Use this as a hardcoded list of departure and destination locations to search from

```
[
  ["FRA", "LON"],
  ["AMS", "FRA"],
  ["ZAG", "AMS"],
  ["ZAG", "PAR"],
  ["PAR", "LON"],
  ["BER", "FRA"],
  ["BUH", "BER"],
  ["BUH", "MIL"],
  ["MOW", "OSL"],
  ["STO", "ROM"],
  ["ROM", "PAR"],
  ["AMS", "MOW"],
  ["BER", "LON"],
  ["LAX", "LON"],
  ["NYC", "LAX"],
  ["BER", "ROM"],
]

```

Rules:

* The flights in the same array can go in both directions e.g. let's look at `["FRA", "LON"]`, here the trips can be constructed as FRA to LON and, LON to FRA.
* The same item can't be reused more than one time in one sequence

Example input:

All combinations for origin: `ZAG` and destination: `PAR`

Example output:

1) ZAG, PAR
2) ZAG, AMS, FRA, LON, PAR

### Optional

* Create Dockerfile for building `Server Service`
* Create Dockefile for building `Client Service`
* Create a docker-compose file that will build and run `Server` and `Client Service`
* Create unit tests for `Server Service`
* Use [Grpc-Web](https://github.com/grpc/grpc-web) as a `Client Service` and display data as we receive it. (Example: https://github.com/grpc/grpc-web/tree/master/net/grpc/gateway/examples/helloworld)
* Add cache for request

If anyone is interested in doing more, send me a public key. I will give you access to the single Linux instance where you can try to deploy the app.


## Contributors

Sintezis Team

## Contact

You can reach me at zvonimir.tomesic@sintezis.co.

