## Get an update from Fluentd source

```
   git pull upstream master
```


## `s3elasticsearch9`

### Edit `templates/Gemfile.erb`

`templates/Gemfile.erb`: find the `case target` and `s3elasticsearch9` as product of `s3` and `elasticsearch9`

```
+  when 's3elasticsearch9'
+    runtime_deps << %w[libcurl4t64]
+    build_deps << %w[build-essential curl libcurl4-openssl-dev]
   end
```

change `if target = 's3'` to `if target == 's3' || target == 's3elasticsearch9'`

```
-<% if target == "s3" %>
+<% if target == "s3" || target == "s3elasticsearch9" %>
```

### Update the `Makefile`.

Edit:
```
ALL_IMAGES :=  <version>/debian-s3elasticsearch9:<version>-debian-s3elasticsearch9-<subversion>
```

### Create plugins directory

```
mkdir -p plugins/<version>/s3elasticsearch9
touch plugins/<version>/s3elasticsearch9/.gitkeep
```

### Create the `docker-image` directory

```
mkdir -p docker-image/<version>/debian-s3elasticsearch9
echo '*' > docker-image/<version>/debian-s3elasticsearch9/.gitignore
```

## Build the src and new image

```
make src image
```

# Retag and Publish

```
docker tag <image id> 293385631482.dkr.ecr.eu-west-1.amazonaws.com/epimorphics/fluentd-kubernetes-daemonset:<version>-debian-s3elasticsearch9-<subversion>
docker push 293385631482.dkr.ecr.eu-west-1.amazonaws.com/epimorphics/fluentd-kubernetes-daemonset:<version>-debian-s3elasticsearch9-<subversion>
```
