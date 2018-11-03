> After https://github.com/debianmaster/openshift-examples/tree/master/istio

```sh
curl -L https://storage.googleapis.com/knative-releases/serving/latest/release-lite.yaml   | sed 's/LoadBalancer/NodePort/'   | oc apply -f -

oc new-project target-proj
oc patch scc/privileged --patch {\"allowedCapabilities\":[\"NET_ADMIN\"]}
oc adm policy add-scc-to-user privileged -z default

```

> serving.yml

```yaml
apiVersion: serving.knative.dev/v1alpha1 # Current version of Knative
kind: Service
metadata:
  name: helloworld-go1 # The name of the app
spec:
  runLatest:
    configuration:
      revisionTemplate:
        spec:
          container:
            image: gcr.io/knative-samples/helloworld-go # The URL to the image of the app
            env:
            - name: TARGET # The environment variable printed out by the sample app
              value: "Go Sample v1"
```

```
oc apply -f serving.yml
```