# What's in this repo?
This repo is a simple client+server that exchange greetings. They communicate via [protocol buffers](https://developers.google.com/protocol-buffers)
generated from `helloworld.proto`.

The purpose of this repo is to show off [our new `local_resource` functionality](https://docs.tilt.dev/local_resource.html). In this Tiltfile, we call:
```python
local_resource('proto', cmd='go generate ./greeter_server', deps=['helloworld/helloworld.proto'])
```
This call tells Tilt: "when `helloworld/helloworld.proto` changes, run `go generate ./greeter_server`". That is,
Tilt generates your protobufs for you automatically, only when the relevant file changes. Even better, you can
see output and status of this command in the UI along with the rest of your resources.

## Give it a shot!

Pull down this repo, `tilt up`, and once everything's up, make a change to `helloworld.proto`--say, add a field to
the `HelloRequest`. Your Local Resource will pick up this change and regenerate `helloworld.pb.go`, and the client and
server will both pick up this file change via Live Update and propagate it to their running containers. ⚡️ 

### Dependencies
1. Install [the Go toolchain](https://golang.org/doc/install)

2. Install [protobuf compiler](https://github.com/google/protobuf/blob/master/README.md#protocol-compiler-installation)

3. Install the protoc Go plugin

   ```
   $ go get -u github.com/golang/protobuf/protoc-gen-go
   ```

4. [Install Tilt](https://docs.tilt.dev/install.html)

5. `tilt up`
