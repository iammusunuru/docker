# docker-container

Three providers in this plugin:

* ResourceModelSource: Lists the containers as nodes
* NodeExecutor: Execute a command in a container via Command step or Commands page.
* WorkflowNodeSteps:
  * exec
  * inspect  
  * kill
  * pause
  * unpause
  * stats

## To build and install

Run the following commands to install the plugins:

    zip -r docker-container.zip docker-container
    cp docker-container.zip $RDECK_BASE/libext


## Resource Model Mapping and Tags

### Mapping attributes from default values

With the "Custom Mapping" option, you can use the default attributes getting from `docker inspect` to generate node attributes on Rundeck. 

The default mapping values are:

```
'docker:Id': Container ID,
'docker:Created': Created Date,
'docker:Name': Container Name,
'docker:Image': Container Image ID,
'docker:State.Status': Container Statis,
'docker:State.Pid':  Container PID,
'docker:State.StartedAt': Container Started At,
'docker:Config.Image': Container Image Name,
'docker:Config.Hostname': Container Hostname,
'docker:Config.Cmd': Container Command,
'docker:Config.Labels': Container Labels,
'docker:IPAddress': Container IP,
```

By default, these values will be mapped to the `docker` group attributes on nodes. The nodename and hostname will be taken from `docker:Id` and `docker:Config.Hostname`. 

To change these values you can use something like this:

```
nodename.selector=docker:Name,hostname.selector=docker:IPAddress
```

This will define the `nodename` attribute with the value of `docker:Name` and `hostname`  attribute with the value of `docker:IPAddress`


### Mapping attributes from `docker inspect` result.

You can mapping values directly from the result of the `docker inspect` command.

For example:

```
StartedAt.selector=State.StartedAt,FinishedAt.selector=State.FinishedAt
```

This will add the attributes `StartedAt` and `FinishedAt` on the node definition.



### Generating custom tags.

With the "Tag" option, you can generate tags based on the value of the default parameters, eg:

`tag.selector=docker:Config.Image`

This example will add a tag with the value of the parameter `Config.Image`
