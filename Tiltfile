# Once upon a time, the below line would have been:
#     local('go generate ./greeter_server')
#     watch_files('helloworld/helloworld.proto')
# and every change to your .proto file OR to your Tiltfile would regenerate your protobufs,
# making everything slower and more annoying.
#
# No longer! This `local_resource` call means that Tilt will generate your protobufs for
# you when your .proto file changes, and only then; and you can see the status of this
# command in your UI along with the rest of your resources.
local_resource('proto', cmd='go generate ./greeter_server', deps=['helloworld/helloworld.proto'])

docker_build('helloworld-server', '.', dockerfile='Dockerfile.server',
             ignore=['./greeter_client', './helloworld/helloworld.proto'],
             live_update=[
                 sync('./greeter_server/', '/app/greeter_server'),
                 sync('./helloworld/helloworld.pb.go', '/app/helloworld/'),
                 run('CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go install /app/greeter_server/...'),
                 run('/app/restart.sh'),
             ])
k8s_resource('helloworld-server', new_name='server')  # rename for UI readability

docker_build('helloworld-client', '.', dockerfile='Dockerfile.client',
             ignore=['./greeter_server', './helloworld/helloworld.proto'],
             live_update=[
                 sync('./greeter_client/', '/app/greeter_client'),
                 sync('./helloworld/helloworld.pb.go', '/app/helloworld/'),
                 run('CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go install /app/greeter_client/...'),
                 run('/app/restart.sh'),
             ])
k8s_resource('helloworld-client', new_name='client')  # rename for UI readability

k8s_yaml(['k8s/server.yaml', 'k8s/client.yaml'])
