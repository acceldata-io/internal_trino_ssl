
# Table of Contents

1.  [Overview](#orga142b33)
2.  [Installation](#org5416cc1)
    1.  [Coordinator Setup](#org457f280)
    2.  [Worker Setup](#org47e940d)
    3.  [Trino Setup Script](#org6b7b15d)
    4.  [More Complicated Example](#org336750f)



<a id="orga142b33"></a>

# Overview

This set of scripts can be used to set up Java, a jks certificate, the trino user, and *etc/trino*. You can then proceed to the regular setup for Trino through Ambari, making sure to note on which machines you&rsquo;ve set up the coordinator, and on which you&rsquo;ve set up the workers.

Before proceeding to setup the coordinator and the workers, make sure to read through [2.3](#org6b7b15d) which contains additional configuration options you may want.


<a id="org5416cc1"></a>

# Installation


<a id="org457f280"></a>

## Coordinator Setup

You will need to setup the environment for the Coordinator first in order to get your Coordinator&rsquo;s certificate. This needs to be the same machine that you will setup the Coordinator on through Ambari.

Afterwards, run \`./coordinator -i [path to your ssh key] -H [host]\`
There are optional arguments you can pass:

-   -u [user] &#x2013; this defaults to \`acceldata\`
-   -f [path/to/save/certificate] &#x2013; this defaults to your current working directory

You can pass additional arguments to trino-setup by appending \`&#x2013;\` after your command. These will be explained in [2.4](#org336750f).

Example usage:

    ./coordinator -i ~/.ssh/my_ssh_key -H 10.100.11.25 -u acceldata -f ~/trino-cert.crt


<a id="org47e940d"></a>

## Worker Setup

You will need to have already setup your Coordinator&rsquo;s environment in order to proceed with setting up the workers<sup><a id="fnr.1" class="footref" href="#fn.1" role="doc-backlink">1</a></sup>.

Having done this, run \`./workers -i [path to your ssh key] -H [comma or space separated list of hosts]\`

Much like the coordinator setup, you can specify additional arguments:

-   -u [user] &#x2013; this defaults to \`acceldata\`
-   -f [local/path/to/coordinator/cert] &#x2013; this defaults to \`trino-coordinator\` in your working directory.

Example usage:

    ./workers -i ~/.ssh/my_ssh_key -H 10.100.11.26,my_other_host,10.100.11.72 -f ~/trino-cert.crt -u acceldata


<a id="org6b7b15d"></a>

## Trino Setup Script

This script is what does most of the work in setting up Trino and SSL. The defaults should generally work fine for simple usage, but you may wish to customize some aspects of it.

Options:

-   -p [your jks password] &#x2013; defaults to Hadoop@123
-   -j [jdk home path] &#x2013; This is where JAVA\_HOME will be created on the target machine
-   -c [PATH] &#x2013; This is where the .crt will be created and imported from
-   -C [PATH] &#x2013; This is where the coordinator certificate will be placed
-   -l [URL] &#x2013; Where to download JDK from. This must be a direct url to the tarball
-   -t [name] &#x2013; The trino user name
-   -k [PATH] &#x2013; where to save the jks file on the host
-   -r [password] &#x2013; Java truststore password


<a id="org336750f"></a>

## More Complicated Example

By appending \`&#x2013;\` to your calls to either \`worker\` or \`coordinator\`, you can modify additional options for setting up Trino.

In this example, we specify where JAVA\_HOME will be located as well as the PATH and password for the trino keystore.

    ./coordinator -i ~/.ssh/my_ssh_key -H 10.100.11.25 -u acceldata -- -j /usr/lib/java23 -t my_new_trino_user -k /etc/security/ssl/trino.jks -p your_keystore_password

Anything after the \`&#x2013;\` will be treated as an argument to the \`trino-setup\` script.


# Footnotes

<sup><a id="fn.1" href="#fnr.1">1</a></sup> See \`Coordinator Setup\` for details
