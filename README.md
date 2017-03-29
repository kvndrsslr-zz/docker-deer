# Supported tags and respective `Dockerfile` links

* `latest` [(Dockerfile)](https://raw.githubusercontent.com/AKSW/docker-deer/master/Dockerfile)

# What is DEER?

DEER is the **RDF Data Extraction and Enrichment Framework**. It is open source and available on [GitHub](https://github.com/SLIPO-EU/DEER)  

# How to use this image

The volume `/volume/data` needs to be bound to a directory on your hosts FS containing a configuration file for DEER and possibly other files referenced by the configuration. DEER will output files in the same directory. In this example, the volume is bound to `/path/to/working/dir`. We use the example configuration file from the official repository, which you can get with

`$ cd /path/to/working/dir`
`$ wget https://raw.githubusercontent.com/SLIPO-EU/DEER/master/datasets/1/config.ttl `

Now that your configuration file exists in `/path/to/working/dir/config.ttl` we can run DEER:

`$ docker run -i --name deer -v /path/to/working/dir:/volume/data --rm aksw/deer ./config.ttl`

By default, we assume a configuration file in the root of the mounted volume named `config.ttl`, so in this case we can leave out the last argument:

`$ docker run -i --name deer -v /path/to/working/dir:/volume/data --rm aksw/deer`
 
After DEER terminates, its output files are written to `/path/to/working/dir`.


