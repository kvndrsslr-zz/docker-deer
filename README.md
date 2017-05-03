# Supported tags and respective `Dockerfile` links

* `latest` [(Dockerfile)](https://raw.githubusercontent.com/AKSW/docker-deer/master/Dockerfile)
* `server` [(Dockerfile)](https://raw.githubusercontent.com/AKSW/docker-deer/server/Dockerfile)

# What is DEER?

DEER is the **RDF Data Extraction and Enrichment Framework**. It is open source and available on [GitHub](https://github.com/SLIPO-EU/DEER)  

# How to use this image

## Standard Build with Volume Mount

The volume `/volume/data` needs to be bound to a directory on your hosts FS containing a configuration file for DEER and possibly other files referenced by the configuration. DEER will output files in the same directory. In this example, the volume is bound to `/path/to/working/dir`. We use the example configuration file from the official repository, which you can get with

`$ cd /path/to/working/dir`
`$ wget https://raw.githubusercontent.com/SLIPO-EU/DEER/master/datasets/1/config.ttl `

Now that your configuration file exists in `/path/to/working/dir/config.ttl` we can run DEER:

`$ docker run -i --name deer -v /path/to/working/dir:/volume/data --rm aksw/deer ./config.ttl`

By default, we assume a configuration file in the root of the mounted volume named `config.ttl`, so in this case we can leave out the last argument:

`$ docker run -i --name deer -v /path/to/working/dir:/volume/data --rm aksw/deer`
 
After DEER terminates, its output files are written to `/path/to/working/dir`.

## Server Build

Map the private port 4567 to a public port on your host and you are good to go.

`$ docker run -d --restart=always -p 4567:8080 aksw/deer:server`

You can now submit a config file and will receive an unique job id:

`$ wget https://raw.githubusercontent.com/SLIPO-EU/DEER/master/datasets/1/config.ttl `
`$ curl --form "config_file=@./config.ttl" localhost:8080/submit`
`123456`

Once the job execution finishes, you can retrieve the output files that were declared in your configuration as follows:

`curl localhost:8080/:jobid/:filename`

In our example this would be:

`curl localhost:8080/123456/output.ttl`


