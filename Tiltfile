SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='dktapdemo.azurecr.io/mydevworkloads/spring-petclinic-workload-mydev-ns')
LOCAL_PATH = '/Users/dkhopade/src/spring-petclinic'
NAMESPACE = os.getenv("NAMESPACE", default='mydev-ns')
OUTPUT_TO_NULL_COMMAND = os.getenv("OUTPUT_TO_NULL_COMMAND", default=' > /dev/null ')

k8s_custom_deploy(
  'workload-name',
  apply_cmd="tanzu apps workload apply -f workload.yaml --debug --live-update" +
    " --local-path " + LOCAL_PATH +
    " --source-image " + SOURCE_IMAGE +
    " --namespace " + NAMESPACE +
    " --yes " +
    OUTPUT_TO_NULL_COMMAND + 
    " && kubectl get workload spring-petclinic-workload --namespace " + NAMESPACE + " -o yaml",
  delete_cmd="tanzu apps workload delete -f workload.yaml --namespace " + NAMESPACE + " --yes" ,
  deps=['pom.xml', './target/classes'],
  container_selector='workload',
  live_update=[
    sync('./target/classes', '/workspace/BOOT-INF/classes')
  ]
)

k8s_resource('workload-name', port_forwards=["8080:8080"],
  extra_pod_selectors=[{'carto.run/workload-name': 'workload-name', 'app.kubernetes.io/component':'run'}])

allow_k8s_contexts('tap-demo')