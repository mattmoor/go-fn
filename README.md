# go-fn

This buildpack composes a collection of Go function buildpacks with the
Paketo Go order buildpack to create a new order.

# Build this buildpack

This buildpack can be built (from the root of the repo) with:

```shell
pack package-buildpack my-buildpack --config ./package.toml
```

# Use this buildpack

```shell
# This uses HEAD, to use a locally build image (e.g. my-buildpack)
# replace the ghcr.io reference below.
pack build blah --buildpack ghcr.io/mattmoor/go-fn:main
```

# Sample output (http)

```
===> DETECTING
[detector] ======== Output: io.mattmoor.cloudevents.golang.functions@0.0.1 ========
"fmt" => "fmt"
"http" => "net/http"
Input arg: {ImportPath:net/http Name:ResponseWriter Pointer:false}
Input arg: {ImportPath:net/http Name:Request Pointer:true}
Checking function signature: "func(v2.Event)"
Checking function signature: "func(v2.Event)protocol.Result"
Checking function signature: "func(v2.Event)error"
Checking function signature: "func(context.Context, v2.Event)"
Checking function signature: "func(context.Context, v2.Event)protocol.Result"
Checking function signature: "func(context.Context, v2.Event)error"
Checking function signature: "func(v2.Event)*v2.Event"
Checking function signature: "func(v2.Event) (*v2.Event, protocol.Result)"
Checking function signature: "func(v2.Event) (*v2.Event, error)"
Checking function signature: "func(context.Context, v2.Event)*v2.Event"
Checking function signature: "func(context.Context, v2.Event) (*v2.Event, protocol.Result)"
Checking function signature: "func(context.Context, v2.Event) (*v2.Event, error)"
unable to find function "Receiver" in "github.com/paketo-buildpacks/samples/go/mod" with matching signature
err:  io.mattmoor.cloudevents.golang.functions@0.0.1 (1)
4 of 5 buildpacks participating
io.mattmoor.http.golang.functions 0.0.3
paketo-buildpacks/go-dist         0.2.5
paketo-buildpacks/go-mod-vendor   0.0.169
paketo-buildpacks/go-build        0.1.2
===> ANALYZING
[analyzer] Warning: Not restoring cached layer metadata, no cache flag specified.
[analyzer] Skipping buildpack layer analysis
===> RESTORING
Skipping 'restore' due to clearing cache
===> BUILDING
[builder] HTTP Go Function 0.0.3
  Package:  github.com/paketo-buildpacks/samples/go/mod
  Function: Handler

[builder] Paketo Go Distribution Buildpack 0.2.5
  Resolving Go version
    Candidate version sources (in priority order):
      go.mod    -> "1.15.*"
      <unknown> -> ""

[builder]     Selected Go version (using go.mod): 1.15.5

  Executing build process
    Installing Go 1.15.5
[builder]       Completed in 7.037s

[builder] Paketo Go Mod Vendor Buildpack 0.0.169
  Checking module graph
    Running 'go mod graph'
[builder]       Completed in 111ms

  Executing build process
    Running 'go mod vendor'
[builder]       Completed in 1.475s

[builder] Paketo Go Build Buildpack 0.1.2
[builder]   Executing build process
    Running 'go build -o /layers/paketo-buildpacks_go-build/targets/bin -buildmode pie ./http-cmd/function'
[builder]       Completed in 10.931s

[builder]   Assigning launch processes
    web: /layers/paketo-buildpacks_go-build/targets/bin/function
===> EXPORTING
[exporter] Reusing layer 'paketo-buildpacks/go-build:targets'
[exporter] Reusing 1/1 app layer(s)
[exporter] Reusing layer 'launcher'
[exporter] Reusing layer 'config'
[exporter] Reusing layer 'process-types'
[exporter] Adding label 'io.buildpacks.lifecycle.metadata'
[exporter] Adding label 'io.buildpacks.build.metadata'
Adding label 'io.buildpacks.project.metadata'
[exporter] Setting default process type 'web'
[exporter] *** Images (b678cfdbaf8d):
      test-container
[exporter] Adding cache layer 'paketo-buildpacks/go-dist:go'
[exporter] Adding cache layer 'paketo-buildpacks/go-mod-vendor:mod-cache'
[exporter] Adding cache layer 'paketo-buildpacks/go-build:gocache'
Successfully built image test-container
```

# Sample output (cloudevents)

```
===> DETECTING
[detector] ======== Output: io.mattmoor.http.golang.functions@0.0.3 ========
"cloudevents" => "github.com/cloudevents/sdk-go/v2"
Input arg: {ImportPath:github.com/cloudevents/sdk-go/v2 Name:Event Pointer:false}
Output arg: {ImportPath:github.com/cloudevents/sdk-go/v2 Name:Event Pointer:true}
Output arg: {ImportPath: Name:error Pointer:false}
Checking function signature: "func(http.ResponseWriter, *http.Request)"
unable to find function "Handler" in "github.com/paketo-buildpacks/samples/go/mod" with matching signature
err:  io.mattmoor.http.golang.functions@0.0.3 (1)
4 of 5 buildpacks participating
io.mattmoor.cloudevents.golang.functions 0.0.1
paketo-buildpacks/go-dist                0.2.5
paketo-buildpacks/go-mod-vendor          0.0.169
paketo-buildpacks/go-build               0.1.2
===> ANALYZING
[analyzer] Warning: Not restoring cached layer metadata, no cache flag specified.
[analyzer] Skipping buildpack layer analysis
===> RESTORING
Skipping 'restore' due to clearing cache
===> BUILDING
[builder] CloudEvents Go Function 0.0.1
  Package:  github.com/paketo-buildpacks/samples/go/mod
  Function: Receiver
  Protocol: http

[builder] Paketo Go Distribution Buildpack 0.2.5
  Resolving Go version
    Candidate version sources (in priority order):
      go.mod    -> "1.15.*"
      <unknown> -> ""

[builder]     Selected Go version (using go.mod): 1.15.5

  Executing build process
[builder]     Installing Go 1.15.5
[builder]       Completed in 5.531s

[builder] Paketo Go Mod Vendor Buildpack 0.0.169
  Checking module graph
    Running 'go mod graph'
[builder]       Completed in 95ms

  Executing build process
    Running 'go mod vendor'
[builder]       Completed in 511ms

[builder] Paketo Go Build Buildpack 0.1.2
[builder]   Executing build process
    Running 'go build -o /layers/paketo-buildpacks_go-build/targets/bin -buildmode pie ./ce-cmd/function'
[builder]       Completed in 10.675s

[builder]   Assigning launch processes
    web: /layers/paketo-buildpacks_go-build/targets/bin/function
===> EXPORTING
[exporter] Adding layer 'paketo-buildpacks/go-build:targets'
[exporter] Reusing 1/1 app layer(s)
[exporter] Reusing layer 'launcher'
[exporter] Adding layer 'config'
[exporter] Reusing layer 'process-types'
[exporter] Adding label 'io.buildpacks.lifecycle.metadata'
[exporter] Adding label 'io.buildpacks.build.metadata'
Adding label 'io.buildpacks.project.metadata'
Setting default process type 'web'
[exporter] *** Images (3ff7b9658e2d):
      test-container
[exporter] Adding cache layer 'paketo-buildpacks/go-dist:go'
[exporter] Adding cache layer 'paketo-buildpacks/go-mod-vendor:mod-cache'
[exporter] Adding cache layer 'paketo-buildpacks/go-build:gocache'
Successfully built image test-container
```
