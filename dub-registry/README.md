# doker - dub-registry
docker image for running `dub-registry`.

## about
Uses `bitnami/mongodb` for mongodb

## usage
Running the image
```
docker run --rm -ti -p 9095:9095 -v $DUB_REGISTRY_HOME:/bitnami -v $DUB_REGISTRY_HOME:/dub dub-registry
```

This will run both `mongodb` and `dub-registry` while persisting the database in the `$DUB_REGISTRY_HOME` location. The registry is accessible at http://127.0.0.1:9095/

To run it as a daemon and make it auto-restart use
```
docker run -d --restart=always -ti -p 9095:9095 -v $DUB_REGISTRY_HOME:/bitnami -v $DUB_REGISTRY_HOME:/dub dub-registry
```

`dub-registry` can be configured by adding the `settings.json` file in `$DUB_REGISTRY_HOME` folder.
