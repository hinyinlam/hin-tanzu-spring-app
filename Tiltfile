SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='github.com/hinyinlam/hin-tanzu-spring-app')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
   'hin-tanzu-spring-app',
   apply_cmd="tanzu apps workload apply -f PATH-TO-WORKLOAD-YAMl --live-update" +
       " --local-path " + LOCAL_PATH +
       " --source-image " + SOURCE_IMAGE +
       " --namespace " + NAMESPACE +
       " --yes >/dev/null" +
       " && kubectl get workload APP-NAME --namespace " + NAMESPACE + " -o yaml",
   delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes" ,
   deps=['pom.xml', './target/classes'],
   container_selector='workload',
   live_update=[
       sync('./target/classes', '/workspace/BOOT-INF/classes')
   ]
)

k8s_resource('hin-tanzu-spring-app', port_forwards=["8080:8080"],
   extra_pod_selectors=[{'carto.run/workload-name': 'hin-tanzu-spring-app', 'app.kubernetes.io/component': 'run'}])
allow_k8s_contexts('CONTEXT-NAME')