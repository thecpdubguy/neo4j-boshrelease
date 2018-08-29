BOSH release to run neo4j
=======================

Background
----------

### What is neo4j?

Neo4j is the world’s leading Graph Database. It is a high performance graph store with all the features expected of a mature and robust database, like a friendly query language and ACID transactions.

Usage
-----

To use this bosh release, first upload it to your bosh:

```
bosh upload release https://github.com/hybris/neo4j-boshrelease
```

For [bosh-lite](https://github.com/cloudfoundry/bosh-lite), you can quickly create a deployment manifest & deploy a cluster:

```
templates/make_manifest warden
bosh -n deploy
```

For AWS EC2, create a single VM:

```
templates/make_manifest aws-ec2
bosh -n deploy
```

### Override security groups

For AWS & Openstack, the default deployment assumes there is a `default` security group. If you wish to use a different security group(s) then you can pass in additional configuration when running `make_manifest` above.

Create a file `my-networking.yml`:

```yaml
---
networks:
  - name: neo4j1
    type: dynamic
    cloud_properties:
      security_groups:
        - neo4j
```

Where `- neo4j` means you wish to use an existing security group called `neo4j`.

You now suffix this file path to the `make_manifest` command:

```
templates/make_manifest openstack-nova my-networking.yml
bosh -n deploy
```


## Manually creating the release (to a tarball)
https://github.com/hybris/java-boshpackage

To be able to manually build a release for this fork, we had to do the following:
- Init and update the java submodule
- Delete the blob for 1.7 `bosh remove-blob java/openjdk-jre-trusty-1.7.0_65.tar.gz`
- Grab the tarball for 1.8 `wget https://download.run.pivotal.io/openjdk/trusty/x86_64/openjdk-1.8.0_141.tar.gz -O openjdk-jre-trusty-1.8.0_141.tar.gz`
- Add the local 1.8 tarball as a blob `bosh add-blob temp/openjdk-jre-trusty-1.8.0_141.tar.gz java/openjdk-jre-trusty-1.8.0_141.tar.gz`
- create the release `bosh create-release --tarball=../tileReleaseTarballs/neo4j-releaseNoJava7.tgz`








