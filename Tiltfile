local_resource('proto', cmd='go generate ./greeter_server', deps=['helloworld/helloworld.proto'])

docker_build('server', '.', dockerfile='Dockerfile.server')
docker_build('client', '.', dockerfile='Dockerfile.client')

k8s_yaml(['k8s/server.yaml'])
