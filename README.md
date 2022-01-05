# Run .NET WebApp on OpenShift

## Pre-req

- Download and install [.NET 6 SDK](https://dotnet.microsoft.com/en-us/download/dotnet)
- Install VS Code

## Create .NET 6 WebApp project

```bash
# check dotnet version
dotnet --version

# Generate a WebApp project
# dotnet new webApp -o myWebApp --no-https

# Clone project
git clone https://github.com/xdevops-caj-dotnet/myWebApp.git

# Launch the WebApp
cd myWebApp
dotnet run

```

## Deploy .NET WebApp to OpenShift

```bash

export GUID=will

oc new-project ${GUID}-dotnet-demo

# Ensure 'dotnet:6.0-ubi8' builder image stream tag exist
# Import builder image if necessary, please refer to troubleshooting
oc get imagestreamtag  -n openshift | grep 'dotnet:6.0-ubi8'

# Create application by 'dotnet:6.0-ubi8' builder image stream tag
oc new-app dotnet:6.0-ubi8~https://github.com/xdevops-caj-dotnet/myWebApp.git --name my-web-app

# Check build logs
oc logs -f bc/my-web-app

# Expose service
oc get svc
oc expose svc my-web-app

# Check route of the WebApp
oc get route

```

Access the WebApp by browser, Example: <http://my-web-app-will-dotnet-demo.apps.${CLUSTER_DOMAIN}>


## Troubleshooting

### Import .NET 6.0 images into OpenShift

```bash
# Import `ubi8/dotnet-60:6.0` as `openshift/dotnet:6.0-ubi8` image stream
oc import-image dotnet:6.0-ubi8 \
  --from=registry.access.redhat.com/ubi8/dotnet-60:6.0 \
  --confirm \
  -n openshift

```
References:

- [.NET 6.0 SDK and Runtime](https://catalog.redhat.com/software/containers/ubi8/dotnet-60/6182efb9be25a74c00923849?container-tabs=overview)


## References:

- [Run .Net code on Red Hat OpenShift Container Platform on Mac OS](https://cloud.redhat.com/blog/run-.net-code-on-red-hat-openshift-container-platform-on-mac-os)
- [.NET Core Sample App for OpenShift](https://github.com/redhat-developer/s2i-dotnetcore-ex)
- [Red Hat .NET container images](https://catalog.redhat.com/software/containers/search?q=dotnet&p=1)