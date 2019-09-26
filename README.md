# What's in this repo?
This repo is a simple client+server that exchange greetings. They communicate via [protocol buffers](https://developers.google.com/protocol-buffers)
generated from `helloworld.proto`.

The purpose of this repo is to show off [our new `local_resource` functionality](https://docs.tilt.dev/api.html#api.local_resource). Once upon a time, you might have written the following line in your Tiltfile:
```python
local('go generate ./greeter_server')
watch_file('helloworld/helloworld.proto')
```
This would regenerate your protobufs every time you change `helloworld.proto` -- but it would also re-execute your entire Tiltfile. More annoyingly, it would also regenerate your protobufs whenever your Tiltfile re-executed, whether they needed regenerating or not. This solution was hacky, resulted in commands running at unexpected times, and generally made things slow and annoying.

The alternative was running `go generate ./greeter_server` by hand whenever you changed your `.proto` file.
Your Tiltfile execution was faster, your proto generation was predictable, but you had to remember to run
the command when you changed a certain file, which shook you out of your workflow (assuming that you remembered at all,
and didn't spend multiple minutes wondering why something on your server wasn't working).

Local Resource is here to help! In this Tiltfile, we call:
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
