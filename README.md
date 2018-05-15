# S2i: Node.js + Nginx
=====================================================

Build node application & run as deployed on nginx server. Suitable for application require npm & node build packages and deployed over a nginx http server.

Node Version:

* [NodeJS 8](8)
* [NodeJS 10](10)


Nginx Version 1.8


## Test Application

#### Image

```console 
$ docker pull akoserwal/nodejs-nginx-s2i
```
#### Build sample angular

make sure your build directory is `dist`(change angular.json)
 ` "outputPath": "dist" `

```console 
$ s2i build test/test-app/ nodejs-nginx-s2i sample-app 
```
#### Run

```console 
$ docker run -p 8080:8080 sample-app
```
