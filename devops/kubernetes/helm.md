# Helm

Helm is the Kubernetes package manager. Helm leans into the nautical metaphors for Kubernetes. So packages are referred to as "charts".

## A Look at Charts

After installing Helm you can create your own chart with the command `helm create <chart_name>`. This will create a directory with the chart name, and some boilerplate configuration. The directory should contain the following:

- `Chart.yaml`
- `charts/`
- `templates/`
- `values.yaml`

The templates directory is also populated with some boilerplate. Of most interest is the following:

- `NOTES.txt` This file will display text to the user's terminal when they install your chart using Helm.
- `deployment.yaml` The kubernetes manifest
- `service.yaml` A basic manifest for creating a service endpoint
- `_helpers.tpl` helpers to be used by other templates

## Working With Templates

Templates are useful for extending the logic of yaml. So for example consider the following config:

```yaml
metadata:
  name: myhelmchart-configmap
```

This can be transformed to something that isn't hard coded so each release version can have it's own unique name.

```yaml
metadata:
  name: {{ .Release.Name }}-configmap
```

The dot `.` within the brackets signals a namespace object, and each dot separates these objects. The first dot is the top-most namespace for the scope.

### Testing a Template Config

You can pass the parameters `--debug --dry-run` when running `helm install` to test your template rendering without actually commiting to the install. Note that this is not a guarantee that you have generated a valid Kubernetes configuration, it is just for sanity checks to make sure your template is rendering as intended.
