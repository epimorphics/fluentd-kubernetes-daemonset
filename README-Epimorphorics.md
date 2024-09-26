## Get an update from Fluentd source

```
   git pull upstream master
```

### Resolve conflicts

`templates/Genfile.erb`: find the `case target` and move the `s3` clause to an `if`, eg:

```
<% if target =~/\As3/ %>
gem "aws-sdk-s3", "~> 1.101"
gem "fluent-plugin-s3", "~> 1.7.0"
<% end %>
```

## New Version

From version 1.17 the `plugin epimorphics/fluent-plugin-flatten_filter` is included, so ensure the following is present in `templates/Gemfile.erb` and authentication to Github Ruby Package Registry is required.
```
source "https://rubygems.pkg.github.com/epimorphics" do
  gem "fluent-plugin-flatten_filter", "~> 1"
end
```

### Update the `Makefile`.

Edit:
```
ALL_IMAGES :=  <version>/debian-s3elasticsearch8:<version>-debian-s3elasticsearch8-<subversion>
```

### Create plugins directory

```
 touch plugins/<version>/s3elasticsearch8/.gitkeep
```

## Create the Dockerfile etc

```
PAT=<github personal access token> make src-all
```

## Build the Docker image

```
make image
```

# Retag and Publish

```
docker tag <image id> 293385631482.dkr.ecr.eu-west-1.amazonaws.com/epimorphics/fluentd-kubernetes-daemonset:<version>-debian-s3elasticsearch8-<subversion>
docker push 293385631482.dkr.ecr.eu-west-1.amazonaws.com/epimorphics/fluentd-kubernetes-daemonset:<version>-debian-s3elasticsearch8-<subversion>
```
