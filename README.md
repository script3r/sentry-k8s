# Sentry on Kubernetes

## Quickstart

First and foremost, shout out to the people at `sentry.io` for their project. I recommend forking and helping the community.

### Custom Image

If you want to customize specific settings for your installation, build a custom `sentry` image by modifying the files `config.yml`, `Dockerfile` and `sentry.conf.py` in the `build` directory.

Then, proceed to upload the custom image to your repository of choice, as following:

```bash
REPOSITORY=some-repo/your-sentry make build push
```

If you don't want to build a custom image, you may use `script3r/sentry-k8s`.

### Prereqs

You'll need to setup a `PostgreSQL` database with a user and database designated for `sentry`.

You will also want to run the `sentry` migrations on it. For more details see https://docs.sentry.io/server/installation/docker/. 

### Deploy to Kubernetes


Modify the secrets file to contain the actual secrets used in the project. Make sure they're base64 encoded. For example, if your database name and database user are `sentry`, then your secrets file should contain:

```yaml
dbName: c2VudHJ5
dbUser: c2VudHJ5
```

To deploy to Kubernetes, simply type:

```bash
kubectl apply -f deploy/k8s/
```

Notice that this will create a namespace named `sentry`. Confirm the machines are up by typing:

```bash
kubectl get pods -nsentry
```

You should see images for `web`, `worker` and `cron`.

Enjoy! Your `sentry` is now exposed as a service `sentry-web-service` listening on port 80. It is recommended to front this with a TLS/SSL enabled proxy.


### TLS Notes

Notice that by default, this setup script enables TLS/SSL by setting the environment variable `SENTRY_USE_SSL` to `1` in `20web.yml`.

If you want to disable TLS (don't do it!), you may set this environment variable to `0`.
