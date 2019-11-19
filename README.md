# Open Management Portal - Backend

> The API for the Open Management Portal.

## Configuration

The following environment variables are available:

| Name | Example Value | Required |
|------|---------------|----------|
| MP_JWT_VERIFY_PUBLICKEY_LOCATION | http://[your-cluster-internal-sso-service-name]:8080/auth/realms/[your-realm-id]/protocol/openid-connect/certs | True |

## Deployment

This project includes an `openshift-applier` inventory. To use it, make sure that you are logged in to the cluster and that you customize the variables in `.applier/inventory/group_vars/all.yml`. 

The `BuildConfig` included in this project also requires an image pull secret with permission to pull from `registry.redhat.io`. By default, the `BuildConfig` expects it to be called `builder-image-pull-secret`. You can create it on the command-line using `oc` like this:

```bash
oc create secret docker-registry builder-image-pull-secret --docker-server=registry.redhat.io --docker-username=[your-username] --docker-password=[your-password] --docker-email=[anything]
```

Alternatively, you can uncomment the secret in `inventory/group_vars/all.yml` and define the registry credentials in this inventory. If you do this, _do not push the inventory to a public git repository_.

Once these are configured, you can deploy the project with:

```bash
$ cd .applier/

$ ansible-galaxy install -r requirements.yml --roles-path=roles --force

$ ansible-playbook apply.yml -i inventory/
```

## Components

This project was built using Quarkus.

## Testing

This project runs unit tests using an alternate application profile, which disables JWT token verification and user role verification. This means that all endpoints are available without returning 401 or 403, but the user context is not available during tests.

## Useful Commands

``` bash

# serve with hot reload at localhost:8080
$ mvn quarkus:dev

# run unit tests
$ mvn test

# build for production
$ mvn quarkus:build

```
