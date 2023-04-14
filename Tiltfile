SOURCE_IMAGE = os.getenv("SOURCE_IMAGE", default='ghcr.io/thomasvitale/configuration-service-source')
LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='default')

k8s_custom_deploy(
    'configuration-service',
    apply_cmd="carto apps workload apply -f config/workload.yml --update-strategy replace --debug --live-update" +
               " --local-path " + LOCAL_PATH +
               " --source-image " + SOURCE_IMAGE +
               " --namespace " + NAMESPACE +
               " --yes --output yaml",
    delete_cmd="carto apps workload delete -f config/workload.yml --namespace " + NAMESPACE + " --yes",
    deps = ['build.gradle', './bin/main'],
    container_selector='workload',
    live_update = [
        sync('./bin/main', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('configuration-service', port_forwards=["8888:8888"],
            extra_pod_selectors=[{'carto.run/workload-name': 'configuration-service', 'app.kubernetes.io/component': 'run'}])
