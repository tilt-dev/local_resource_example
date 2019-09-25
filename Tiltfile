local_resource('proto', cmd='go generate ./greeter_server', deps=['helloworld/helloworld.proto'])

docker_build('helloworld-server', '.', dockerfile='Dockerfile.server',
             ignore="./greeter_client")
docker_build('helloworld-client', '.', dockerfile='Dockerfile.client',
             ignore="./greeter_server")

k8s_yaml(['k8s/server.yaml'])
